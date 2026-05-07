## ^^Generales^^

### `ocr_consultation(env, file_content, filename="document.pdf")` {#ocr-consultation}

- **Configuración Dinámica:** Consulta los parámetros del sistema (`ir.config_parameter`) de Odoo (`mkt_ocr_plugin.url_ocr_server` y `mkt_ocr_plugin.path_ocr_server`) usando el entorno (`env`) en modo `sudo()` para armar dinámicamente la URL del servidor OCR, evitando configuraciones rígidas (hardcoded).
- **Preparación de la Petición:** Determina el tipo MIME del archivo usando la librería `mimetypes` y encapsula el contenido binario en memoria (`io.BytesIO`) para enviarlo mediante una petición `requests.post`.
- **Manejo de Respuesta Exitosa (`200 OK`):**
    - Deserializa el JSON retornado por el servicio OCR y mapea los resultados clave: `nombre_proveedor`, `igv`, `subtotal`, `precio` (total), `fecha`, `numero_factura`, `documento_proveedor`, etc.
    - Captura el tiempo de respuesta del servidor y marca el `status` de la operación como `"success"`.
- **Manejo de Errores:** 
    - Si el status code es diferente de 200, lee el mensaje de error directamente desde el JSON de respuesta y retorna un `status` de `"error"`.
    - Si ocurre una excepción técnica (ej. timeout o servidor OCR inalcanzable), se captura en el bloque `except` y se genera un mensaje de error detallado, devolviendo `"error"`.

**Efecto:**  
Actúa como cliente integral de la API del OCR, retornando una tupla con todos los valores extraídos o procesados, dejándolos listos para ser asimilados e insertados en la base de datos por el modelo `settlement`.

---

## ^^Transformación de Datos^^

### `date_format(fecha_str)` {#date-format}

- **Diccionario de Meses:** Emplea un mapa predefinido (`meses_map`) para traducir los nombres de los meses en español (ej. "enero", "feb") a sus correspondientes valores numéricos del 1 al 12.
- **Limpieza de Cadena:** Convierte el texto extraído del OCR a minúsculas, elimina caracteres conflictivos como puntos y limpia los espacios residuales.
- **Extracción mediante Expresión Regular (Regex):** 
    - Aplica un patrón robusto `r'(\d{1,2})[\s\->|/]*(?:de\s*)?([a-z]+)[\s\->|/]*(?:del\s*|de\s*)?[\s\->|/]*(\d{4})'` para identificar fechas textuales con ruido o separadores atípicos (ej. "25 de Febrero del 2024", "12>ene/2023").
    - Transforma las coincidencias en un objeto `datetime` de Python y luego formatea la salida al string estándar `%Y-%m-%d`.
- **Respaldo Numérico:** Si el formato no contiene texto (meses con letras), intenta parsear la fecha directamente probando iterativamente los formatos `%d-%m-%Y` y `%Y-%m-%d`.

**Efecto:**  
Garantiza que fechas ilegibles, caracteres extraños o formatos poco comunes detectados por el motor de visión artificial no detengan el proceso ni provoquen errores al guardar la fecha en Odoo.

### `formatInvoiceNumber(numero)` {#format-invoice-number}

- **Validación de Serie:** Evalúa si el número de comprobante contiene el separador estándar (`-`). Si no lo tiene, retorna el número original sin alteración.
- **Formateo de Secciones:**
    - Divide la cadena utilizando el guion como delimitador.
    - A la primera sección (la serie) le aplica un `ljust(4, "0")` para asegurar un ancho constante.
    - A la última sección (el correlativo) le aplica un `zfill(8)` para rellenar con ceros a la izquierda hasta asegurar una longitud estricta de 8 dígitos numéricos.
- **Ensamblaje:** Concatena la serie formateada y el correlativo nuevamente con un guion intermedio.

**Efecto:**  
Estandariza visual y técnicamente los números de factura (ej. "F01-123" se transforma en "F010-00000123"), manteniendo la uniformidad, pulcritud y el orden en los registros contables dentro del sistema.
