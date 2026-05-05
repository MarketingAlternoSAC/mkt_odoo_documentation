### Descripción de la clase

**Modelo técnico:** `settlement` (Extensión)

Este modelo hereda del modelo existente `settlement` para agregar la funcionalidad fundamental de procesamiento de Reconocimiento Óptico de Caracteres (OCR). Su propósito es ser el puente entre los documentos adjuntos en las liquidaciones y el motor de OCR.

### Funcionalidades Clave

- **Activación Inteligente:** El botón de OCR solo se hace visible y operable cuando la liquidación alcanza un estado específico ("En Espera de Pago").
- **Orquestación del Flujo OCR:**
  1. **Extracción y Compresión:** Extrae los documentos adjuntos (imágenes/PDF) y los convierte a una cadena codificada en Base64.
  2. **Llamada al Servicio Externo:** Se comunica con un servicio externo (API) para procesar el archivo.
  3. **Integración de Datos:** Al recibir una respuesta exitosa del servicio, el método mapea automáticamente los datos extraídos (como RUC, DNI, fecha, importes y moneda) a los campos correspondientes en la liquidación.
- **Gestión de Fallos:** Si el servicio de OCR no logra extraer la información correctamente, el sistema registra automáticamente el incidente en el modelo `ocr.usage.log` para su posterior análisis.
