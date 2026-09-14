# Objetivo 3 — Actividad 16

## Implementar herramientas de búsqueda y filtrado de información

**Producto:** buscador global y filtros funcionales  
**Formato:** enlace al repositorio  
**Periodo previsto en el PA:** 28 de octubre al 2 de noviembre de 2026  
**Versión implementada:** 0.6.0

## 1. Objetivo

Facilitar la localización de proyectos y la consulta específica de información mediante un buscador global y filtros especializados para proyectos, seguimiento mensual, costos, gastos e historial.

## 2. Necesidad atendida

Los archivos utilizados anteriormente exigían recorridos manuales entre hojas y búsquedas visuales. El aplicativo centraliza los criterios de consulta, reduce el tiempo requerido para encontrar un proyecto y muestra de forma explícita qué filtros están afectando los resultados.

## 3. Búsqueda global de proyectos

La barra superior incorpora un buscador disponible desde todos los módulos. La búsqueda compara el texto ingresado con:

- Nombre del proyecto.
- Centro de costo.
- Municipio.
- Tipo de servicio.
- Número de contrato.
- Estado del proyecto.

La comparación ignora diferencias entre mayúsculas, minúsculas y tildes. Se muestran hasta ocho coincidencias inmediatas para conservar una interacción rápida y sencilla.

Cada resultado presenta el nombre del proyecto, centro de costo, municipio, estado y una acción para abrir directamente su vista individual.

## 4. Operación del buscador

1. El usuario escribe uno o varios caracteres.
2. La aplicación normaliza el texto.
3. Se consultan en memoria los proyectos autorizados que ya fueron recuperados.
4. Se comparan todos los campos definidos.
5. Los resultados coincidentes aparecen debajo del buscador.
6. El usuario selecciona un resultado o presiona Enter para abrir el primero.
7. La aplicación conserva el proyecto seleccionado y abre su detalle.
8. Escape o el botón de limpieza restablecen la búsqueda.

Si no existen coincidencias, la interfaz muestra un mensaje de resultado vacío sin modificar la pantalla actual.

## 5. Filtros del módulo de proyectos

| Criterio | Comportamiento |
|---|---|
| Proyecto o cliente | Coincidencia parcial por nombre o cliente |
| Centro de costo | Coincidencia parcial del identificador contable |
| Municipio | Coincidencia parcial por ubicación |
| Estado | Coincidencia exacta con el catálogo de estados |

Los filtros se pueden combinar. El contador presenta la cantidad de proyectos encontrados y las etiquetas de criterios activos permiten entender la consulta aplicada.

## 6. Filtros de seguimiento mensual

| Criterio | Comportamiento |
|---|---|
| Proyecto | Limita el seguimiento a un proyecto |
| Periodo | Limita el resultado a un mes y año |
| Validación | Separa borradores, pendientes, validados y rechazados |

Cuando el usuario llega desde el detalle de un proyecto, el sistema aplica automáticamente el filtro correspondiente.

## 7. Filtros de costos y gastos

| Criterio | Comportamiento |
|---|---|
| Proyecto | Selecciona los movimientos de un proyecto |
| Periodo | Selecciona los movimientos de un periodo |
| Tipo | Separa costos y gastos |
| Categoría | Realiza coincidencia parcial |
| Desde | Define la fecha inicial |
| Hasta | Define la fecha final |

Los criterios se aplican también a los totales y a la exportación CSV. La fecha inicial no puede ser posterior a la fecha final.

## 8. Filtros de historial

| Criterio | Comportamiento |
|---|---|
| Proyecto | Muestra los cambios relacionados con un proyecto |
| Módulo | Filtra proyectos, periodos, contratos, facturación, pagos, costos o seguimiento |
| Acción | Separa creación, modificación y eliminación |
| Desde | Inicio del intervalo |
| Hasta | Fin del intervalo |

El resultado filtrado conserva el acceso a la comparación de valores anteriores y nuevos.

## 9. Visualización de filtros activos

