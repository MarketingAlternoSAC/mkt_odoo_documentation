## ^^Generales^^

### `settlement_refuse_intern_control(self)`

- **Herencia:** Llama al método original `super().settlement_refuse_intern_control()` para preservar la funcionalidad estándar de rechazo en Control Interno.
- **Búsqueda de Logs:** Para cada registro actual, busca si existe un log asociado en el modelo `ocr.usage.log` (filtrando por `res_model` y `res_id`).
- **Actualización o Creación:**
    - Si encuentra un log asociado, actualiza el estado (`status`) a `"Rechazado Control Interno"` y enciende la bandera `file_refuse_CI = True`. También actualiza los identificadores del usuario y la compañía actuales.
    - Si no existe un log, crea uno de forma reactiva indicando que el archivo fue rechazado en Control Interno, garantizando que el evento quede documentado incluso si el OCR original no generó un log o el usuario lo evadió.

**Efecto:**  
Mantiene un control estadístico preciso sobre la fiabilidad del OCR. Si un documento pasa el OCR pero es rechazado en contabilidad, el log se actualiza para reflejar esto, proporcionando métricas de precisión en entornos reales.
