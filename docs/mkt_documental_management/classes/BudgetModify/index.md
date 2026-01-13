### Descripción de la clase

**Modelo técnico:** `budget.modify`

Esta clase permite gestionar modificaciones sobre presupuestos (budget) en tres frentes principales:

- Cambio de ejecutivo de uno o varios presupuestos.
- Movimiento de liquidaciones/documentos entre presupuestos.
- Cambio de responsable de los presupuestos (con posibilidad de revertir).

### Integraciones técnicas

- **`mail.thread`**  
    Permite que las modificaciones queden trazadas con seguimiento (chatter).
- **`mail.activity.mixin`**  
    Habilita la gestión de actividades asociadas al registro.

### Comportamiento por defecto

- **Ordenación:** registros más recientes primero (`id DESC`).
