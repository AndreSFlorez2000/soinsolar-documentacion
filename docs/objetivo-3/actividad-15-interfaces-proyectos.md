# Objetivo 3 — Actividad 15

## Desarrollar las interfaces principales de gestión de proyectos

**Producto:** interfaces funcionales de gestión de proyectos  
**Formato:** enlace al repositorio  
**Periodo previsto en el PA:** 26 al 30 de octubre de 2026  
**Versión implementada:** 0.5.0

## 1. Objetivo

Desarrollar las pantallas principales que permiten registrar, consultar, actualizar y revisar integralmente los proyectos desde una interfaz web clara, coherente y adaptable a computadores, tabletas y teléfonos.

## 2. Alcance funcional

La actividad integra las siguientes interfaces:

- Listado centralizado de proyectos.
- Resumen de proyectos por estado.
- Formulario de creación.
- Formulario de actualización.
- Eliminación controlada.
- Vista individual del proyecto.
- Resumen financiero.
- Ficha descriptiva.
- Accesos relacionados con seguimiento mensual, costos y gastos e historial.

La interfaz trabaja sobre la capa de servicios y repositorios existente. De esta forma, la presentación no ejecuta consultas directas ni contiene reglas financieras duplicadas.

## 3. Interfaz de consulta de proyectos

La pantalla principal presenta cuatro indicadores de contexto:

| Indicador | Descripción |
|---|---|
| Total de proyectos | Cantidad total disponible para el usuario |
| Activos | Proyectos que se encuentran en ejecución |
| Planeados | Proyectos registrados que aún no han iniciado |
| Finalizados | Proyectos cuyo ciclo de ejecución terminó |

También incluye filtros por nombre o cliente, centro de costo, municipio y estado. La tabla muestra centro de costo, nombre, contrato, municipio, tipo de servicio, estado, avance financiero y acciones disponibles.

## 4. Registro y actualización

El formulario utiliza los campos definidos en el modelo de datos:

| Campo | Comportamiento |
|---|---|
| Centro de costo | Obligatorio y único |
| Nombre | Obligatorio |
| Cliente | Opcional |
| Municipio | Obligatorio |
| Tipo de servicio | Obligatorio |
| Potencia | Numérica y no negativa |
| Estado | Selección controlada |
| Fecha de inicio | Opcional |
| Fecha de finalización | Opcional y no puede ser anterior al inicio |
| Observaciones | Texto opcional con longitud limitada |

El mismo componente se reutiliza para crear y editar. Cuando se abre en modo edición, recupera la información vigente del proyecto y conserva su identificador.

## 5. Vista individual del proyecto

La vista individual reúne información descriptiva y financiera sin requerir búsquedas adicionales.

### Información descriptiva

- Centro de costo.
- Nombre.
- Estado.
- Cliente.
- Municipio.
- Tipo de servicio.
- Potencia instalada o proyectada.
- Número de contrato.
- Fechas de inicio y finalización.
- Observaciones.

### Información financiera

- Valor contractual vigente.
- Total facturado.
- Total pagado.
- Costos y gastos.
- Saldo contractual.
- Pago pendiente.
- Porcentaje de avance financiero acumulado.

Las cifras aplican las reglas definidas para el sistema:

- Valor contractual vigente = valor inicial + adiciones − deducciones.
- Avance financiero = total facturado ÷ valor contractual vigente × 100.
- Saldo contractual = valor contractual vigente − total facturado.
- Pago pendiente = total facturado − total pagado.

## 6. Navegación contextual

Desde el detalle se puede acceder a:

| Acción | Resultado |
|---|---|
| Seguimiento mensual | Abre los registros mensuales filtrados por el proyecto |
| Costos y gastos | Abre los movimientos filtrados por el proyecto |
| Historial | Abre la trazabilidad filtrada por el proyecto |
| Editar proyecto | Abre el formulario con la información vigente |
| Volver | Regresa al listado de proyectos |

Se retiraron controles sin una operación implementada. Esto evita botones que aparenten funcionar pero lleven a contenido vacío.

## 7. Flujo de operación

