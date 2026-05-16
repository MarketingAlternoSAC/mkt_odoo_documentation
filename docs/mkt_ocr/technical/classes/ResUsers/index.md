**Modelo técnico:** `res.users` (Extensión)

Extiende el modelo principal de usuarios de Odoo para incorporar propiedades específicas relacionadas con la funcionalidad de Reconocimiento Óptico de Caracteres (OCR).

### Uso principal

- **Control de Experiencia de Usuario:** Sirve para identificar si el usuario actual ha interactuado previamente con el módulo de OCR.
- **Gestión de Encuestas e Interfaz:** Actúa como un interruptor o indicador para disparar (o no) pop-ups, como la encuesta de satisfacción o futuros tutoriales, garantizando que no se abrume al usuario pidiendo retroalimentación repetitiva tras el primer uso.
