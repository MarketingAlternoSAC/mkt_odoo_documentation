# Funciones: Supervisor

## Metodos de Obtencion de datos

### `_get_all_cities(self)` {#get-all-cities}

La función es un método de utilidad diseñado para inicializar valores por defecto en un campo relacional. Utiliza el decorador `@api.model`

- **self.env['photocheck.city']**: Accede al registro del modelo de ciudades mediante el entorno de Odoo.
- **.search([]).ids**: El método search con un dominio vacío [] recupera todos los registros de la tabla. El atributo .ids extrae únicamente la lista de identificadores enteros.

**Propósito**: Garantiza que, al abrir un formulario de creación, el campo Many2many aparezca pre-poblado con todas las ciudades existentes en la base de datos.