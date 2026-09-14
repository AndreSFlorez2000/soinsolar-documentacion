# Objetivo 3 — Actividad 14

## Implementar la lógica para conservar la trazabilidad histórica

**Producto:** historial funcional de cambios  
**Formato:** enlace al repositorio  
**Periodo previsto en el PA:** 23 al 28 de octubre de 2026  
**Versión implementada:** 0.4.0

## 1. Objetivo

Conservar evidencia cronológica de las operaciones relevantes del aplicativo para responder qué registro cambió, cuándo ocurrió, qué acción se realizó y cuáles eran sus valores antes y después.

## 2. Alcance funcional

La trazabilidad cubre:

- Proyectos.
- Contratos y modificaciones de sus valores.
- Periodos y cambios entre abierto y cerrado.
- Seguimientos mensuales y su validación.
- Facturas.
- Pagos.
- Costos y gastos.

El módulo permite consultar los eventos más recientes, filtrar el historial, abrir el detalle antes/después y exportar la consulta visible a CSV.

## 3. Estructura del evento de auditoría

| Campo | Propósito |
|---|---|
| Identificador | Distingue el evento de forma única |
| Tabla | Indica el módulo que originó el cambio |
| Registro | Identifica el elemento modificado |
| Acción | INSERT, UPDATE o DELETE |
| Usuario | Usuario autenticado que ejecutó la operación |
| Fecha y hora | Marca temporal generada por la base de datos |
| Datos anteriores | Copia del registro antes del cambio |
| Datos nuevos | Copia del registro después del cambio |
| Proyecto relacionado | Se deriva del registro para permitir el filtro por proyecto |

## 4. Funcionamiento

1. El usuario realiza una operación en un módulo funcional.
2. PostgreSQL ejecuta el cambio autorizado.
3. Un trigger posterior genera automáticamente el evento de auditoría.
4. Para una creación se conserva el estado nuevo; para una eliminación se conserva el estado anterior; para una actualización se conservan ambos.
5. La interfaz consulta los eventos en orden descendente.
6. El usuario puede filtrar por proyecto, módulo, acción y rango de fechas.
7. El botón Ver cambio presenta un resumen y los datos anteriores y posteriores.
8. La exportación CSV utiliza exactamente el conjunto filtrado que se ve en pantalla.

## 5. Reglas implementadas

- La auditoría se genera en la base de datos y no depende de que el frontend la envíe.
- La fecha y la hora son automáticas.
- El usuario se toma de la sesión autenticada mediante `auth.uid()`.
- La aplicación no ofrece operaciones para editar ni eliminar eventos históricos.
- La tabla de auditoría solo permite consulta a usuarios activos autorizados.
- Los cambios de campos técnicos se excluyen del resumen para destacar la información útil.
- Los datos completos antes y después siguen disponibles en el detalle.
- El filtro de fechas valida que la fecha inicial no sea posterior a la final.
- El modo demostrativo reproduce la misma estructura sin almacenar información real.

## 6. Protección del histórico financiero

La trazabilidad no se limita a guardar eventos. También protege la coherencia histórica:

- Los proyectos con movimientos no se eliminan; cambian de estado.
- Las facturas anuladas se conservan con su estado.
- Los pagos anulados se conservan con su estado.
- Los periodos cerrados dejan sus movimientos en modo consulta.
- Una reapertura registra un nuevo evento y no borra el cierre anterior.
- Los cálculos de cada periodo se reconstruyen desde contratos, facturas, pagos, costos y gastos conservados.

## 7. Consultas disponibles

| Filtro | Comportamiento |
|---|---|
| Proyecto | Muestra los cambios asociados con un proyecto |
| Módulo | Limita la consulta a una tabla funcional |
| Acción | Separa creaciones, modificaciones y eliminaciones |
| Desde | Incluye eventos desde el inicio del día seleccionado |
| Hasta | Incluye eventos hasta el final del día seleccionado |

La consulta real limita inicialmente la respuesta a los 500 eventos más recientes para mantener una carga rápida en esta versión básica. Los filtros de módulo, acción y fecha se aplican en la base de datos; la relación con el proyecto se determina a partir de las copias auditadas.

## 8. Componentes desarrollados

| Componente | Responsabilidad |
|---|---|
| `index.html` | Filtros, tabla, contador, exportación y detalle |
| `styles.css` | Presentación de acciones, comparación y estados |
| `app.js` | Consulta, resumen de cambios, filtros y CSV |
| `models.js` | Normalización del evento y relación con proyecto |
| `demo-store.js` | Generación y consulta de auditoría demostrativa |
| `repositories.js` | Consulta protegida de `audit_log` |
| `202609110002_integrity_security.sql` | Función, triggers, permisos y políticas RLS |

## 9. Casos verificados

| Caso | Resultado esperado | Resultado |
|---|---|---|
| Crear un proyecto | Aparece un evento de creación | Aprobado |
| Modificar un seguimiento | Conserva estado anterior y nuevo | Aprobado |
| Validar un seguimiento | Registra el cambio de validación | Aprobado |
| Cerrar o reabrir un periodo | Registra la transición de estado | Aprobado |
| Registrar un costo | Genera un evento asociado con el proyecto | Aprobado |
| Filtrar por proyecto, módulo y acción | Solo aparecen coincidencias | Aprobado |
| Consultar el detalle | Presenta antes y después | Aprobado |
| Exportar la consulta | Genera CSV con las filas visibles | Aprobado |

## 10. Seguridad

La función de auditoría usa un contexto controlado y no se expone para ejecución pública. Los permisos sobre las tablas se conceden únicamente al rol autenticado y se complementan con políticas RLS. La interfaz utiliza la llave publicable y nunca incorpora la llave administrativa `service_role`.

## 11. Resultado

La Actividad 14 queda implementada con auditoría automática, consulta cronológica, filtros, comparación antes/después y exportación. Las pruebas comprueban la creación de eventos, la derivación del proyecto relacionado, la conservación de ambos estados y la consulta combinada por criterios.

- Código principal: `src/assets/js/app.js`
- Modelo de auditoría: `src/assets/js/data/models.js`
- Repositorio: `src/assets/js/data/repositories.js`
- Migración de seguridad: `supabase/migrations/202609110002_integrity_security.sql`
- Pruebas: `tests/period-history-modules.test.mjs`
