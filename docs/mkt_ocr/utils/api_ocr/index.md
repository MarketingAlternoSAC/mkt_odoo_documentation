### Descripción del módulo

**Archivo técnico:** `api_ocr.py`

Este archivo contiene un conjunto de funciones utilitarias (helpers) destinadas a facilitar la comunicación con el servidor externo de OCR y el procesamiento de los datos recibidos. A diferencia de las clases ubicadas en `models`, estas funciones no interactúan directamente con la base de datos de Odoo mediante el ORM, sino que se encargan de la lógica de transformación de datos, manejo de peticiones HTTP y limpieza de cadenas de texto.

### Uso principal

- **Comunicación HTTP:** Encapsula y gestiona de forma segura las solicitudes POST hacia la API del motor de OCR.
- **Estandarización de Fechas:** Asegura que los distintos formatos de fecha (textuales o numéricos, con separadores inusuales) extraídos del documento sean compatibles con el estándar exigido por los campos `Date` de Odoo (`YYYY-MM-DD`).
- **Limpieza de Identificadores:** Da formato a números de comprobantes (como facturas o boletas) para asegurar que mantengan una estructura coherente y unificada (ej. `0001-00000001`) al ser importados a la plataforma.
