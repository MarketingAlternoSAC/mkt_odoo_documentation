## ^^Generales^^

### `create(self, vals)`

Decorador: `@api.model`

- Si `vals['name']` viene como `'New'`:
    - Reemplaza `name` con el valor de la secuencia `budget.modify`.
- Llama al `super` para crear el registro.

**Efecto:**  
Garantiza que todas las modificaciones tengan un código único autogenerado.

### `modify_executive_budget(self)`

- Para cada presupuesto en `budget_ids`:
    - Asigna `budget.executive_id = self.new_executive`.
- Cambia `state` a `'modified'`.

**Efecto:**  
Todos los presupuestos seleccionados pasan a tener un nuevo ejecutivo.

### `revert_executive_modify(self)`

- Condiciones: solo actúa si:
    - `modify_type == 'executive'` y
    - `state == 'modified'`.
- Crea un nuevo registro `budget.modify` con:
    - La descripción `"Revertion of "`.
    - Tipo de modificación `executive`.
    - Invierte `old_executive` y `new_executive` (lo que era nuevo pasa a viejo y viceversa).
    - Copia los `budget_ids` de la modificación original.
- Marca la modificación original como revertida: `is_reverted = True`.
- Llama a `new_modify.modify_executive_budget()` para aplicar de inmediato la reversión.

**Efecto:**  
Los presupuestos listados en `budget_ids` vuelven a tener como `executive_id` al ejecutivo original.

### `modify_responsible_budget(self)`

- Para cada registro `rec`:
    - Para cada presupuesto en `rec.budget_ids`:
        - Asigna `budget.responsible_id = rec.new_responsible`.
    - Marca `rec.state = 'modified'`.

**Efecto:**  
Todos los presupuestos seleccionados pasan a tener un nuevo responsable.

### `revert_responsible_modify(self)`

- Para cada `rec`:
    - Si `state == 'modified'` y existen `new_responsible` y `old_responsible`:
        - Crea un nuevo `budget.modify` con:
            - Descripción: `"Revertion of "`.
            - `modify_type`: se mantiene el mismo tipo original.
            - `old_responsible`: toma el nuevo responsable de la modificación original.
            - `new_responsible`: toma el responsable anterior.
            - `partner_id`: copia el cliente si existe.
            - `budget_ids`: copia los mismos presupuestos (con comando M2M `(4, id)`).
        - Marca el registro original como `is_reverted = True`.
        - Llama a `new_modify.modify_responsible_budget()` para aplicar la reversión.

**Efecto:**  
Los presupuestos recuperan el responsable anterior, quedando trazada la reversión en un nuevo registro `budget.modify`.

### `modify_responsible_revision(self)`

- Para cada presupuesto en `budget_ids`:
    - Asigna `budget.responsible_revision = self.executive_revision`.
- Cambia `state` a `'modified'`.

**Efecto:**  
Marca o desmarca un flag de revisión (`responsible_revision`) en los presupuestos, según el valor de `executive_revision`.

### `modify_settlement_between_budget(self)`

- Recorre las líneas en `self.old_budget_id.budget_line_ids`.
- Para cada línea `settlement` donde `settlement.remove` sea `True`:
    - Arma un diccionario `values` con los campos principales de la línea:
        - `budget_id`
        - `documental_settlement_id`
        - `date`
        - `document_type`
        - `document_file`
        - `document_filename`
        - `document`
        - `reason`
        - `amount`
        - `settlement_name`
        - `settlement_detail_id`
    - Crea una nueva línea de presupuesto en `self.new_budget_id.budget_line_ids` con esos valores.
    - Obtiene:
        - `settle = settlement.documental_settlement_id` (liquidación documental).
        - `rq_settle = settle.requirement_id` (requerimiento asociado).
    - Actualiza tanto la liquidación como el requerimiento para apuntar al nuevo presupuesto:
        - `settle.budget_id = new_budget_id`
        - `settle.cost_center_id`, `settle.campaign_id` se alinean con los del nuevo presupuesto.
        - `rq_settle.budget_id`, `rq_settle.cost_center_id`, `rq_settle.campaign_id` se alinean también con el nuevo presupuesto.
        - `rq_settle.partner_id` y `rq_settle.year_month_id` se copian desde el nuevo presupuesto.
    - Finalmente, elimina la línea original: `settlement.unlink()`.
