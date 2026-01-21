# Seguridad del Módulo

Este módulo define categorías y grupos de seguridad específicos para gestionar requisitos, liquidaciones, movilidad y presupuesto.

## 📂 Documental Requirement

### 👥 <u>User</u>

(`documental_requirement_user`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                                  | Nombre                              | Modelo                    | Permisos (L,E,C,D) |
| :---------------------------------- | :---------------------------------- | :------------------------ | :----------------- |
| `access_requirement_user`           | `access.documental.requirements`    | `documental.requirements` | ✅ ✅ ✅ ❌        |
| `access_internal_announcement_user` | `Internal Announcement - Read Only` | `internal.announcement`   | ✅ ❌ ❌ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

`show_own_requirement_user_rule`

```python
[
    '|','|','|',
    ('create_uid','=',user.id),
    ('employee_id.parent_id.user_id','=',user.id),
    ('refund_user_id','=',user.id),
    ('refund_employee_id.parent_id.user_id','=',user.id)
]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>External Control</u>

(`documental_requirement_external_control`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                                    | Nombre                           | Modelo                    | Permisos (L,E,C,D) |
| :------------------------------------ | :------------------------------- | :------------------------ | :----------------- |
| `access_requirement_external_control` | `access.documental.requirements` | `documental.requirements` | ✅ ❌ ❌ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

`external_control_requirement_rule`

```python
[
    '|', '|',
    ('create_uid', '=', user.id),
    ('employee_id', 'in', user.get_employes()),
    ('budget_id.partner_brand_id', 'in', user.get_brands()),
    ('is_province_supervizor', '=', user.employee_id.is_supervize_province)
]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Boss</u>

(`documental_requirement_boss`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                        | Nombre                           | Modelo                    | Permisos (L,E,C,D) |
| :------------------------ | :------------------------------- | :------------------------ | :----------------- |
| `access_requirement_boss` | `access.documental.requirements` | `documental.requirements` | ✅ ✅ ❌ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

`executive_requirement_rule`

```python
[
    '|','|','|','|',
    ('create_uid','=',user.id),
    ('budget_id.executive_id','=',user.id),
    ('employee_id.parent_id.user_id','=',user.id),
    ('employee_id.parent_id.parent_id.user_id','=',user.id),
    ('refund_employee_id.parent_id.user_id','=',user.id),
]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Budget Executive</u>

(`documental_requirement_budget_executive`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                                    | Nombre                           | Modelo                    | Permisos (L,E,C,D) |
| :------------------------------------ | :------------------------------- | :------------------------ | :----------------- |
| `access_requirement_budget_executive` | `access.documental.requirements` | `documental.requirements` | ✅ ✅ ❌ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

`budget_responsible_requirement_rule`

```python
[('budget_id.responsible_id','=',user.id)]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Intern Control</u>

(`documental_requirement_intern_control`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                                  | Nombre                           | Modelo                    | Permisos (L,E,C,D) |
| :---------------------------------- | :------------------------------- | :------------------------ | :----------------- |
| `access_requirement_intern_control` | `access.documental.requirements` | `documental.requirements` | ✅ ✅ ❌ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

`show_two_requirement_signatures_rule`

```python
[(1,'=',1)]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Intern Control only read</u>

(`documental_requirement_intern_control_read`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                                            | Nombre                           | Modelo                    | Permisos (L,E,C,D) |
| :-------------------------------------------- | :------------------------------- | :------------------------ | :----------------- |
| `access_requirement_intern_control_read_only` | `access.documental.requirements` | `documental.requirements` | ✅ ❌ ❌ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

`show_requirement_intern_control_only_read`

```python
[(1,'=',1)]
```

- **Permisos:** 👁️ Leer | 🚫 Escribir | 🚫 Crear | 🚫 Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Admin</u>

(`documental_requirement_admin`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                                   | Nombre                           | Modelo                    | Permisos (L,E,C,D) |
| :----------------------------------- | :------------------------------- | :------------------------ | :----------------- |
| `access_requirement_admin`           | `access.documental.requirements` | `documental.requirements` | ✅ ✅ ✅ ❌        |
| `access_internal_announcement_admin` | `Internal Announcement - Admin`  | `internal.announcement`   | ✅ ✅ ✅ ✅        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

`show_all_requirement_admin_rule`

- **Dominio:** (Sin restricciones explícitas aparte de acceso global)
- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Administration</u>

(`documental_requirement_administration`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                                  | Nombre                           | Modelo                    | Permisos (L,E,C,D) |
| :---------------------------------- | :------------------------------- | :------------------------ | :----------------- |
| `access_requirement_administration` | `access.documental.requirements` | `documental.requirements` | ✅ ✅ ❌ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

`administration_requirement_rule`

```python
[(1,'=',1)]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

---

## 📂 Documental Settlement

### 👥 <u>User</u>

(`documental_settlement_user`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                       | Nombre                          | Modelo                   | Permisos (L,E,C,D) |
| :----------------------- | :------------------------------ | :----------------------- | :----------------- |
| `access_settlement_user` | `access.documental.settlements` | `documental.settlements` | ✅ ✅ ❌ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

**Show only own user settlement** (`show_own_settlement_user_rule`)

```python
['|',('create_uid','=',user.id),('employee_id.parent_id.user_id','=',user.id)]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Boss</u>

(`documental_settlement_boss`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                       | Nombre                          | Modelo                   | Permisos (L,E,C,D) |
| :----------------------- | :------------------------------ | :----------------------- | :----------------- |
| `access_settlement_boss` | `access.documental.settlements` | `documental.settlements` | ✅ ✅ ❌ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

**Show only own user settlement** (`show_own_settlement_user_rule`)

```python
['|',('create_uid','=',user.id),('employee_id.parent_id.user_id','=',user.id)]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Intern Control</u>

(`documental_settlement_intern_control`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                                 | Nombre                          | Modelo                   | Permisos (L,E,C,D) |
| :--------------------------------- | :------------------------------ | :----------------------- | :----------------- |
| `access_settlement_intern_control` | `access.documental.settlements` | `documental.settlements` | ✅ ✅ ❌ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

**Show two signatures in Settlement** (`show_two_settlement_signature_rule`)

```python
[('is_boss_signed','=',True)]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Administration (`documental_settlement_administration`) y Accounting</u>

(`documental_settlement_accounting`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                                        | Nombre                               | Modelo                   | Permisos (L,E,C,D) |
| :---------------------------------------- | :----------------------------------- | :----------------------- | :----------------- |
| `access_settlement_administration`        | `access.documental.settlements`      | `documental.settlements` | ✅ ✅ ❌ ❌        |
| `access_settlement_accounting`            | `access.documental.settlements`      | `documental.settlements` | ✅ ✅ ❌ ❌        |
| `access_internal_announcement_accounting` | `Internal Announcement - Accounting` | `internal.announcement`  | ✅ ✅ ✅ ✅        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

**Show three signatures in Settlement** (`show_three_settlement_signature_rule`)

```python
[(1,'=',1)]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Admin</u>

(`documental_settlement_admin`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                        | Nombre                          | Modelo                   | Permisos (L,E,C,D) |
| :------------------------ | :------------------------------ | :----------------------- | :----------------- |
| `access_settlement_admin` | `access.documental.settlements` | `documental.settlements` | ✅ ✅ ✅ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

**Show all settlement** (`show_all_settlement_admin_rule`)

- **Dominio:** (Acceso Total)
- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

---

## 📂 Documental Mobility

### 👥 <u>User</u>

(`documental_mobility_user`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                     | Nombre                                  | Modelo                           | Permisos (L,E,C,D) |
| :--------------------- | :-------------------------------------- | :------------------------------- | :----------------- |
| `access_mobility_user` | `access.documental.mobility.expediture` | `documental.mobility.expediture` | ✅ ✅ ✅ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

**Show only own user mobility** (`show_own_mobility_user_rule`)

```python
[('create_uid','=',user.id)]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Boss</u>

(`documental_mobility_boss`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                     | Nombre                                  | Modelo                           | Permisos (L,E,C,D) |
| :--------------------- | :-------------------------------------- | :------------------------------- | :----------------- |
| `access_mobility_boss` | `access.documental.mobility.expediture` | `documental.mobility.expediture` | ✅ ✅ ✅ ❌        |

<strong> 📜 Reglas de Registro </strong>

(Record Rules)

**Show only own boss mobility** (`show_own_mobility_boss_rule`)

```python
[
    '|',
    ('employee_id.parent_id.user_id','=',user.id),
    ('employee_id.parent_id.parent_id.user_id','=',user.id),
]
```

- **Permisos:** 👁️ Leer | ✏️ Escribir | ➕ Crear | 🗑️ Eliminar

---

## 📂 Budget Control

### 👥 <u>User</u>

(`budget_control_user`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                   | Nombre          | Modelo   | Permisos (L,E,C,D) |
| :------------------- | :-------------- | :------- | :----------------- |
| `access_budget_user` | `access.budget` | `budget` | ✅ ❌ ❌ ❌        |

<br/>
<hr style="border: 2px solid #333; margin: 2rem 0;">
<br/>

### 👥 <u>Administrator</u>

(`budget_control_administrator`)

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                            | Nombre          | Modelo   | Permisos (L,E,C,D) |
| :---------------------------- | :-------------- | :------- | :----------------- |
| `access_budget_administrator` | `access.budget` | `budget` | ✅ ✅ ✅ ❌        |

---

<strong> 🔐 Accesos</strong>

### 👥 <u>Todos lo</u>

s usuarios

<strong> 🔐 Accesos</strong>

(ir.model.access)

| ID                                      | Nombre                                    | Modelo                             | Permisos (L,E,C,D) |
| :-------------------------------------- | :---------------------------------------- | :--------------------------------- | :----------------- |
| `access_budget_campaign`                | `access.budget.campaign`                  | `budget.campaign`                  | ✅ ✅ ✅ ❌        |
| `access_budget_class`                   | `access.budget.class`                     | `budget.class`                     | ✅ ✅ ✅ ❌        |
| `access_year_month`                     | `access.year.month`                       | `year.month`                       | ✅ ✅ ✅ ❌        |
| `access_budget_line`                    | `access.budget.line`                      | `budget.line`                      | ✅ ✅ ✅ ✅        |
| `access_requirement_justification`      | `access.requirement.detail.justification` | `requirement.detail.justification` | ✅ ✅ ✅ ✅        |
| `access_requirement_detail`             | `access.requirement.detail`               | `requirement.detail`               | ✅ ✅ ✅ ✅        |
| (_... y otros modelos complementarios_) |                                           |                                    |                    |
