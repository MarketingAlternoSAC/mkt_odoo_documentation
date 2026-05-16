### Campos principales

| Campo | Tipo / Relación | Descripción |
|-------|-----------------|-------------|
| `id_user` | Char | ID del usuario que ejecutó la acción de OCR. Por defecto guarda el ID del entorno actual (`self.env.user.id`). |
| `company_id` | Char | ID de la compañía asociada a la sesión del usuario (`self.env.company.id`). |
| `comment` | Text | Comentarios adicionales que el usuario puede ingresar al momento de completar la encuesta de satisfacción. |
| `res_model` | Char | Modelo relacionado desde donde se originó la llamada al OCR (por ejemplo, `settlement`). |
| `res_id` | Integer | ID del registro específico que invocó el OCR. Permite enlazar el log con el documento original. |
| `satisfaction` | Selection | Puntuación brindada por el usuario al resultado del OCR. Opciones: `0` (Malo), `1` (Regular), `2` (Bueno). |
| `status` | Char | Estado actual del procesamiento o lectura (ej. 'Pendiente', 'Error en OCR', 'Rechazado Control Interno'). |
| `file_refuse_CI` | Boolean | Bandera que indica si el documento procesado por OCR fue posteriormente rechazado en el paso de "Control Interno". |