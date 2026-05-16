**Modelo técnico:** `ocr.response`

### Uso Prinicpal

- **Registro de Usos:** Servir como un registro historico para poder auditor el uso del OCR. Registrando las cada respuesta obtenida del OCR.
- **Gestión de Errores:** Almacena los mensajes de error originados en fallas de OCR para un posterior análisis técnico.
- **Trazabilidad de Rendimiento:** Guarda los tiempos de respuesta de la API externa para identificar posibles cuellos de botella.
- **Identificacion de Confusiones:** Almacenar las respuestas obtenidas del OCR permite identificar en que documentos se confundio el modelo, lo que facilita un posterior análisis técnico y entrenamiento del modelo de OCR.