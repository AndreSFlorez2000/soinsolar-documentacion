# Prueba técnica ejecutada y documentada

**Sistema:** Control de costos, gastos y ejecución de proyectos  
**Versión:** 1.1.0  
**Fecha de ejecución:** 14 de septiembre de 2026  
**Tipo:** verificación automatizada, estructural y financiera con datos anonimizados

## 1. Alcance real de la prueba

La prueba revisó modelos, reglas financieras, almacén demostrativo, repositorios, navegación, interfaces, cálculos gerenciales y proceso de construcción de la demostración pública. No se presenta como validación final de usuario; esa aprobación corresponde al Objetivo 4.

## 2. Escenario financiero reproducido

Se utilizó un escenario determinista con:

- valor inicial: $100.000.000;
- adiciones: $20.000.000;
- deducciones: $5.000.000;
- facturación: $40.000.000;
- pago: $30.000.000;
- costos y gastos: $25.000.000.

| Resultado | Operación | Esperado | Obtenido |
|---|---|---:|---:|
| Contrato vigente | 100.000.000 + 20.000.000 − 5.000.000 | $115.000.000 | $115.000.000 |
| Avance financiero | 40.000.000 ÷ 115.000.000 × 100 | 34,78 % | 34,78 % |
| Saldo contractual | 115.000.000 − 40.000.000 | $75.000.000 | $75.000.000 |
| Pago pendiente | 40.000.000 − 30.000.000 | $10.000.000 | $10.000.000 |
| Recaudo | 30.000.000 ÷ 40.000.000 × 100 | 75 % | 75 % |
| Tasa de costos | 25.000.000 ÷ 115.000.000 × 100 | 21,74 % | 21,74 % |
| Diferencia facturación-costos | 40.000.000 − 25.000.000 | $15.000.000 | $15.000.000 |

## 3. Procesos verificados

| Proceso | Comprobación | Resultado |
|---|---|---|
| Proyecto | Normalización, creación, edición, filtros, detalle y protección ante borrado con movimientos | Aprobado |
| Contrato | Creación, actualización, cálculo vigente y restricción frente a facturación acumulada | Aprobado |
| Periodo | Un solo periodo abierto, coherencia de fechas, cierre y reapertura controlados | Aprobado |
| Factura | Asociación proyecto-contrato-periodo, número único y límite contractual | Aprobado |
| Pago | Asociación con factura, referencia única y límite del saldo | Aprobado |
| Costo/gasto | Tipo, categoría, fecha, periodo, consulta y exportación | Aprobado |
| Seguimiento | Registro mensual, validación, acumulados y bloqueo por cierre | Aprobado |
| Reporte | Totales, porcentajes ponderados, filtros y salida CSV | Aprobado |
| Auditoría | Eventos de inserción, actualización y eliminación lógica o controlada | Aprobado |
| Perfiles | Consulta, roles válidos y actualización controlada | Aprobado |
| Demostración | Datos ficticios y persistencia local sin credenciales | Aprobado |

## 4. Vistas verificadas

| Vista | Contenido funcional revisado |
|---|---|
| Inicio | KPI generales, estados, alertas y serie mensual |
| Proyectos | listado, búsqueda, filtros, CRUD y detalle |
| Detalle de proyecto | resumen financiero, contrato, seguimiento, facturas, pagos, costos e historial |
| Seguimiento mensual | periodos, creación, validación y acumulados |
| Facturación y pagos | KPI, filtros, alta de facturas y alta de pagos |
| Costos y gastos | totales diferenciados, registro, filtros y CSV |
| Reportes | consolidado gerencial, fórmulas, filtros y CSV |
| Historial | eventos, antes/después, filtros y CSV |
| Administración | perfiles existentes, roles y estado |

## 5. Casos automáticos incorporados

La suite amplió la cobertura de la versión anterior con casos específicos para:

1. asignar contrato a un proyecto recién creado;
2. actualizar el resumen financiero después de facturar;
3. registrar un pago válido;
4. rechazar un pago que exceda la factura;
5. actualizar un perfil existente;
6. comprobar que no quedan vistas de relleno;
7. consolidar totales gerenciales;
8. calcular porcentajes generales ponderados;
9. diferenciar saldo contractual y pago pendiente;
10. comprobar la presencia del reporte y su exportación.

Además se conservaron los casos de modelos, seguridad, periodos, costos, seguimiento, filtros, indicadores, navegación, construcción pública y documentación.

## 6. Evidencia y criterio

El criterio técnico de aprobación es: suite automática sin fallos, construcción pública completa, ausencia de pantallas de relleno y coincidencia exacta entre fórmulas esperadas y obtenidas.

La evidencia reproducible se encuentra en el repositorio mediante el comando **npm test** y en la ejecución de GitHub Actions asociada con la versión 1.1.0.

## 7. Conclusión

La prueba técnica confirma que la aplicación dispone del ciclo funcional necesario para comenzar el Objetivo 4. La siguiente etapa debe concentrarse en la interacción de Andrés y gerencia con datos controlados, registrar observaciones, corregir incidentes y repetir la regresión antes de la aprobación final.
