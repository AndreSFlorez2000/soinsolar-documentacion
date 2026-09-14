# Objetivo 3 — Actividad 13

## Implementar el manejo de periodos y registros mensuales

**Producto:** módulo de periodos y seguimiento mensual  
**Formato:** enlace al repositorio  
**Periodo previsto en el PA:** 21 al 26 de octubre de 2026  
**Versión implementada:** 0.4.0

## 1. Objetivo

Implementar el control mensual de la operación financiera de los proyectos. El módulo organiza la información por mes y año, calcula el avance financiero desde la facturación, permite validar el seguimiento de cada proyecto y protege los registros cuando el periodo se cierra.

## 2. Alcance funcional

El módulo permite:

- Consultar los periodos en orden cronológico descendente.
- Crear un periodo mensual con año y mes.
- Mantener como máximo un periodo abierto.
- Consultar el seguimiento por proyecto, periodo y estado de validación.
- Registrar o actualizar una sola ficha de seguimiento por proyecto y periodo.
- Asociar la ficha con el contrato vigente del proyecto.
- Registrar el valor reconocido del mes y las observaciones administrativas.
- Calcular el valor facturado, los costos y los porcentajes sin digitarlos manualmente.
- Validar el seguimiento mensual.
- Cerrar el periodo cuando todos los proyectos con actividad tienen seguimiento validado.
- Reabrir un periodo cerrado cuando no existe otro periodo abierto.
- Bloquear cambios de seguimiento, facturas, pagos, costos y gastos en periodos cerrados.

## 3. Datos administrados

### Periodo

| Campo | Uso | Regla implementada |
|---|---|---|
| Año | Año contable | Entero entre 2020 y 2100 |
| Mes | Mes contable | Entero entre 1 y 12 |
| Estado | Habilitación del periodo | Abierto o cerrado |
| Fecha de cierre | Momento exacto del cierre | Se asigna automáticamente |
| Cerrado por | Usuario que ejecuta el cierre | Se toma de la sesión autenticada |

La combinación año–mes es única. Además, un índice parcial de PostgreSQL impide que existan dos periodos abiertos simultáneamente.

### Seguimiento mensual

| Campo | Uso | Origen o regla |
|---|---|---|
| Proyecto | Proyecto controlado | Obligatorio |
| Contrato | Contrato vigente | Debe pertenecer al proyecto |
| Periodo | Mes del seguimiento | Obligatorio y abierto para edición |
| Valor reconocido | Avance económico reconocido en el mes | No negativo; el acumulado no supera el contrato |
| Valor facturado del mes | Facturas vigentes del proyecto en el periodo | Calculado |
| Valor pagado del mes | Pagos vigentes del proyecto en el periodo | Calculado |
| Costos y gastos del mes | Movimientos del proyecto en el periodo | Calculado |
| Estado de validación | Flujo de revisión | Borrador, pendiente, validado o rechazado |
| Observaciones | Explicación del registro | Opcional |
| Fecha de validación | Evidencia temporal | Automática al validar |

La base de datos garantiza un único seguimiento por proyecto y periodo.

## 4. Cálculos financieros del módulo

El porcentaje no se captura manualmente. Se deriva de los datos financieros registrados:

```text
Valor contractual vigente = valor inicial + adiciones − deducciones
Avance financiero mensual = facturado del mes ÷ valor contractual vigente × 100
Facturación acumulada = suma de facturas vigentes hasta el periodo consultado
Avance financiero acumulado = facturación acumulada ÷ valor contractual vigente × 100
Saldo contractual = valor contractual vigente − total facturado
Pago pendiente = total facturado − total pagado
```

El valor del contrato constituye el límite de facturación. Los pagos se registran contra facturas y no pueden superar el valor facturado correspondiente. De esta forma, avance, saldo contractual y pago pendiente representan conceptos distintos.

## 5. Flujo de operación

