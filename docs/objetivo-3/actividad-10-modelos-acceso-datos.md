# Objetivo 3 — Actividad 10

## Implementar los modelos y estructuras de acceso a datos

**Producto:** Modelos implementados  
**Formato de evidencia:** Enlace al repositorio  
**Periodo previsto:** 14 al 19 de octubre de 2026

## Objetivo

Crear una capa organizada entre las interfaces y Supabase para centralizar validaciones, consultas, filtros, escritura de movimientos y manejo de errores.

## Estructura implementada

### Modelos y validaciones

- Validación de identificadores UUID.
- Validación y redondeo de valores monetarios.
- Validación de textos obligatorios.
- Estados permitidos.
- Normalización de proyectos.
- Normalización de resúmenes financieros.
- Normalización de seguimiento mensual.
- Transformación de formularios a registros de base de datos.

### Repositorios

| Componente | Responsabilidad |
|---|---|
| Proyectos | Listar, filtrar, consultar, crear y modificar proyectos. |
| Contratos | Consultar y actualizar la información contractual. |
| Periodos | Consultar, crear y cerrar periodos mensuales. |
| Seguimiento | Registrar y validar el avance mensual. |
| Finanzas | Gestionar facturas, pagos, costos y gastos. |

### Servicio central

Un servicio único entrega los repositorios a las interfaces usando la sesión autenticada. Las pantallas no necesitan repetir la configuración ni construir consultas directamente.

## Filtros incluidos

- Proyecto.
- Centro de costo.
- Municipio.
- Estado.
- Año y mes.
- Tipo de movimiento.
- Periodo.

## Protección

Las validaciones del navegador mejoran la experiencia, pero PostgreSQL conserva la autoridad final sobre límites contractuales, pagos, periodos, roles y auditoría.

## Pruebas ejecutadas

Se comprobaron:

- Saldo contractual.
- Pago pendiente.
- Avance financiero.
- Protección frente a división por cero.
- Rechazo de valores negativos.
- Conversión de formularios.
- Rechazo de estados inválidos.

La verificación conjunta obtuvo **11 pruebas aprobadas y 0 fallos**. Los datos usados fueron ficticios.

## Resultado

Los módulos de proyectos, contratos, seguimiento, facturación, pagos, costos y gastos ya cuentan con una vía común de acceso a datos. El código permanece en el repositorio privado y esta evidencia pública no expone detalles sensibles.

## Diagramas relacionados

La organización de los modelos y repositorios se complementa con:

- [Diagramas del sistema](./diagramas-sistema.md): arquitectura, secuencia, clases, entidad–relación y seguridad.