- Al terminar, marca el registro de modificación como `state = 'modified'`.

**Efecto:**

- Las líneas marcadas con `remove = True` se mueven del presupuesto antiguo al nuevo.
- Las liquidaciones (`documental.settlements`) y sus requerimientos quedan re-asignadas al nuevo presupuesto y su configuración (centro de costo, campaña, cliente, periodo).
- Las líneas originales en el presupuesto viejo se eliminan.

---

## ^^OnChange^^

### `_onchange_modify_type(self)`

- Si `modify_type == 'executive'`:
    - Limpia `old_budget_id` y `new_budget_id`.
- Si `modify_type == 'move_btwn_budget'`:
    - Limpia `old_executive` y `new_executive`.
- Para otros tipos:
    - Limpia todos: `old_budget_id`, `new_budget_id`, `old_executive`, `new_executive`.

**Efecto:**  
Evita que se mezclen parámetros de distintos tipos de modificación. Los campos de ejecutivo no se usan cuando el tipo es "mover entre presupuestos", y viceversa.

### `onchange_budgets(self)`

Decorador: `@api.onchange('old_executive', 'modify_type', 'revision_executive_id')`

- Si `old_executive` está definido:
    - Busca todos los presupuestos con `executive_id = old_executive` y asigna sus ids en `budget_ids`.
- Si `revision_executive_id` está definido:
    - Hace lo mismo pero usando `revision_executive_id` como filtro.

**Efecto:**  
Al seleccionar un ejecutivo (viejo o de revisión), se rellena automáticamente la lista de presupuestos sobre los que se trabajará.

### `_onchange_responsible_customer_fill_budgets(self)`

Decorador: `@api.onchange('old_responsible', 'partner_id')`

- Para cada registro:
    - Siempre limpia `budget_ids` primero.
    - Si no hay `old_responsible`, no hace nada más.
    - Arma un dominio base: presupuestos con `responsible_id = old_responsible`.
    - Si además hay `partner_id`, agrega al dominio `('partner_id', '=', partner_id)`.
    - Busca los presupuestos que cumplen ese dominio y asigna sus ids a `budget_ids`.

**Efecto:**  
Al elegir un responsable (y opcionalmente un cliente), se llenan automáticamente los presupuestos que están a cargo de ese responsable y cliente.

---

## ^^Computadas^^

### `compute_budget_executives(self)`

Decorador: `@api.depends('description', 'modify_type', 'budget_ids', 'old_executive')`

- Si hay datos en `description`, `modify_type`, `budget_ids` o `old_executive`:
    - Busca todos los presupuestos con `executive_id` no vacío.
    - Toma los usuarios `executive_id` y los asigna a `current_executive_ids`.
- En caso contrario:
    - Limpia `current_executive_ids`.

**Efecto:**  
Define el conjunto de ejecutivos disponibles para el dominio de `old_executive`.

### `compute_budget_responsibles(self)`

Decorador: `@api.depends('description', 'modify_type', 'budget_ids', 'old_responsible')`

- Obtiene todos los presupuestos con `responsible_id` definido.
- Mapea esos `responsible_id` a un conjunto de usuarios.
- Para cada registro:
    - Asigna ese conjunto a `current_responsible_ids`.
    - Si no hubiera ninguno, asigna todos los usuarios (`self.env['res.users']`) como fallback.

**Efecto:**  
Define el conjunto de usuarios que se usan en el dominio de `old_responsible`, basados en quienes ya son responsables de algún presupuesto.
