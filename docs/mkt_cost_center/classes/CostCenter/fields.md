| Campo              | Tipo / Relación                 | Descripción                               |
| :----------------- | :------------------------------ | :---------------------------------------- |
| `name`             | Char                            | Nombre del centro de costo. Requerido. Debe ser único. |
| `code`             | Char                            | Número o código del centro de costo. Es el identificador principal (`_rec_name`). Requerido. |
| `partner_id`       | Many2one (`res.partner`)        | Razón Social (Socio) asociada. Requerido. |
| `province_id`      | Many2one (`res.province`)       | Provincia a la que pertenece el centro de costo. Requerido. |
| `partner_brand_id` | Many2one (`res.partner.brand`)  | Marca asociada. Requerido.                |
| `executive_id`     | Many2one (`res.users`)          | Ejecutivo asignado.                       |
| `responsible_id`   | Many2one (`res.users`)          | Usuario responsable del centro de costo.  |
