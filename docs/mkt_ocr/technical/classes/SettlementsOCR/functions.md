## ^^Generales^^

### `action_get_ocr_data(self, base64_file, filename)`

Decorador: `@api.model`

- **Validación:** Comprueba si se ha proporcionado un archivo en formato base64. Si no existe, retorna un diccionario vacío.
- **Procesamiento y Trazabilidad:**
    - **Decodificación:** Convierte la cadena base64 en contenido binario.
    - **Consulta OCR:** Envía el archivo al servicio externo mediante [ocr_consultation](../../utils/api_ocr/functions.md#ocr-consultation).
    - **Persistencia de Logs (Nuevo Cursor):** Utiliza un cursor de base de datos independiente (`self.env.registry.cursor()`) para crear un registro en el modelo [ocr.response](../ResponseOCR/index.md). Esto garantiza que el log se guarde incluso si la transacción principal falla o se revierte (rollback), capturando el usuario, tiempo de respuesta, modelo y las respuestas cruda y procesada.
    - **Gestión de Adjuntos:** Crea un `ir.attachment` vinculado al log de `ocr.response` para almacenar el documento procesado, permitiendo auditorías posteriores.
- **Manejo de Errores:** Captura excepciones o fallos reportados por el servicio, registra el error en el log persistente con estado "Error en OCR" y levanta un `UserError` para notificar al frontend.
- **Mapeo de Datos:** En caso de éxito, marca `used_document_ocr = True` e incluye metadatos de la operación (`ocr_time`, `ocr_status`, `ocr_error`) en el diccionario `vals`. Luego mapea los campos extraídos:
    - **Fecha:** Formateada mediante [date_format](../../utils/api_ocr/functions.md#date-format).
    - **Identificación:** Asigna el RUC o DNI al campo `dni_ruc` si está disponible. Si no hay documento pero sí se extrajo el `nombre`, lo asigna al campo `partner`.
    - **Documento:** Estandariza el número de factura/boleta con [formatInvoiceNumber](../../utils/api_ocr/functions.md#format-invoice-number).
    - **Importes:** Asigna el `total` a `settle_amount` e `igv` a `settle_igv`.
    - **Moneda:** Traduce la `moneda_pago` (ej. 'PEN', 'USD', 'S/', '$') a las opciones internas del sistema ('soles', 'dolares', 'euros') usando un mapa de equivalencias.

**Efecto:**  
Retorna un diccionario `vals` de campos al frontend. El formulario de la liquidación se autocompleta dinámicamente con los datos extraídos de la factura o boleta, reduciendo la carga manual, o en su defecto, detiene el proceso y notifica fallas, asegurando la trazabilidad del error.