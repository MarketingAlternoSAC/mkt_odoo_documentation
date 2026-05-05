# Seguridad y Accesos del Módulo OCR

Este módulo define los permisos de acceso para el modelo de logs y encuestas (ocr.usage.log), asegurando que los usuarios estándar y los administradores tengan los privilegios correctos.

## OCR Usage Log

### <u>Usuario Interno</u>

(`base.group_user`)

**Accesos**

(ir.model.access)

| ID de Regla | Nombre | Modelo | Permisos (L,E,C,D) |
|-------------|--------|--------|-------------------|
| `access_encuesta_ocr` | `encuesta.user` | `ocr.usage.log` | ✅ ❌ ✅ ❌ |


**Nota de Seguridad:** Los usuarios estándar solo pueden crear nuevos logs (al enviar una encuesta o usar el OCR) y leerlos. No pueden modificarlos ni eliminarlos, garantizando la integridad de los datos de métricas.

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">

### <u>Administrador</u>

(`base.group_system`)

**Accesos**

(ir.model.access)

| ID de Regla | Nombre | Modelo | Permisos (L,E,C,D) |
|-------------|--------|--------|-------------------|
| `acces_encuesta_admin` | `encuesta.admin` | `ocr.usage.log` | ✅ ✅ ✅ ✅ |

**Nota de Seguridad:** Los administradores tienen control total sobre los registros de uso del OCR, lo que les permite depurar y administrar los logs del sistema.