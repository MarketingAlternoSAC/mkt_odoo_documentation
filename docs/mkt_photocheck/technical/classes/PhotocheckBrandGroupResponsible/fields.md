# Campos: Photocheck Brand Group Responsible

| Nombre | Tipo | Descripción |
| :--- | :--- | :--- |
| `brand_group_id` | Many2one | El grupo de marcas al que se asigna la responsabilidad. |
| `user_id` | Many2one | El usuario (`res.users`) designado como responsable. |
| `city_ids` | Many2many | Ciudades permitidas para este responsable. Si está vacío, puede ver todas las ciudades del grupo. |
