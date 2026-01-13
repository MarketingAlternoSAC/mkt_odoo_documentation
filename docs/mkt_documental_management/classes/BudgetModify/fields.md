### Campos principales

| Campo         | Tipo / Relación | Descripción                                                                                                                              |
| ------------- | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `name`        | Char            | Código interno de la modificación; se llena automáticamente con una secuencia (`budget.modify`) al crear el registro (si está en `New`). |
| `description` | Char            | Descripción breve de la modificación (obligatoria).                                                                                      |
| `modify_type` | Selection       | Tipo de modificación (ver opciones abajo).                                                                                               |
| `budget_ids`  | Many2many       | Lista de presupuestos afectados por la modificación.                                                                                     |
| `state`       | Selection       | Estado del registro: `draft` (borrador) o `modified` (acción ejecutada).                                                                 |
| `is_reverted` | Boolean         | Indica si esta modificación ya fue revertida mediante un registro nuevo de tipo "reversión".                                             |

#### Opciones de `modify_type`

| Valor                | Descripción                                               |
| -------------------- | --------------------------------------------------------- |
| `executive`          | Cambio de ejecutivo de los presupuestos.                  |
| `responsible`        | Cambio de responsable de los presupuestos.                |
| `move_btwn_budget`   | Mover liquidaciones/detalles entre presupuestos.          |
| `executive_revision` | Marca/revisión ejecutiva especial sobre los presupuestos. |

---

## Campos relacionados con EJECUTIVO

| Campo                   | Tipo / Relación         | Descripción                                                                                                              |
| ----------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `old_executive`         | Many2one (`res.users`)  | Usuario que actualmente es ejecutivo de los presupuestos a modificar (dominio limitado por `current_executive_ids`).     |
| `new_executive`         | Many2one (`res.users`)  | Nuevo ejecutivo al que se asignarán los presupuestos.                                                                    |
| `current_executive_ids` | Many2many (`res.users`) | Conjunto de usuarios que ya figuran como `executive_id` en algún presupuesto; se calcula en [`compute_budget_executives`](functions.md#compute_budget_executivesself). |
| `revision_executive_id` | Many2one (`res.users`)  | Ejecutivo con el que se filtran presupuestos cuando se hace una "revisión ejecutiva".                                    |
| `executive_revision`    | Boolean                 | Marca booleana que se usa para activar el flag `responsible_revision` en los presupuestos.                               |

---

## Campos relacionados con PRESUPUESTO origen/destino

| Campo                  | Tipo / Relación     | Descripción                                                                                                      |
| ---------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `old_budget_id`        | Many2one (`budget`) | Presupuesto origen desde donde se van a mover líneas/liquidaciones.                                              |
| `budget_line_ids`      | One2many (related)  | Líneas de presupuesto (campo relacionado con `old_budget_id.budget_line_ids`), usadas como origen de movimiento. |
| `lock_budget_line_ids` | One2many (related)  | Otro conjunto de líneas relacionadas (`old_budget_id.line_ids`), usado para control/visualización.               |
| `new_budget_id`        | Many2one (`budget`) | Presupuesto destino al cual se moverán las liquidaciones/detalles.                                               |

---

## Campos relacionados con RESPONSABLE

| Campo                     | Tipo / Relación          | Descripción                                                                                                               |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| `partner_id`              | Many2one (`res.partner`) | Cliente asociado usado para filtrar presupuestos en combinación con el responsable (opcional).                            |
| `old_responsible`         | Many2one (`res.users`)   | Responsable actual de los presupuestos (dominio limitado por `current_responsible_ids`).                                  |
| `new_responsible`         | Many2one (`res.users`)   | Nuevo responsable a asignar a los presupuestos.                                                                           |
| `current_responsible_ids` | Many2many (`res.users`)  | Conjunto de usuarios que figuran como `responsible_id` en algún presupuesto; se calcula en [`compute_budget_responsibles`](functions.md#compute_budget_responsiblesself). |
