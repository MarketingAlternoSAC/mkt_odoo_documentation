# Guía Técnica: OCR

Esta sección describe la arquitectura técnica, las dependencias y la lógica interna del módulo `mkt_ocr_plugin`.

## Arquitectura del Módulo

El módulo actúa como un puente entre Odoo 17 y un servicio externo de OCR, gestionando las peticiones, procesando las respuestas JSON y mapeando los resultados a campos de Odoo.

---

## Estructura de Archivos

*   `models/`: Contiene la lógica de negocio, configuración y registros (logs) del OCR.
*   `views/`: Definiciones de la interfaz de usuario para las configuraciones y los logs.
*   `security/`: Definición de grupos de acceso y permisos.
*   `utils/`: Funciones de utilidad para comunicarse con la API de OCR.

---

## Diagrama de Flujo
<div align="center">

```mermaid

graph TD
    %% Estilos de los contenedores
    subgraph Archivo ["Módulo de Archivo"]
        A1["Conversión a Base64<br/>y Guardado"]
    end

    subgraph OCR ["Servidor OCR"]
        B1["Extracción con<br/>Modelo Donut"] --> B2["Limpieza de<br/>Datos Crudos"]
        B2 --> B3["Envío de Resultados"]
    end

    subgraph Backend ["Lógica de Backend"]
        C1["Normalización de Fecha<br/>(y-m-d)"] --> C2["Estandarización de Factura<br/>(XXXX-XXXXXXXXXX)"]
    end

    subgraph Usuario ["Intervención de Usuario"]
        D1["Revisión de Datos<br/>Extraídos"] --> D2["Guardar Liquidación<br/>en Odoo"]
    end

    %% Flujo Principal
    Inicio((Inicio)) --> Archivo
    Archivo --> OCR
    OCR --> Backend
    Backend --> Usuario
    D2 --> Fin((Fin))

    %% Estilización de nodos
    style Inicio fill:#f9f,stroke:#333,stroke-width:2px
    style Fin fill:#f9f,stroke:#333,stroke-width:2px
    style Archivo fill:#e1f5fe,stroke:#01579b
    style OCR fill:#f3e5f5,stroke:#4a148c
    style Backend fill:#fff3e0,stroke:#e65100
    style Usuario fill:#e8f5e9,stroke:#1b5e20
```
</div>

---