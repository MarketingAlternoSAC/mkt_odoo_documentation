# Guía Técnica: Photocheck

Esta sección describe la arquitectura técnica, las dependencias y la lógica interna del módulo `mkt_photocheck`.

## Arquitectura del Módulo

El módulo sigue la estructura estándar de Odoo 17, con lógica personalizada para la gestión de imágenes y la seguridad multinivel.

## Dependencias de Software (Python)

Para que el procesamiento de imágenes funcione, el servidor debe tener instaladas las siguientes librerías:

*   `rembg`: Librería principal para la eliminación de fondos mediante inteligencia artificial.
*   `onnxruntime`: Motor de ejecución para los modelos de `rembg`.
*   `Pillow (PIL)`: Para la manipulación básica de imágenes en memoria.
*   `numpy`: Para la conversión de imágenes a arreglos compatibles con `rembg`.

## Estructura de Archivos

*   `models/`: Contiene la lógica de negocio y las definiciones de tablas.
*   `views/`: Definiciones de la interfaz de usuario (formularios, listas, menús).
*   `security/`: Definición de grupos de acceso y reglas de registro (ir.rule).
*   `data/`: Contiene secuencias (para el nombre autogenerado) y plantillas de correo.
*   `report/`: Definición del diseño del fotocheck para su impresión.
