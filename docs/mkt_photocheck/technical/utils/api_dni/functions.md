# Funciones: API DNI

## `apiperu_dni(dni)` {#apiperu-dni}

Función externa que se conecta con el servicio de APIPerú para obtener datos de identidad oficiales a partir de un número de DNI. Actúa como un puente de validación de datos para asegurar que los nombres y apellidos registrados coincidan con el documento de identidad

**Configuración de Conexión:**

*   **Endpoint:** Utiliza la URL de producción de apiperu.dev.
*   **Autenticación:** Emplea un Bearer Token en las cabeceras (headers) para validar el acceso al servicio.
*   **Formato:** Define que tanto el envío como la recepción de datos se realicen en formato JSON.

**Proceso de Petición (Request):**

*   Convierte el DNI recibido en un string y lo empaqueta en un objeto JSON.
*   Realiza una petición de tipo POST.

**Nota Técnica:** Se observa el uso de `verify=False` para omitir la validación de certificados SSL, lo cual facilita la conexión en entornos de desarrollo con proxies o restricciones de red.

**Manejo de Respuesta:**

*   **Éxito (Status 200):** Desglosa la respuesta JSON para extraer el objeto `data`. Retorna una tupla con cuatro valores específicos: `nombre_completo`, `nombres`, `apellido_paterno` y `apellido_materno`.
*   **Error de Servicio:** Si el código de estado no es 200, imprime el error y el cuerpo de la respuesta para depuración.
*   **Excepción de Red:** Captura errores de conexión (`RequestException`) para evitar que el sistema se detenga abruptamente si el servicio externo no está disponible.

**Retorno:**

*   Una tupla con la información de identidad si la consulta es exitosa.
*   En caso de error, la función no retorna valores explícitos (retorna `None`), lo cual debe ser manejado por el método que la invoque.

---