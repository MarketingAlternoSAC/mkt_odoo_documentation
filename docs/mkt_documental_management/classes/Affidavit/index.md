### Descripción de la clase

**Modelo técnico:** `affidavit`

Esta clase representa una **declaración jurada** vinculada a un empleado. Permite registrar información clave como el monto declarado, moneda, ubicación, concepto y las firmas de las partes involucradas.

### Uso principal

- Registro formal de declaraciones juradas de empleados.
- Soporte para impresión, reportes y visualización desde portal.

### Integraciones técnicas

- **`portal.mixin`**  
    Permite que el registro sea accesible desde el portal de usuario.
- **`mail.thread` / `mail.activity.mixin`**  
    Habilita seguimiento (chatter) y actividades asociadas al registro.

### Comportamiento por defecto

- **Ordenación:** registros más recientes primero (`id DESC`).
