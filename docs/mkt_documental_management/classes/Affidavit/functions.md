## ^^Generales^^

### `create(self, vals)`

Decorador: `@api.model`

- Si `vals['name']` no está definido o es `'New'`:
    - Reemplaza name usando la secuencia `ir.sequence` con código `'Affidavit'`.
    - Llama al método `super()` para crear el registro.

**Efecto:**  
Garantiza que toda declaración jurada tenga un código único generado por secuencia en lugar de quedar como “New”.

### `preview_affidavit_document(self)`

- Asegura que el método se ejecute sobre un único registro (`ensure_one`).
- Retorna una acción `ir.actions.act_url` que abre la URL del portal asociada al registro.
- Utiliza `get_portal_url()` proporcionado por `portal.mixin`.

**Efecto:**  
Permite previsualizar la declaración jurada desde el backend usando la misma URL que se emplea en el portal.

### `get_default_country(self)`

Función auxiliar definida fuera de la clase.

- Busca el país con código `'PE'`.
- Retorna su identificador.

**Efecto:**  
Hace que las nuevas declaraciones juradas se creen, por defecto, con el país Perú asignado en `country_id`.

---

## ^^Computadas^^

### `_compute_access_url(self)`

- Extiende la lógica base de `portal.mixin` mediante `super()`.
- Sobrescribe el campo `access_url` para cada registro con el patrón:
    - `my/affidavits/<id>`

**Efecto:**  
Define explícitamente la ruta del portal donde el usuario puede visualizar la declaración jurada.

### `_compute_date_month(self)` {#compute-date-month}

Decorador: `@api.depends('date')`

- Para cada registro:
    - Si `date` está definido:
        - Usa `format_date` con locale `es_PE` para obtener el nombre del mes.
        - Almacena el valor con la primera letra en mayúscula en `date_month`.
    - Si no hay fecha:
        - Deja el campo vacío.

**Efecto:**  
Mantiene una representación textual del mes de la fecha, útil para reportes e impresiones sin recomputar en cada uso.

---

## ^^OnChange^^

### `_onchange_currency(self)`

Decorador: `@api.onchange('amount_currency_type')`

- Se ejecuta cuando cambia el campo `amount_currency_type`.
- Si el valor es:
    - `'soles'` → asigna la moneda con `name = 'PEN'`.
    - `'dolares'` → asigna la moneda con `name = 'USD'`.

**Efecto:**  
Actualiza automáticamente `currency_id` para que el monto se exprese en la moneda correcta según la selección del usuario.
