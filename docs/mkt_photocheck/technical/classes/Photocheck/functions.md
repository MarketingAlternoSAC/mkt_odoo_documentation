# Funciones: Photocheck

Lógica de negocio y métodos principales de la clase `Photocheck`.

## Métodos de Procesamiento

### `modify_image(self)` {#modify-image}

Este método se encarga de procesar la imagen almacenada en el campo `photo`, eliminando el fondo de forma automática y actualizando el registro con la nueva versión procesada en formato Base64.

- **Decodificación y Preparación:** Convierte la cadena Base64 de self.photo en datos binarios brutos (bytes).
    Utiliza la librería `PIL` (Pillow) para abrir la imagen y luego la transforma en un `NumPy array`, formato necesario para el procesamiento computacional.

- **Procesamiento de Remover Fondo:** Obtiene una sesión de procesamiento mediante `get_rembg_session()`. Ejecuta la función `rembg.remove()`, la cual utiliza modelos de aprendizaje profundo para identificar el sujeto principal y eliminar el fondo de la imagen.

- **Conversión de Salida:** Transforma el array resultante (ya sin fondo) de nuevo a un objeto de imagen PIL.
Guarda la imagen procesada en un flujo de bytes utilizando el formato `PNG` (para preservar la transparencia del canal alfa generado tras remover el fondo).

- **Actualización del Registro:** Codifica los bytes de la imagen final nuevamente a Base64 y actualiza el atributo `self.photo`, permitiendo que la imagen procesada persista en la base de datos o el modelo.

### `onchange_complete_name(self)`

Este método actúa como un disparador automático (onchange) que se ejecuta cada vez que el usuario modifica el campo `dni`.

- **Llamada al Servicio de DNI:**
    Invoca a la función [`apiperu_dni`](/mkt_photocheck/technical/utils/api_dni/functions/), pasándole el valor del DNI ingresado en el campo self.dni.

- **Actualización de Datos:**
    Recibe como respuesta los datos personales (nombres y apellidos) desde el servicio de DNI.
    
- **Sincroniza esta información con campos específicos del registro actual:**
    - **complete_name:** Combina el primer nombre, apellido paterno y apellido materno en una sola cadena.
    - **first_name:** Asigna el primer nombre recibido.
    - **last_name:** Asigna los apellidos (paterno y materno concatenados).

## Lógica de Workflow

### `button_to_do(self)`
Cambia el estado a `to_do`, registra la fecha de solicitud y envía el correo de notificación.

### `button_done(self)`
Cambia el estado a `done`, registra la fecha de finalización y envía el correo de confirmación.

### `button_refused(self)`
Cambia el estado a `refused`, registra la fecha de rechazo y envía el correo de notificación de rechazo.

## Seguridad y Acceso

### `search(self, args, offset=0, limit=None, order=None)`
Sobrescribe el método de búsqueda estándar de Odoo para aplicar reglas de visibilidad dinámicas basadas en el perfil del usuario. Actúa como un filtro de seguridad a nivel de base de datos para restringir qué registros puede ver cada usuario.

- **Acceso Total (Administrador):** Verifica si el usuario pertenece al grupo photocheck_admin. De ser así, se omite cualquier restricción y se ejecuta la búsqueda estándar (super), permitiendo el acceso a todos los registros.

- **Dominio Base (Autenticación Propia):** Define un criterio de visibilidad mínima. El usuario siempre puede ver registros donde:
    - Él sea el creador (`create_uid`).
    - Esté asignado como supervisor (`photocheck_supervisor_id`).
    - Sea el usuario directamente relacionado con el registro (`user_id`).

- **Responsabilidades por Grupo y Ciudad:** El método consulta el modelo `photocheck.brand.group.responsible` para identificar áreas adicionales de responsabilidad:
    - **Filtrado por Ciudad:** Si el usuario tiene ciudades asignadas para un grupo de marcas, solo verá registros de ese grupo dentro de esas ciudades específicas.
    - **Filtrado por Grupo:** Si no tiene ciudades específicas, ve todos los registros pertenecientes a ese grupo de marcas.

