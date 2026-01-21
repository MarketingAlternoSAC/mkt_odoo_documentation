### Descripción de la clase

**Modelo técnico:** `cost.center`

Esta clase representa un **Centro de Costo**. Permite gestionar centros de costos y su vinculación con entidades clave como la razón social, provincia y marca.

### Uso principal

- Administración de centros de costos para segmentación contable o de gestión.
- Trazabilidad mediante integración con el chatter.

### Integraciones técnicas

- **`mail.thread` / `mail.activity.mixin`**  
    Habilita seguimiento (chatter) y actividades asociadas al registro para auditar cambios en campos clave.

### Comportamiento por defecto

- **Representación:** Se utiliza el campo `code` (Cost center number) como nombre representativo (`_rec_name`).
