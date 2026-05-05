### Descripción de la clase

**Modelo técnico:** `ocr.usage.log`

Un modelo nuevo e independiente diseñado para llevar una auditoría completa sobre el uso, métricas de rendimiento y niveles de satisfacción de la herramienta OCR. Este modelo actúa como el núcleo de estadísticas y retroalimentación para el área de desarrollo.

### Uso principal

- **Trazabilidad de Rendimiento:** Guarda los tiempos de respuesta de la API externa para identificar posibles cuellos de botella.
- **Gestión de Errores:** Almacena los mensajes de error originados en fallas de OCR para un posterior análisis técnico.
- **Recopilación de Feedback:** Guarda la valoración del usuario (encuesta de satisfacción) respecto a los resultados del OCR.
- **Auditoría de Control Interno:** Indica si una liquidación que usó OCR fue rechazada en la fase de auditoría financiera, permitiendo correlacionar fallas de lectura con rechazos documentales.
