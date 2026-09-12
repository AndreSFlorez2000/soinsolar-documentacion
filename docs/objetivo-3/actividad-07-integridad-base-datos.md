# Objetivo 3 — Actividad 7

## Implementar tablas, relaciones y restricciones de integridad

**Producto:** Script y esquema de base de datos  
**Formato de evidencia:** Enlace al repositorio  
**Periodo previsto:** 9 al 14 de octubre de 2026

## Resultado técnico

Se implementó un modelo relacional compuesto por perfiles, proyectos, contratos, periodos, seguimiento mensual, facturas, pagos, costos/gastos y auditoría.

## Relaciones

- Un proyecto puede tener varios contratos.
- Cada movimiento conserva su proyecto y periodo.
- Las facturas se relacionan con el proyecto y contrato correspondientes.
- Los pagos pertenecen a una factura.
- Los costos y gastos se registran por proyecto y mes.
- Las claves compuestas evitan asociaciones entre proyectos equivocados.

## Controles de integridad

1. Centro de costo único.
2. Valores monetarios válidos.
3. Valor contractual vigente calculado automáticamente.
4. Facturación acumulada limitada por el contrato.
5. Pagos acumulados limitados por la factura.
6. Prohibición de pagar o anular incorrectamente una factura.
7. Avance reconocido limitado por el contrato.
8. Prohibición de reducir un contrato por debajo de lo ejecutado.
9. Fechas coherentes con el periodo mensual.
10. Bloqueo de movimientos en periodos cerrados.
11. Validación mensual con usuario y fecha.
12. Auditoría de inserciones, modificaciones y eliminaciones.

## Seguridad

- El administrador gestiona información, usuarios y cierres.
- Gerencia consulta y modifica dentro de permisos controlados.
- Los usuarios no autenticados no acceden a información financiera.
- Las políticas RLS se ejecutan en PostgreSQL.

## Vistas calculadas

- Resumen financiero por proyecto.
- Resumen mensual por proyecto y periodo.

Estas vistas entregan contrato vigente, facturado, pagado, costos/gastos, saldo contractual, pago pendiente y porcentaje de avance sin duplicar fórmulas en cada pantalla.

## Resultado

La integridad quedó centralizada en la base de datos, disminuyendo el riesgo de duplicidad, fórmulas dañadas y diferencias entre fuentes. Los scripts completos permanecen protegidos en el repositorio privado.

## Diagramas relacionados

Las reglas de integridad se complementan con representaciones visuales del modelo y de sus controles:

- [Diagramas del sistema](./diagramas-sistema.md): entidad–relación, estados del periodo, validación financiera, seguridad y auditoría.
