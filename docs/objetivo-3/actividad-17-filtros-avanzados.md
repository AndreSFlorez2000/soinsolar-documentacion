# Objetivo 3 — Actividad 17

## Implementar herramientas avanzadas de búsqueda y filtrado de información

**Producto:** buscador y filtros funcionales  
**Formato:** enlace al repositorio  
**Periodo previsto en el PA:** 29 de octubre al 3 de noviembre de 2026  
**Versión implementada:** 0.8.0

## 1. Objetivo

Profundizar las herramientas de consulta del aplicativo mediante filtros financieros y operativos combinables, con eliminación individual de criterios y comportamiento equivalente en el modo demostrativo y en el acceso real a Supabase.

## 2. Alcance

La actividad amplía la búsqueda desarrollada previamente e incorpora:

- Filtro parcial por tipo de servicio.
- Avance financiero mínimo.
- Avance financiero máximo.
- Combinación con proyecto, cliente, centro de costo, municipio y estado.
- Eliminación individual de cualquier filtro activo.
- Validación del rango porcentual.
- Búsqueda por cliente en la vista financiera protegida.
- Aplicación equivalente en el repositorio real y en la demostración.

## 3. Criterios disponibles para proyectos

| Criterio | Tipo de coincidencia |
|---|---|
| Proyecto o cliente | Parcial |
| Centro de costo | Parcial |
| Municipio | Parcial |
| Estado | Exacta según catálogo |
| Tipo de servicio | Parcial |
| Avance mínimo | Mayor o igual al porcentaje |
| Avance máximo | Menor o igual al porcentaje |

Los criterios pueden aplicarse de manera independiente o simultánea. El resultado debe cumplir todas las condiciones seleccionadas.

## 4. Rango de avance financiero

El porcentaje utilizado en el filtro no se digita dentro de los datos del proyecto. Se obtiene de la regla financiera:

Avance financiero = total facturado ÷ valor contractual vigente × 100.

El rango admite valores entre 0 y 100. La aplicación impide:

- Valores negativos.
- Valores superiores a 100.
- Valores no numéricos.
- Un mínimo superior al máximo.

Si el rango es inválido, la consulta no se ejecuta y se presenta un mensaje dentro del módulo.

## 5. Eliminación individual de criterios

Cada filtro aplicado se representa como una etiqueta interactiva. Al seleccionarla:

1. Se identifica el campo asociado.
2. Se limpia únicamente ese campo.
3. El formulario conserva los demás criterios.
4. Se ejecuta nuevamente la consulta.
5. Se actualizan la tabla, el contador y las etiquetas.

El botón Limpiar continúa disponible para restablecer todos los filtros simultáneamente.

## 6. Acceso real a los datos

El repositorio de proyectos construye la consulta sobre la vista protegida project_financial_summary:

- Coincidencia combinada entre nombre y cliente.
- Coincidencia parcial para centro de costo, municipio y tipo de servicio.
- Igualdad para estado.
- Comparadores mayor o igual y menor o igual para el avance.
- Orden alfabético por proyecto.

La migración 202609140001_search_dashboard.sql incorpora el cliente como columna adicional de la vista sin exponer información fuera de las políticas RLS.

## 7. Modo demostrativo

La demostración utiliza la misma estructura de filtros sobre datos ficticios. Para cada proyecto calcula primero su resumen financiero y después verifica el rango solicitado. Esto permite demostrar el comportamiento completo sin utilizar registros empresariales ni credenciales reales.

## 8. Flujo de consulta

1. El usuario abre Proyectos.
2. Selecciona criterios operativos o financieros.
3. La interfaz valida los porcentajes.
4. La capa de aplicación envía un objeto de filtros.
5. El repositorio aplica las condiciones.
6. Se normalizan los resultados.
7. La tabla y el contador se actualizan.
8. Los criterios activos permanecen visibles.
9. El usuario puede retirar uno o limpiar todos.

## 9. Casos verificados

| Caso | Resultado esperado | Resultado |
|---|---|---|
| Filtrar por tipo de servicio | Muestra coincidencias parciales | Aprobado |
| Definir avance mínimo | Excluye porcentajes inferiores | Aprobado |
| Definir avance máximo | Excluye porcentajes superiores | Aprobado |
| Combinar servicio, estado y avance | Cumple todas las condiciones | Aprobado |
| Mínimo superior al máximo | Impide la consulta y muestra error | Aprobado |
| Quitar una etiqueta | Retira solo ese criterio | Aprobado |
| Buscar por cliente | Consulta la columna protegida | Aprobado |
| Limpiar todo | Restablece la consulta general | Aprobado |

## 10. Seguridad

Los filtros no alteran las políticas de acceso. La base de datos limita primero los registros mediante RLS y luego aplica los criterios solicitados. Los valores textuales se limpian antes de construir condiciones combinadas y la aplicación utiliza exclusivamente la llave publicable.

## 11. Componentes

| Componente | Responsabilidad |
|---|---|
| index.html | Campos avanzados y etiquetas interactivas |
| app.js | Validación, lectura y eliminación de criterios |
| repositories.js | Filtros sobre Supabase |
| demo-store.js | Filtros sobre datos ficticios |
| models.js | Cliente dentro del resumen normalizado |
| 202609140001_search_dashboard.sql | Ampliación segura de la vista |
| advanced-search-filters.test.mjs | Pruebas de combinaciones y acceso |

## 12. Resultado

La Actividad 17 queda implementada mediante filtros avanzados por tipo de servicio y rango de avance, combinación de criterios, eliminación individual y soporte equivalente en la base de datos y la demostración.

- Interfaz: index.html
- Lógica: src/assets/js/app.js
- Acceso a datos: src/assets/js/data/repositories.js
- Migración: supabase/migrations/202609140001_search_dashboard.sql
- Pruebas: tests/advanced-search-filters.test.mjs
