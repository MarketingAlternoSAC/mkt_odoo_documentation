# Fotocheck

La clase `Photocheck` es el núcleo del módulo. Hereda de `mail.thread` y `mail.activity.mixin` para proporcionar capacidades de mensajería y seguimiento de actividades.

Esta clase gestiona el ciclo de vida de un fotocheck, desde la recolección de datos del colaborador hasta el procesamiento de su fotografía y la aprobación final.

*   **Modelo Odoo:** `photocheck`
*   **Herencia:** `mail.thread`, `mail.activity.mixin`
*   **Orden por defecto:** `id desc`
