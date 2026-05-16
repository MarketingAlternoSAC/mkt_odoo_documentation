# Seguridad: Photocheck

Este módulo define la estructura de permisos y reglas de registro para gestionar la creación y flujo de photochecks, segmentando el acceso según el rol jerárquico y la responsabilidad sobre marcas o ciudades.

## Definición de Grupos (res_groups.xml)

#### 1. User (`photocheck_user`)

Personal operativo con acceso a sus propios registros. Solo puede ver y gestionar sus propios registros o donde figura como supervisor.

#### 2. Boss (`photocheck_boss`)

Jefaturas con capacidad de gestión sobre grupos de marcas específicos con acceso a registros según su responsabilidad por marca y ciudad.

#### 3. Designer (`photocheck_designer`)

Equipo de diseño encargado del procesamiento gráfico. Tienen visibilidad de todos los registros que han pasado el estado borrador.

#### 4. Admin (`photocheck_admin`)

Administradores con acceso total sin restricciones.

## Reglas de Registro (`ir.rule`)

**Usuario (`photocheck_user`)**

| ID de Regla | Nombre | Modelo | Permisos (L,E,C,D) |
|-------------|--------|--------|-------------------|
| `access_photocheck_user` | `access.photocheck.user` | `photocheck` | ✅ ✅ ✅ ❌ |
| `access_photocheck_brand_group_user` | `access.photocheck.brand.group.user` | `photocheck.brand.group` | ✅ ❌ ❌ ❌ |
| `access_photocheck_job_user` | `access.photocheck.job.user` | `photocheck.job` | ✅ ❌ ❌ ❌ |
| `access_photocheck_city_user` | `access.photocheck.city.user` | `photocheck.city` | ✅ ❌ ❌ ❌ |
| `access_photocheck_supervisor_user` | `access.photocheck.supervisor.user` | `photocheck.supervisor` | ✅ ❌ ❌ ❌ |

**Nota de Seguridad:** El usuario puede crear y editar photochecks, pero no tiene permisos para modificar los maestros (ciudades, cargos, marcas) ni eliminar registros.

**Jefe (`photocheck_boss`)**

| ID de Regla | Nombre | Modelo | Permisos (L,E,C,D) |
|-------------|--------|--------|-------------------|
| `access_photocheck_boss` | `access.photocheck.boss` | `photocheck` | ✅ ✅ ✅ ❌ |
| `access_photocheck_brand_group_boss` | `access.photocheck.brand.group.boss` | `photocheck.brand.group` | ✅ ✅ ✅ ❌ |
| `access_photocheck_job_boss` | `access.photocheck.job.boss` | `photocheck.job` | ✅ ✅ ✅ ❌ |
| `access_photocheck_city_boss` | `access.photocheck.city.boss` | `photocheck.city` | ✅ ✅ ✅ ❌ |
| `access_photocheck_supervisor_boss` | `access.photocheck.supervisor.boss` | `photocheck.supervisor` | ✅ ✅ ✅ ❌ |

**Nota de Seguridad:** Posee permisos de escritura y creación en catálogos para gestionar la estructura operativa de su equipo.

**Diseñador (`photocheck_designer`)**

| ID de Regla | Nombre | Modelo | Permisos (L,E,C,D) |
|-------------|--------|--------|-------------------|
| `access_photocheck_designer` | `access.photocheck.designer` | `photocheck` | ✅ ✅ ✅ ❌ |
| `access_photocheck_brand_group_designer` | `access.photocheck.brand.group.designer` | `photocheck.brand.group` | ✅ ❌ ❌ ❌ |

**Nota de Seguridad:** Similar al usuario, pero su visibilidad está restringida por estados del flujo de trabajo mediante reglas de registro.

**Administrador (`photocheck_admin`)**

| ID de Regla | Nombre | Modelo | Permisos (L,E,C,D) |
|-------------|--------|--------|-------------------|
| `access_photocheck_admin` | `access.photocheck.admin` | `photocheck` | ✅ ✅ ✅ ✅ |
| `Todas las tablas maestras` | `-` | `Modelos del módulo` | ✅ ✅ ✅ ✅ |

**Nota de Seguridad:** Posee permisos de escritura y creación en catálogos para gestionar la estructura operativa de su equipo.

## Reglas de Registro (`ir_rule.xml`)

| Nombre de Regla | Grupo | Dominio (Lógica de Filtro) |
|-------------|--------|-------------------|
| `My own Photocheck` | `User` | Registros creados por el usuario, donde es el supervisor o donde es el usuario asignado.|
| `Boss Group Access` | `Boss` | Registros cuya marca pertenece a un grupo donde el usuario es responsable (`responsible_lines`). |
| `Some Photocheck` | `Designer` | Todos los registros cuyo estado sea diferente a 'Borrador' (`draft`). |
| `All Photocheck` | `Admin` | Acceso irrestricto a todos los registros del modelo (`1=1`). |

**Lógica de Seguridad:**
La visibilidad del Boss es dinámica; no depende de la creación manual, sino de su vinculación como responsable en el modelo de grupos de marcas, permitiendo una supervisión transversal.

## Filtro por Código (`search`)

La clase `Photocheck` sobreescribe el método `search` para inyectar dominios dinámicos basados en la tabla de responsabilidades, asegurando que un "Boss" solo vea registros de las marcas y ciudades que tiene permitidas explícitamente.
