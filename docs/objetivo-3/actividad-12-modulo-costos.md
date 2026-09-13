# Objetivo 3 — Actividad 12

## Desarrollar las funcionalidades para el registro y consulta de costos

**Producto:** módulo de costos  
**Formato:** enlace al repositorio  
**Periodo previsto en el PA:** 19 al 23 de octubre de 2026  
**Versión implementada:** 0.3.0

## 1. Objetivo

Implementar el módulo que registra y consulta costos y gastos de cada proyecto, conservando su centro de costo, periodo mensual, fecha, clasificación, soporte y valor monetario.

## 2. Alcance funcional

El módulo permite:

- Registrar movimientos clasificados como costo o gasto.
- Asociar cada movimiento con un proyecto y un periodo mensual.
- Registrar categoría, descripción, proveedor, referencia documental, fecha, valor y ubicación del soporte.
- Consultar todos los movimientos autorizados.
- Filtrar por proyecto, periodo, tipo, categoría y rango de fechas.
- Mostrar por separado el total de costos y el total de gastos.
- Mostrar el total general de la consulta filtrada.
- Exportar a CSV exactamente los registros visibles en la consulta.
- Reflejar el nuevo valor en el dashboard y en el detalle financiero del proyecto.
- Conservar los movimientos demostrativos en el navegador.

## 3. Estructura del movimiento

| Campo | Uso | Regla |
|---|---|---|
| Proyecto | Proyecto que origina el movimiento | Obligatorio y existente |
| Periodo | Mes contable del registro | Obligatorio y abierto |
| Tipo | Diferencia costo de gasto | Costo o gasto |
| Categoría | Agrupación para consulta y reporte | Obligatoria |
| Descripción | Explicación del movimiento | Obligatoria, mínimo 3 caracteres |
| Proveedor | Tercero relacionado | Opcional |
| Referencia documental | Factura, comprobante u otro soporte | Opcional |
| Fecha | Fecha del movimiento | Debe pertenecer al periodo seleccionado |
| Valor | Importe en pesos colombianos | Mayor que cero |
| Ruta del soporte | Referencia al documento digital | Opcional |

## 4. Flujo de registro

1. El usuario abre Costos y gastos.
2. Selecciona Nuevo movimiento.
3. La aplicación carga proyectos y periodos abiertos.
4. El usuario diligencia y envía el formulario.
5. El modelo valida identificadores, textos, fecha, tipo y valor.
6. Se comprueba que el periodo esté abierto y corresponda al mes de la fecha.
7. El repositorio registra el movimiento.
8. La base de datos aplica relaciones, restricciones, RLS y auditoría.
9. La interfaz actualiza totales, tabla, dashboard y detalle del proyecto.

## 5. Consultas y filtros

Los filtros pueden aplicarse de manera individual o combinada:

| Filtro | Comportamiento |
|---|---|
| Proyecto | Consulta los movimientos de un proyecto específico |
| Periodo | Limita el resultado a un mes y año |
| Tipo | Separa costos de gastos |
| Categoría | Busca coincidencias parciales |
| Desde y hasta | Consulta un rango cronológico válido |

La suma mostrada utiliza únicamente los registros que cumplen los filtros. La exportación conserva esa misma selección para evitar diferencias entre pantalla y archivo descargado.

## 6. Reglas implementadas

- Todo costo o gasto pertenece a un proyecto.
- Todo movimiento pertenece a un periodo mensual.
- El valor debe ser positivo.
- La fecha debe ser válida y corresponder al periodo.
- Un periodo cerrado permite consulta, pero no nuevos registros.
- La categoría y la descripción no pueden quedar vacías.
- El costo y el gasto conservan clasificaciones separadas.
- Los movimientos actualizan el consolidado financiero sin alterar facturación ni pagos.
- Las operaciones en Supabase quedan protegidas con RLS y registradas en auditoría.

## 7. Componentes desarrollados

| Componente | Responsabilidad |
|---|---|
| `index.html` | Filtros, indicadores, tabla y formulario de movimientos |
| `styles.css` | Diseño adaptable de la consulta y del formulario |
| `app.js` | Registro, filtros, totales, actualización y exportación |
| `models.js` | Validación y normalización de costos y gastos |
| `demo-store.js` | Persistencia y reglas del modo demostrativo |
| `repositories.js` | Consultas e inserciones sobre `costs_expenses` |
| `application-data.js` | Conexión común para modo demo y Supabase |

## 8. Casos verificados

| Caso | Resultado esperado | Resultado |
|---|---|---|
| Registrar un costo válido | Se agrega y actualiza el total | Aprobado |
| Filtrar por proyecto y periodo | Solo aparecen los movimientos asociados | Aprobado |
| Buscar una categoría | Se reconocen coincidencias parciales | Aprobado |
| Registrar valor cero | La operación es rechazada | Aprobado |
| Usar un tipo no permitido | La operación es rechazada | Aprobado |
| Registrar en un periodo cerrado | La operación es rechazada | Aprobado |
| Usar una fecha de otro mes | La operación es rechazada | Aprobado |

## 9. Seguridad e integridad

- Las tablas de proyectos, periodos y movimientos se relacionan mediante UUID.
- La base de datos verifica la existencia del proyecto y el periodo.
- El frontend no utiliza credenciales administrativas.
- RLS controla lectura e inserción para usuarios autenticados y activos.
- Los cambios quedan disponibles para el registro de auditoría.
- La demostración pública trabaja exclusivamente con información ficticia.

## 10. Verificación y resultado

La Actividad 12 queda implementada con registro, consulta, filtros, totales y exportación. Las pruebas automáticas verifican normalización, clasificación, periodos, fechas, valores, filtros y actualización de totales.

- [Diagramas del sistema](./diagramas-sistema.md)
- Código principal: `src/assets/js/app.js`
- Repositorio financiero: `src/assets/js/data/repositories.js`
- Pruebas: `tests/project-cost-modules.test.mjs`

