| Campo                  | Tipo / Relación                | Descripción                                                                                                                     |
| ---------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| `name`                 | Char                           | Identificador de la declaración jurada. Se autogenera mediante una secuencia (`Affidavit`) cuando su valor inicial es `'New'`.  |
| `amount_currency_type` | Selection                      | Tipo de moneda elegido por el usuario (soles o dólares). Determina qué moneda real se asigna al registro.                       |
| `user_vat`             | Char                           | Almacena el RUC o DNI (VAT) del partner asociado al usuario relacionado.                                                        |
| `employee_id`          | Many2one (`hr.employee`)       | Empleado asociado a la declaración. Por defecto, se asigna el empleado del usuario logueado.                                    |
| `job_id`               | Many2one (related)             | Puesto de trabajo del empleado. Campo relacionado con `employee_id`.                                                            |
| `user_id`              | Many2one (`res.users`)         | Usuario Odoo asociado a la declaración. Puede diferir del usuario que crea el registro.                                         |
| `location`             | Char                           | Ubicación donde se emite la declaración (texto libre).                                                                          |
| `country_id`           | Many2one (`res.country`)       | País de la declaración. Por defecto, se asigna Perú (`PE`).                                                                     |
| `state_id`             | Many2one (`res.country.state`) | Departamento asociado a la ubicación.                                                                                           |
| `city_id`              | Many2one                       | Provincia asociada a la ubicación.                                                                                              |
| `district_id`          | Many2one                       | Distrito asociado a la ubicación.                                                                                               |
| `concept`              | Char / Text                    | Concepto principal de la declaración jurada.                                                                                    |
| `activity`             | Char / Text                    | Actividad o detalle adicional relacionado con el concepto.                                                                      |
| `currency_id`          | Many2one (`res.currency`)      | Moneda real usada para el monto (`PEN`, `USD`, etc.).                                                                           |
| `amount`               | Monetary                       | Monto de la declaración, expresado en la moneda definida por `currency_id`.                                                     |
| `date`                 | Date                           | Fecha de la declaración. Campo con tracking habilitado.                                                                         |
| `state`                | Selection                      | Estado del registro (`draft`, `petitioner`). Actualmente no existen métodos que modifiquen este estado.                         |
| `from_portal`          | Boolean                        | Indica si el registro fue creado desde el portal.                                                                               |
| `date_month`           | Char (compute, store)          | Nombre del mes de la fecha (`date`) en texto (por ejemplo, “Enero”). [Calculado](functions.md#compute-date-month) y almacenado. |
| `petitioner_signature` | Binary                         | Firma del solicitante o peticionario.                                                                                           |
| `executive_signature`  | Binary                         | Firma del ejecutivo.                                                                                                            |
