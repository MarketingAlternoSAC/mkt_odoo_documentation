# Campos: Photocheck Brand Group

| Nombre | Tipo | Descripción |
| :--- | :--- | :--- |
| `name` | Char | Nombre del grupo. |
| `brand_ids` | Many2many | Marcas asociadas (`res.partner.brand`). |
| `responsible_lines` | One2many | Usuarios responsables asignados a este grupo. |
