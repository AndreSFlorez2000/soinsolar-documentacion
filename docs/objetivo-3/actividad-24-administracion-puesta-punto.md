# Actividad técnica complementaria 24 — Administración y puesta a punto

**Objetivo:** completar la navegación, la gestión básica de perfiles y la preparación verificable de la versión 1.1.0.

## Administración de perfiles

La aplicación consulta los perfiles ya vinculados con Supabase Auth y permite a un administrador autorizado cambiar el nombre, el rol y el estado de otros perfiles.

Roles definidos:

- **Administrador:** registra, modifica y administra la información.
- **Gerencia:** consulta reportes y, según la política aprobada, puede modificar información permitida.

Por seguridad, la interfaz web no crea cuentas de autenticación ni maneja contraseñas administrativas. Las cuentas nuevas se crean desde Supabase Auth o un servicio seguro del servidor y luego aparecen en la vista de administración.

## Puesta a punto

- Menú completo: inicio, proyectos, seguimiento mensual, facturación y pagos, costos y gastos, reportes, historial y administración.
- Eliminación de pantallas de relleno.
- Recuperación de la vista seleccionada mediante la ruta.
- Actualización manual de datos.
- Estado visible del modo de datos.
- Datos públicos anonimizados.
- Validaciones y mensajes de error comprensibles.
- Actualización de modelos, repositorios, servicios y demostración.
- Ampliación de las pruebas automáticas.

## Seguridad conservada

- Autenticación con Supabase.
- Row Level Security.
- Llave publicable en el navegador; nunca service_role.
- Contraseñas y secretos fuera del repositorio.
- Auditoría de operaciones.
- Separación entre repositorio privado y demostración pública anonimizada.

## Resultado

La versión 1.1.0 queda coherente para evaluación funcional y puede pasar a los casos de prueba del Objetivo 4.