1. El administrador consulta el periodo activo.
2. Registra las facturas, pagos, costos y gastos del mes en sus módulos correspondientes.
3. Abre Seguimiento mensual y selecciona el proyecto y el periodo.
4. Registra el valor reconocido y las observaciones.
5. La aplicación obtiene automáticamente los valores financieros relacionados.
6. El sistema calcula el avance mensual y el acumulado.
7. El registro pasa de borrador a pendiente y posteriormente a validado.
8. Al solicitar el cierre, la base de datos comprueba todos los proyectos con actividad.
9. Si falta alguna validación, el cierre es rechazado e informa la causa.
10. Si la comprobación se supera, el periodo queda cerrado y sus movimientos se vuelven de solo consulta.

## 6. Reglas de integridad

- Solo existe un periodo abierto.
- Año y mes no pueden repetirse.
- Proyecto, contrato y periodo deben existir.
- El contrato debe corresponder al proyecto.
- Solo existe un seguimiento por proyecto y periodo.
- El valor reconocido no puede ser negativo ni superar acumuladamente el contrato.
- El avance financiero se calcula desde las facturas no anuladas.
- El seguimiento validado registra usuario y fecha de validación.
- El cierre requiere validación para cada proyecto activo con movimientos en el periodo.
- Un periodo cerrado rechaza inserciones, actualizaciones y eliminaciones financieras.
- La reapertura queda auditada y solo procede si no hay otro periodo abierto.

## 7. Componentes desarrollados

| Componente | Responsabilidad |
|---|---|
| `index.html` | Pantalla, filtros, indicadores, tablas y formularios |
| `styles.css` | Estados visuales, tablas y adaptación móvil |
| `app.js` | Creación, cierre, reapertura, registro, validación y mensajes |
| `models.js` | Validación y normalización de periodos y seguimiento |
| `demo-store.js` | Reglas funcionales y persistencia local de la demostración |
| `repositories.js` | Consultas y escrituras sobre Supabase |
| `202609110001_initial_schema.sql` | Tablas, claves y unicidad de periodos |
| `202609110002_integrity_security.sql` | Bloqueos, cierre, cálculos, auditoría, vista y RLS |

## 8. Casos verificados

| Caso | Resultado esperado | Resultado |
|---|---|---|
| Crear un segundo periodo abierto | La operación es rechazada | Aprobado |
| Repetir proyecto y periodo | Se actualiza el registro existente, sin duplicarlo | Aprobado |
| Consultar por proyecto, periodo o validación | Solo aparecen coincidencias | Aprobado |
| Calcular avance mensual | Utiliza facturado del mes ÷ contrato vigente | Aprobado |
| Calcular avance acumulado | Acumula la facturación hasta el periodo | Aprobado |
| Cerrar con seguimientos pendientes | La operación es rechazada | Aprobado |
| Validar y cerrar el periodo | El periodo queda cerrado | Aprobado |
| Modificar un periodo cerrado | La operación es rechazada | Aprobado |
| Reabrir mientras existe otro abierto | La operación es rechazada | Aprobado |

## 9. Seguridad y trazabilidad

En el entorno conectado, las operaciones requieren sesión activa y permisos de escritura. Las reglas críticas también se ejecutan en PostgreSQL para que no puedan evitarse mediante cambios en la interfaz. La creación, validación, cierre y reapertura quedan registradas automáticamente en la auditoría.

La demostración pública usa únicamente proyectos ficticios y conserva los cambios en el almacenamiento del navegador.

## 10. Resultado

La Actividad 13 queda implementada como un módulo funcional de periodos y seguimiento mensual. La interfaz ofrece creación, consulta, filtros, validación, cierre y reapertura; la capa de datos conserva las reglas en modo demostrativo y Supabase; y las pruebas automáticas comprueban la integridad del proceso.

- Código principal: `src/assets/js/app.js`
- Modelos: `src/assets/js/data/models.js`
- Persistencia demostrativa: `src/assets/js/data/demo-store.js`
- Persistencia real: `src/assets/js/data/repositories.js`
- Pruebas: `tests/period-history-modules.test.mjs`
