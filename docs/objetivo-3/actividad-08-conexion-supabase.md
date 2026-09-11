# Objetivo 3 — Actividad 8

## Configurar la conexión entre la aplicación y la base de datos

**Producto:** Conexión funcional  
**Formato de evidencia:** Enlace al repositorio  
**Periodo previsto:** 13 al 16 de octubre de 2026

> En el plan de trabajo la actividad de conexión aparece después de la Actividad 7 con numeración 9. Para conservar la secuencia trabajada, esta evidencia se presenta como Actividad 8.

## Arquitectura

La aplicación web usa el cliente oficial de Supabase para conectarse a PostgreSQL mediante:

- URL pública del proyecto.
- Llave publicable limitada.
- Sesión autenticada.
- Políticas Row Level Security.

No se incorpora la llave administrativa en el frontend.

## Funciones implementadas

- Inicialización controlada del cliente.
- Inicio y cierre de sesión.
- Persistencia y renovación de sesión.
- Modo demostrativo cuando no existe configuración.
- Comprobación de autenticación.
- Consulta protegida al resumen financiero.
- Manejo uniforme de errores.

## Verificación automatizada

Se creó una prueba que revisa:

1. Validez de la URL y de la llave publicable.
2. Disponibilidad del servicio de autenticación.
3. Inicio de sesión con un usuario local de prueba.
4. Acceso autorizado a una vista financiera.
5. Aplicación de las políticas RLS.

## Seguridad

- No se publican credenciales.
- La conexión de demostración no contiene datos reales.
- Las contraseñas se proporcionan únicamente mediante variables locales.
- Los permisos se validan en la base de datos, no solo en los botones de la interfaz.
- Las acciones principales quedan auditadas.

## Resultado

La conexión quedó implementada de forma reutilizable y verificable. El prototipo público conserva datos ficticios, mientras el entorno conectado solo presenta información después de autenticar al usuario.
