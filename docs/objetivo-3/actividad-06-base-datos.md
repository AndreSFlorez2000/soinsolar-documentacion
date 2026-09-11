# Objetivo 3 — Actividad 6

## Crear la base de datos de desarrollo

**Producto:** Base de datos inicial  
**Formato de evidencia:** Enlace al repositorio  
**Periodo previsto:** 8 al 13 de octubre de 2026

## Objetivo

Crear una base de datos centralizada, segura y reproducible para administrar proyectos, contratos, periodos mensuales, seguimiento, facturación, pagos, costos y gastos.

## Tecnología seleccionada

- PostgreSQL administrado mediante Supabase.
- Supabase Auth para autenticación.
- Row Level Security para autorización.
- Migraciones SQL versionadas.
- Datos de prueba completamente anonimizados.

## Estructura creada

La base de desarrollo contiene:

- Perfiles y roles.
- Proyectos identificados por centro de costo.
- Contratos y valor contractual vigente.
- Periodos mensuales abiertos o cerrados.
- Seguimiento y validación del avance mensual.
- Facturas y pagos.
- Costos y gastos.
- Auditoría de cambios.

## Cálculos cubiertos

- Valor contractual vigente = inicial + adiciones − deducciones.
- Avance financiero = facturado ÷ valor contractual vigente × 100.
- Saldo contractual = valor contractual vigente − facturado.
- Pago pendiente = facturado − pagado.
- Avance mensual = valor reconocido del mes ÷ valor contractual vigente × 100.

## Protección de información

La implementación pública no incluye contraseñas, llaves privadas, información empresarial, proyectos reales ni cifras identificables. El código y las migraciones permanecen en el repositorio privado.

## Resultado

La base inicial quedó definida mediante migraciones reproducibles y una semilla de desarrollo ficticia. Esta estructura sirve como fundamento para las restricciones de integridad de la Actividad 7.
