# Módulo de OCR

Este módulo integra funcionalidades de Reconocimiento Óptico de Caracteres (OCR) dentro de Odoo 17, permitiendo extraer automáticamente datos de documentos (como PDF o imágenes) para rellenar campos en el sistema. 

Además, incluye un sistema de registro (log) y encuestas de satisfacción para medir el rendimiento y uso del OCR.

## Características Principales

*   **Extracción Automática:** Procesamiento de facturas y recibos para extraer datos como RUC, fecha, monto, etc.
*   **Integración con API Externa:** Uso de un servicio de OCR externo para el análisis de documentos.
*   **Validación y Registro:** Logs de cada intento de procesamiento para auditar la fiabilidad de la extracción.
*   **Encuestas de Satisfacción:** Interfaz para que el usuario califique el resultado del OCR.

---

Para más detalles, consulta las siguientes secciones:

*   [Guía Funcional](functional/index.md): Para usuarios finales.
*   [Guía Técnica](technical/index.md): Para desarrolladores y administradores de sistema.
