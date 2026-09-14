# Cierre técnico previo al Objetivo 4

**Proyecto:** Sistema de control de costos, gastos y ejecución de proyectos  
**Versión evaluada:** 1.1.0  
**Fecha:** 14 de septiembre de 2026

## 1. Propósito

Verificar la terminación del Objetivo 3 y completar los elementos funcionales indispensables antes de iniciar las pruebas y la validación del Objetivo 4.

La revisión del cronograma oficial confirma que el Objetivo 3 finaliza en la Actividad 20, correspondiente a la integración de los componentes en una versión funcional. El cronograma presenta un salto de numeración después de la Actividad 7; la conexión con Supabase se documentó previamente como Actividad 8 para conservar la trazabilidad, pero no se modifica el cronograma oficial.

## 2. Diagnóstico de la versión integrada

| Componente revisado | Situación encontrada en 1.0.0 | Acción de cierre | Resultado 1.1.0 |
|---|---|---|---|
| Proyectos | CRUD y consulta funcionales | Mantener e integrar | Completo |
| Contratos | Existía la tabla, pero no una interfaz para crear o editar el contrato | Actividad técnica 21 | Gestión contractual incorporada |
| Facturación | Existía el modelo de datos, pero no la operación desde la interfaz | Actividad técnica 22 | Registro y consulta incorporados |
| Pagos | Existía el modelo de datos, pero no la operación desde la interfaz | Actividad técnica 22 | Registro y control de saldos incorporados |
| Costos y gastos | Funcional | Integrar con reportes | Completo |
| Seguimiento mensual | Funcional | Vincular resultados financieros | Completo |
| Reportes gerenciales | Indicadores dispersos | Actividad técnica 23 | Informe consolidado, filtros y CSV |
| Usuarios y roles | Seguridad en base de datos, sin vista administrativa | Actividad técnica 24 | Consulta y edición controlada de perfiles existentes |
| Historial | Funcional | Incluir nuevas operaciones | Contratos, facturas y pagos auditables |
| Navegación | Había secciones incompletas o no accesibles | Actividad técnica 24 | Ocho módulos funcionales y sin pantallas de relleno |

## 3. Actividades técnicas complementarias

Estas actividades son acciones internas de cierre y no agregan filas ni cambian la numeración del PA.

1. **Actividad técnica 21 — Gestión contractual:** crear y modificar el contrato asociado con cada proyecto y calcular su valor vigente.
2. **Actividad técnica 22 — Facturación y pagos:** registrar facturas y pagos por periodo, impedir excedentes y mostrar saldos.
3. **Actividad técnica 23 — Reportes gerenciales:** consolidar contrato, facturación, recaudo, costos, avance y saldos; filtrar y exportar.
4. **Actividad técnica 24 — Administración y puesta a punto:** visualizar perfiles existentes, controlar roles, completar navegación, pruebas y documentación.

## 4. Flujo funcional consolidado

1. El administrador registra un proyecto.
2. Asocia un contrato con valor inicial, adiciones y deducciones.
3. Trabaja únicamente sobre el periodo mensual abierto.
4. Registra facturas asociadas al proyecto, contrato y periodo.
5. Registra pagos contra una factura existente.
6. Registra costos y gastos soportados.
7. Consolida el seguimiento mensual.
8. Consulta indicadores generales y por proyecto.
9. Genera el reporte gerencial y lo exporta a CSV.
10. Consulta el historial para conocer qué cambió, cuándo y sobre qué registro.
11. Administra el rol o estado de perfiles ya creados en Supabase Auth.

## 5. Reglas y fórmulas implementadas

| Concepto | Fórmula o regla |
|---|---|
| Valor contractual vigente | Valor inicial + adiciones − deducciones |
| Avance financiero | Total facturado ÷ valor contractual vigente × 100 |
| Saldo contractual | Valor contractual vigente − total facturado |
| Pago pendiente | Total facturado − total pagado |
| Recaudo | Total pagado ÷ total facturado × 100 |
| Tasa de costos | Costos y gastos ÷ valor contractual vigente × 100 |
| Diferencia facturación-costos | Total facturado − costos y gastos |
| Límite de facturación | La suma de facturas no puede superar el contrato vigente |
| Límite de pago | La suma de pagos de una factura no puede superar su valor |
| Periodo | La fecha del movimiento debe pertenecer al periodo seleccionado y este debe estar abierto |
| Identificación | Centro de costo, número de factura y referencia de pago no pueden repetirse |

## 6. Arquitectura verificada

- Interfaz web en HTML5, CSS3, Bootstrap y JavaScript modular.
- Capa de aplicación que selecciona modo demostrativo o Supabase.
- Modelos y repositorios para proyectos, contratos, periodos, seguimiento, facturas, pagos, costos, perfiles y auditoría.
- PostgreSQL/Supabase con autenticación, restricciones, auditoría y seguridad por filas.
- Demostración pública anonimizada con persistencia local.
- Pruebas automáticas en GitHub Actions.

## 7. Criterio de preparación para el Objetivo 4

La versión 1.1.0 queda preparada para iniciar el Objetivo 4 porque:

- el ciclo proyecto–contrato–factura–pago–costo–seguimiento–reporte está disponible;
- los cálculos principales son reproducibles;
- las validaciones financieras impiden excedentes;
- las operaciones sensibles dependen del periodo abierto;
- los perfiles y roles existentes pueden revisarse;
- las acciones generan trazabilidad;
- se dispone de datos demostrativos, guía de prueba y casos verificables.

La aprobación definitiva de usabilidad y concordancia con el proceso real corresponde al Objetivo 4 y debe ser realizada con usuarios de SOINSOLAR sobre un entorno de validación, sin utilizar información sensible en la demostración pública.