1. El usuario ingresa al módulo Proyectos.
2. El sistema carga el resumen por estados y la lista autorizada.
3. El usuario puede buscar, filtrar, crear o seleccionar un proyecto.
4. Al abrir un proyecto se consultan simultáneamente su ficha y su resumen financiero.
5. La pantalla presenta la información consolidada.
6. Las acciones relacionadas mantienen seleccionado el proyecto y aplican el filtro correspondiente.
7. Una creación o modificación actualiza la lista, los indicadores y el dashboard.
8. Cada cambio queda registrado por el mecanismo de auditoría.

## 8. Validaciones y mensajes

- Los campos obligatorios se verifican antes de enviar.
- El centro de costo no puede repetirse.
- Los valores numéricos no admiten cantidades negativas.
- Las fechas mantienen coherencia cronológica.
- La eliminación solicita confirmación.
- Un proyecto con movimientos relacionados no puede eliminarse físicamente.
- Durante las operaciones se muestra estado de procesamiento.
- Los errores se presentan dentro del formulario o módulo correspondiente.
- Cuando una búsqueda no produce resultados se muestra un estado vacío.

## 9. Experiencia de usuario y accesibilidad

- Encabezados y etiquetas describen cada sección.
- Los mensajes dinámicos usan regiones de estado accesibles.
- Los botones tienen tipo explícito para evitar envíos accidentales.
- El estado del proyecto se representa con texto y color.
- La navegación relacionada incluye una etiqueta accesible.
- Los formularios pueden utilizarse con teclado.
- El foco visible identifica las acciones rápidas.
- Los textos extensos se ajustan sin desbordar la tarjeta.

## 10. Adaptación a dispositivos

En escritorio se muestran indicadores y paneles en varias columnas. En pantallas intermedias, las tarjetas se reorganizan. En teléfonos:

- La cabecera del proyecto se presenta verticalmente.
- Los botones ocupan el ancho disponible.
- La ficha descriptiva pasa a una columna.
- Las acciones relacionadas se apilan.
- Las tablas mantienen desplazamiento horizontal controlado.
- Los formularios pasan a una sola columna.

## 11. Componentes desarrollados

| Componente | Responsabilidad |
|---|---|
| `index.html` | Estructura del listado, formularios, detalle y acciones |
| `styles.css` | Distribución, estados, adaptación móvil y foco |
| `app.js` | Carga, representación, navegación y actualización de las vistas |
| `application-data.js` | Fachada única para las operaciones de la interfaz |
| `repositories.js` | Acceso a proyectos y resumen financiero |
| `demo-store.js` | Comportamiento funcional de la demostración anonimizada |
| `project-interfaces.test.mjs` | Verificación automatizada de la estructura de interfaz |

## 12. Casos verificados

| Caso | Resultado esperado | Resultado |
|---|---|---|
| Abrir el módulo | Presenta resumen, filtros y listado | Aprobado |
| Filtrar proyectos | Muestra únicamente las coincidencias | Aprobado |
| Crear un proyecto válido | Guarda y actualiza la interfaz | Aprobado |
| Editar un proyecto | Recupera y actualiza sus datos | Aprobado |
| Abrir el detalle | Integra ficha y resumen financiero | Aprobado |
| Usar una acción relacionada | Abre el módulo con el proyecto filtrado | Aprobado |
| Consultar desde celular | Reorganiza controles y paneles | Aprobado |
| Revisar controles visibles | No existen pestañas inactivas | Aprobado |
| Verificar identificadores HTML | No existen identificadores duplicados | Aprobado |

## 13. Seguridad

La interfaz respeta los permisos de la sesión. Las operaciones reales pasan por Supabase y sus políticas RLS; la llave administrativa no se incluye en el navegador. La demostración pública utiliza exclusivamente información ficticia y guarda sus cambios de manera local.

## 14. Resultado

La Actividad 15 queda implementada mediante una interfaz funcional de gestión de proyectos que reúne consulta, filtros, creación, edición, eliminación controlada, información descriptiva, resumen financiero y navegación contextual. La estructura es consistente con los módulos desarrollados previamente y está preparada para las herramientas generales de búsqueda e indicadores de las actividades siguientes.

- Interfaz: `index.html`
- Comportamiento: `src/assets/js/app.js`
- Presentación: `src/assets/css/styles.css`
- Pruebas: `tests/project-interfaces.test.mjs`