Debajo de cada formulario se presentan etiquetas con el nombre y valor de cada criterio utilizado. Esta solución permite:

- Confirmar la consulta aplicada.
- Detectar filtros olvidados.
- Interpretar correctamente los contadores y totales.
- Diferenciar una consulta general de una consulta específica.
- Limpiar todos los criterios desde una acción visible.

Los filtros sin valor no generan etiquetas.

## 10. Flujo general

1. El usuario ingresa al módulo.
2. La interfaz recupera la información autorizada.
3. El usuario selecciona uno o varios criterios.
4. La aplicación valida combinaciones y rangos.
5. La capa de datos ejecuta la consulta.
6. Se actualizan filas, contadores, totales y etiquetas.
7. Limpiar filtros restablece el formulario y la consulta general.
8. Los resultados vacíos se comunican sin producir errores.

## 11. Usabilidad y accesibilidad

- El buscador incluye etiqueta accesible.
- El estado expandido se comunica mediante aria-expanded.
- Los resultados se agrupan en una región identificable.
- Enter abre la primera coincidencia.
- Escape limpia y cierra.
- El foco visible permite identificar cada resultado.
- El botón de limpieza tiene descripción accesible.
- Los filtros activos se anuncian como contenido dinámico.
- Los estados no dependen únicamente del color.
- Los campos de búsqueda utilizan el tipo HTML correspondiente.

## 12. Adaptación a dispositivos

En escritorio el buscador comparte la cabecera con el periodo y la sesión. En teléfonos ocupa todo el ancho disponible y los resultados se ajustan al contenedor. Los formularios de filtros cambian progresivamente de varias columnas a una sola, conservando botones accesibles y tablas con desplazamiento horizontal.

## 13. Seguridad y privacidad

La búsqueda global trabaja únicamente con proyectos que la sesión ya tiene autorización para consultar. No amplía permisos ni evita las políticas RLS. Las consultas reales pasan por la capa de repositorios y utilizan la llave publicable. La demostración pública contiene exclusivamente información ficticia.

No se guardan contraseñas, llaves administrativas ni términos de búsqueda en el repositorio.

## 14. Componentes desarrollados

| Componente | Responsabilidad |
|---|---|
| index.html | Buscador, resultados y contenedores de filtros activos |
| app.js | Normalización, coincidencias, teclado, limpieza y representación de criterios |
| styles.css | Presentación del buscador, resultados, etiquetas y adaptación móvil |
| repositories.js | Consultas filtradas sobre la información autorizada |
| demo-store.js | Aplicación de filtros en la demostración local |
| search-filter-tools.test.mjs | Verificaciones automatizadas de la actividad |

## 15. Casos verificados

| Caso | Resultado esperado | Resultado |
|---|---|---|
| Buscar por nombre | Muestra proyectos coincidentes | Aprobado |
| Buscar por centro de costo | Localiza el proyecto correspondiente | Aprobado |
| Buscar sin distinguir tildes | Mantiene la coincidencia | Aprobado |
| Buscar un valor inexistente | Muestra estado vacío | Aprobado |
| Presionar Enter | Abre la primera coincidencia | Aprobado |
| Presionar Escape | Limpia y cierra el buscador | Aprobado |
| Combinar filtros | Aplica simultáneamente los criterios | Aprobado |
| Usar un rango inválido | Muestra error y no ejecuta la consulta | Aprobado |
| Limpiar filtros | Restablece la consulta general | Aprobado |
| Consultar desde celular | Ajusta buscador, resultados y formularios | Aprobado |

## 16. Resultado

La Actividad 16 queda implementada mediante un buscador global de proyectos, filtros combinables por módulo, validación de rangos, indicadores de criterios activos, estados vacíos y navegación directa al detalle. La versión conserva una operación básica, clara y visualmente consistente con el resto del aplicativo.

- Interfaz: index.html
- Lógica: src/assets/js/app.js
- Estilos: src/assets/css/styles.css
- Pruebas: tests/search-filter-tools.test.mjs
