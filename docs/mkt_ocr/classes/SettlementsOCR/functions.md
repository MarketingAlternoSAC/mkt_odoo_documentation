## ^^Generales^^

### `action_get_ocr_data(self, base64_file, filename)`

Decorador: `@api.model`

- **Validación:** Comprueba la existencia del archivo en formato base64. Si no hay archivo, retorna un diccionario vacío.
- **Decodificación:** Convierte el archivo base64 enviado desde el frontend en bytes legibles por el motor.
- **Llamada a la API:** Envía el archivo y su nombre al servicio externo a través de la función utilitaria `ocr_consultation`.
- **Manejo de Errores:** Si el proceso devuelve un estado `"error"` o falla la conexión, captura la excepción, abre un nuevo cursor de base de datos (`new_cr`) para asegurar que el registro del error se guarde independientemente de un rollback, y crea una entrada en `ocr.usage.log` (incluyendo el modelo actual, el ID, y el detalle del error). Posteriormente levanta un `UserError` para notificar a la interfaz.
- **Mapeo de Datos:** Si el proceso es exitoso, actualiza el campo `is_ocr = True` y recopila variables en el diccionario `vals`:
    - Formatea la `fecha` mediante la utilidad `date_format`.
    - Asigna el RUC o DNI (`documento_cliente`) al campo `dni_ruc` o `partner`.
    - Formatea el `numero_factura` usando `formatInvoiceNumber` para estandarizar la entrada.
    - Captura los importes numéricos (`total` a `settle_amount`, `igv` a `settle_igv`).
    - Traduce la `moneda_pago` (PEN, USD, EUR) a las opciones del sistema ('soles', 'dolares', 'euros').

**Efecto:**  
Retorna un diccionario `vals` de campos al frontend. El formulario de la liquidación se autocompleta dinámicamente con los datos extraídos de la factura o boleta, reduciendo la carga manual, o en su defecto, detiene el proceso y notifica fallas, asegurando la trazabilidad del error.
