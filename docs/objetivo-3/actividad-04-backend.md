# Actividad 4 — Backend configurado

## Alcance de esta actividad

La arquitectura aprobada utiliza Supabase como backend administrado. Se prepararon:

- `supabase/config.toml` con servicios de API, PostgreSQL, autenticación y almacenamiento.
- `src/assets/js/services/supabase.js` para iniciar el cliente, autenticar, cerrar sesión y verificar la conexión.
- `.env.example` y `config.example.js` con los nombres de variables necesarias.
- Exclusión de la configuración local mediante `.gitignore`.
- Regla explícita para impedir el uso de la llave `service_role` en el navegador.

La creación de la instancia de base de datos y del esquema relacional se realizará en las actividades 6 y 7, como establece el PA. Hasta entonces el frontend funciona en modo demostración.

## Criterio de cumplimiento

El frontend dispone de una capa de servicio independiente y puede cambiar de modo demostración a Supabase completando la URL y la llave publicable, sin modificar las reglas ni las pantallas.

