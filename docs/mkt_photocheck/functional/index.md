# Guía Funcional: Photocheck

Esta sección está dirigida a los usuarios finales, supervisores y administradores que interactúan con la interfaz de usuario del módulo de Photocheck.

## Objetivos del Módulo

El objetivo principal es agilizar el proceso de emisión de fotochecks, asegurando que la información sea verídica y las fotos cumplan con los estándares requeridos.

## Roles de Usuario

1.  **Usuario / Solicitante:** Puede crear solicitudes de fotocheck y enviarlas a revisión.
2.  **Supervisor / Boss:** Supervisa las solicitudes de su grupo de marcas y ciudades asignadas. Puede aprobar o rechazar solicitudes.
3.  **Diseñador:** Tiene acceso a las solicitudes aprobadas para proceder con la impresión o diseño final.
4.  **Administrador:** Posee control total sobre la configuración de grupos, marcas, ciudades y cargos.

## Funcionalidades Clave

### Búsqueda por DNI
Al ingresar el DNI de un colaborador, el sistema consulta automáticamente una base de datos de la reniec por medio de [`Api DNI`](/mkt_photocheck/technical/utils/api_dni/functions/) para obtener el nombre completo, nombres y apellidos, minimizando errores de digitación.

### Procesamiento de Fotos
El sistema permite cargar una fotografía del colaborador y, mediante un botón de procesamiento, eliminar automáticamente el fondo de la imagen, dejando solo la silueta necesaria para el carnet.

### Detección de Duplicados
Al crear una nueva solicitud, el sistema verifica si ya existe un fotocheck activo para ese DNI, marcándolo como "Duplicado" si es necesario para evitar emisiones innecesarias.
