**Modelo técnico:** `documental.requirements` (Extensión)

Esta clase hereda del modelo existente de requerimientos documentales (`documental.requirements`). Su objetivo es intervenir en el proceso de revisión contable, específicamente en el paso de "Control Interno".

### Uso principal

- **Auditoría de Documentos Rechazados:** Sincroniza el estado de las liquidaciones rechazadas con el registro de uso de OCR (`ocr.usage.log`). Esto permite a los administradores saber cuántas de las facturas leídas por OCR fueron posteriormente rechazadas por el departamento contable (por ejemplo, debido a errores de lectura en importes u omisiones), estableciendo una métrica de precisión y "falsos positivos".
