# Flujo de Trabajo OCR

El flujo de trabajo de OCR permite procesar documentos (PDFs) y extraer información clave para autocompletar campos en registros de Odoo.

## Responsables involucrados

| Rol Responsable | Responsabilidad | Grupo Permisos |
| :--- | :--- | :--- |
| Usuario Generico | Cualquier usuario que tenga acceso al módulo de Documental Management puede generar el requerimiento y liquidación donde subirá el archivo para ser procesado por el OCR. | base.group_user |

---
## Proceso Paso a Paso

El proceso se divide en cuatro fases clave diseñadas para transformar la información de un documento PDF en datos estructurados y listos para usar dentro de Odoo.

1. **Inicio del Requerimiento:**
    El usuario comienza su flujo de trabajo habitual dentro del módulo de **Documental Management**. Aquí debe crear un nuevo requerimiento, completar los campos obligatorios de la cabecera y proceder a generar una nueva línea de liquidación.

2. **Carga de Archivo y Extracción OCR:**
    Dentro del asistente (wizard) de la línea de liquidación, el usuario debe cargar el archivo PDF de la factura. Una vez cargado, debe pulsar el botón **"Procesar OCR"**.
    *   **¿Qué sucede detrás?** El sistema envía el documento a nuestro servidor especializado que, mediante inteligencia artificial (modelo `DONUT`), identifica y extrae los datos.
    *   > **IMPORTANTE:**
        Actualmente, el sistema está optimizado exclusivamente para **Facturas**. La compatibilidad con otros tipos de documentos se implementará en futuras versiones.

3. **Confirmación de Procesamiento:**
    Si la extracción es exitosa, el sistema mostrará un mensaje de confirmación en pantalla:
    <figure markdown="span">
        ![Mensaje de éxito](../../assets/MensajeExitoOCR.png){ width="300" }
    </figure>
    Este mensaje indica que los campos de la liquidación han sido autocompletados con la información detectada en el PDF.

4. **Verificación Humana y Registro Final:**
    Como paso final y más importante, el usuario debe actuar como filtro de calidad:
    *   **Revisión Visual:** Comprobar que los campos (RUC, Fecha, Serie, Número, Montos) coincidan exactamente con lo que muestra la factura física o digital.
    *   **Corrección:** Si algún dato no fue detectado correctamente, el usuario puede editar el campo manualmente en ese momento.
    *   **Finalización:** Una vez validada la información, se presiona **Guardar** para registrar oficialmente la línea de liquidación.
---

## Diagrama de Flujo
<div align="center">

```mermaid
graph TD
    %% Estilos de los contenedores
    subgraph Requerimiento  ["Requerimiento: Módulo Documental Management"]
        A1["Crear Nuevo Requerimiento"] --> A2["Generar Línea de Liquidación"]
    end

    subgraph Procesamiento ["Procesamiento"]
        B1["Cargar Archivo PDF"] --> B2["Presionar Botón <br/>"Procesar OCR""]
        B2 --> B3["Extracción con<br/>Modelo Donut"]
    end

    subgraph Verificación ["Verificación"]
        C1["Revisión Visual de Datos"] --> C2["Edición Manual (si aplica)"]
    end

    subgraph Finalización ["Finalización"]
        D1["Guardar Liquidación<br/>en Odoo"] --> D2["Fin"]
    end

    %% Flujo Principal
    Inicio((Inicio)) --> Requerimiento 
    Requerimiento --> Procesamiento
    Procesamiento --> Verificación
    Verificación --> Finalización
    D2 --> Fin((Fin))

    %% Estilización de nodos
    style Inicio fill:#f9f,stroke:#333,stroke-width:2px
    style Fin fill:#f9f,stroke:#333,stroke-width:2px
    style Requerimiento fill:#e1f5fe,stroke:#01579b
    style Procesamiento fill:#f3e5f5,stroke:#4a148c
    style Verificación fill:#fff3e0,stroke:#e65100
    style Finalización fill:#e8f5e9,stroke:#1b5e20
```
</div>
