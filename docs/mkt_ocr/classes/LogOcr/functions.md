## ^^Generales^^

### `action_submit(self)`

- **Validación de usuario:** Comprueba si el usuario actual (`self.env.user`) ya ha utilizado la funcionalidad OCR consultando el campo `used_ocr`.
- **Actualización de estado:** Si el usuario no ha usado OCR antes, el método escribe (`write`) `True` en el campo `used_ocr`. Se utiliza `sudo()` para garantizar que la escritura ocurra independientemente de las restricciones de seguridad que pueda tener el usuario sobre su propio perfil de Odoo.
- **Cierre del Wizard:** Retorna una acción de ventana `ir.actions.act_window_close` para cerrar el modal de la encuesta de satisfacción.

**Efecto:**  
Permite recolectar la evaluación del usuario en la base de datos sin interrumpir su flujo de trabajo, cerrando automáticamente el formulario y registrando que el usuario ya conoce/utilizó el sistema OCR.