- **Construcción Dinámica del Dominio:** Utiliza lógica para combinar múltiples criterios con operadores OR (|) y AND (&).
    - Concatena los subdominios de responsabilidad con el dominio base, asegurando que el filtro final sea una suma lógica de todos los permisos del usuario.
- **Ejecución Final:** Retorna el resultado de la búsqueda original de Odoo, pero inyectando los filtros de seguridad calculados en el argumento args.

### `_get_brand_group_domain(self)`
Genera un dominio dinámico para filtrar los grupos de marcas disponibles en la interfaz, basándose en los permisos del usuario.

- **Permisos de Administrador:** Si el usuario es administrador, devuelve un dominio vacío [], permitiendo ver todos los grupos.
- **Restricción por Responsabilidad:** Para otros usuarios, busca en el modelo `photocheck.brand.group.responsible` los grupos a los que están vinculados.
- **Control de Seguridad:** Si el usuario no tiene grupos asignados, devuelve un dominio que no encuentra registros `[('id', '=', False)]`, garantizando que no se exponga información no autorizada.

## Utilidades

### `get_photocheck_url(self)`
Construye la URL técnica necesaria para redirigir a un usuario directamente a la vista de formulario del registro actual.

- **Base Dinámica:** Utiliza `request.httprequest.host_url` para adaptarse automáticamente al dominio actual (local, desarrollo o producción).
- **Estructura de la URL:** Genera una cadena con los parámetros de ID, modelo (`photocheck`) y tipo de vista (`form`) bajo el hash `#web` de Odoo.

### `create(self, vals_list)`
Extiende la creación estándar de registros para añadir validaciones de duplicidad y asignación de folios.

- **Detección de Duplicados:** Antes de crear, busca registros existentes con el mismo DNI que no estén rechazados. Si encuentra uno, marca el nuevo registro como duplicate.
- **Secuenciación Automática:** Si el registro no tiene un nombre asignado, consulta la secuencia `ir.sequence` configurada para el modelo `photocheck` para generar un código único (ej. FOTO-001).
- **Modo Superusuario:** Realiza la búsqueda de duplicados usando `.sudo()` para garantizar que la validación sea global y no limitada por las reglas de registro del usuario que crea.

## Gestión de Notificaciones (Email)

### `send_email_status_change / send_email_refused / send_email_done`
Conjunto de métodos para disparar comunicaciones automáticas cuando el estado del Photocheck cambia.

- **Ejecución con Privilegios:** Utiliza `.sudo()` para asegurar que el correo se envíe y el registro se actualice incluso si el usuario actual no tiene permisos de escritura sobre ciertos campos técnicos.
- **Registro de Envío:** Actualiza el campo `email_status_change_send_on` con la fecha y hora exacta de la acción.
- **Trazabilidad (Chatter):** Publica un mensaje en el muro del registro (chat) especificando el estado enviado, el destinatario y la marca de tiempo, manteniendo un historial de auditoría claro.

## Campos Computados

- #### `compute_brands(self)` {#compute-brands}
    Mapea y vincula automáticamente las marcas (`brand_ids`) asociadas al grupo de marcas seleccionado, utilizando el comando de Odoo (`6, 0, [ids]`) para reemplazar la relación Many2many.

- #### `compute_initial_name(self)` {#compute-initial-name}
    Extrae la primera palabra del campo `first_name` (primer nombre) para ser utilizada en formatos de visualización simplificados o impresión de carnets.

- #### `compute_photo_name(self)` {#compute-photo-name}
    Genera el nombre técnico del archivo de imagen combinando el DNI y el Nombre Completo, facilitando la identificación de archivos fuera del sistema.

- #### `compute_brand_counter(self)` {#compute-brand-counter}
    Calcula el total de marcas vinculadas al registro, útil para validaciones de interfaz o reportes estadísticos.