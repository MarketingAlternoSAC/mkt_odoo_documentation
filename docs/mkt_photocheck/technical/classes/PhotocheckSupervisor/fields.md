# Campos: Photocheck Supervisor

| Nombre | Tipo | Descripción |
| :--- | :--- | :--- |
| `name` | Char | Nombre del supervisor. `Requerido`. |
| `user_id` | Many2one | Usuario del supervisor.|
| `brand_group_ids` | Many2one | Grupo de marcas del supervisor.|
| `city_ids` | Many2one | Ciudades del supervisor. Utiliza la funcion [`_get_all_cities`](functions.md#get-all-cities) para obtener las ciudades del supervisor.|