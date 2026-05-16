# Campos: Photocheck

Listado de campos principales definidos en el modelo `photocheck`.

| Nombre | Tipo | Descripción |
| :--- | :--- | :--- |
| `name` | Char | Identificador del fotocheck (ej. secuencia o "New"). |
| `dni` | Char | Número de documento del colaborador. Requerido. |
| `first_name` | Char | Nombres del colaborador (autocompletado por API DNI). |
| `last_name` | Char | Apellidos del colaborador (autocompletado por API DNI). |
| `complete_name` | Char | Nombre completo del colaborador. |
| `initial_name` | Char | Primer nombre calculado en [`compute_initial_name()`](functions.md#compute-initial-name). |
| `state` | Selection | Estado actual: `draft`, `to_do`, `done`, `refused`. |
| `state_duplicate` | Selection | Indica si es un registro nuevo o duplicado (`new`, `duplicate`). |
| `photo_raw` | Image | Fotografía original cargada. |
| `photo_name` | Char | Nombre de la fotografía calculado en [`compute_photo_name()`](functions.md#compute-photo-name). |
| `photo` | Image | Fotografía procesada (sin fondo). |
| `photocheck_brand_group_id` | Many2one | Grupo de marcas asociado. |
| `brand_ids` | Many2many | Marcas asociadas calculado en [`compute_brands()`](functions.md#compute-brands). |
| `brand_counter` | Integer | Cantidad de marcas asociadas calculado en [`compute_brand_counter()`](functions.md#compute-brand-counter). |
| `job_id` | Many2one | Cargo del colaborador. |
| `city_id` | Many2one | Ciudad del colaborador. |
| `user_id` | Many2one | Usuario de Odoo que gestiona el registro. |
| `photocheck_supervisor_id` | Many2one | Supervisor asignado. |
| `active` | Boolean | Indica si el registro está activo. |
| `note` | Text | Notas adicionales. |
| `request_date` | Datetime | Fecha de envío a procesamiento. |
| `done_date` | Datetime | Fecha de finalización. |
| `refused_date` | Datetime | Fecha de rechazo. |
