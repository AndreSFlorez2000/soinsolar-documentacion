# Objetivo 3 — Actividad 18

## Implementar indicadores y estadísticas generales

**Producto:** dashboard general funcional  
**Formato:** enlace al repositorio  
**Periodo previsto en el PA:** 30 de octubre al 4 de noviembre de 2026  
**Versión implementada:** 0.8.0

## 1. Objetivo

Implementar un tablero general que convierta la información registrada en indicadores financieros, operativos y mensuales útiles para la consulta y toma de decisiones de la administración y la gerencia.

## 2. Principio de cálculo

Los indicadores no se escriben manualmente en la interfaz. Se recalculan desde los proyectos, contratos, facturas, pagos, costos, gastos, periodos y seguimientos que la sesión está autorizada para consultar.

La lógica matemática se encuentra separada de la presentación en el módulo domain/dashboard.js. Esto permite probar los resultados sin depender del navegador.

## 3. Indicadores financieros principales

| Indicador | Cálculo |
|---|---|
| Valor contractual vigente | Suma de los valores contractuales vigentes |
| Total facturado | Suma de facturas activas |
| Total pagado | Suma de pagos activos |
| Costos y gastos | Suma de movimientos registrados |
| Saldo contractual | Suma del valor contractual menos lo facturado |
| Pago pendiente | Suma del valor facturado menos lo pagado |
| Avance financiero global | Facturación total ÷ valor contractual total × 100 |
| Relación costos/contrato | Costos y gastos ÷ valor contractual total × 100 |

El avance global es ponderado por el valor de los contratos. No corresponde al promedio simple de los porcentajes de cada proyecto.

## 4. Indicadores operativos

El dashboard presenta:

- Total de proyectos registrados.
- Cantidad de proyectos activos.
- Distribución entre planeados, activos, suspendidos, finalizados y cancelados.
- Proyectos con mayor porcentaje de avance.
- Cantidad de seguimientos pendientes de validación.
- Periodo actualmente abierto.

La distribución por estado incluye cantidad y proporción visual respecto del total.

## 5. Comportamiento mensual

Los registros mensuales se agrupan por año y mes. Para cada periodo se suman:

- Valor facturado.
- Valor pagado.
- Costos y gastos.

Los periodos se ordenan cronológicamente y se muestran los seis más recientes. Los valores del gráfico se expresan en millones de pesos para facilitar su lectura.

Cuando la biblioteca gráfica no está disponible, la aplicación genera una representación alternativa. Si no existen movimientos, muestra un estado vacío en lugar de inventar valores.

## 6. Alertas generales

Las alertas también se derivan de los registros:

| Condición | Alerta |
|---|---|
| Pago pendiente superior a cero | Valor facturado sin pago registrado |
| Existe un periodo abierto | El periodo todavía admite modificaciones |
| Hay seguimientos sin validar | Deben revisarse antes del cierre |
| No se presenta ninguna condición | Mensaje de operación sin alertas |

La interfaz limita el panel a las alertas más relevantes para mantener un tablero básico y legible.

## 7. Actualización del dashboard

El tablero se actualiza:

- Al iniciar el aplicativo.
- Al regresar a Inicio.
- Después de crear o modificar un proyecto.
- Después de eliminar controladamente un proyecto.
- Cuando se recargan los datos de referencia.

La carga inicial obtiene simultáneamente proyectos, periodos y seguimiento mensual. Los cálculos se realizan únicamente después de recibir y normalizar la información.

## 8. Exportación

El botón Exportar resumen CSV genera una evidencia con:

- Total de proyectos.
- Proyectos activos.
- Valor contractual.
- Total facturado.
- Total pagado.
- Costos y gastos.
- Saldo contractual.
- Pago pendiente.
- Avance financiero global.
- Relación entre costos y contrato.

El archivo utiliza los mismos indicadores visibles para evitar diferencias entre pantalla y exportación.

## 9. Tratamiento de casos especiales

- Si no hay proyectos, los valores se presentan en cero.
- Los valores inexistentes o no numéricos no contaminan los totales.
- No se realizan divisiones cuando el valor contractual es cero.
- Los meses inválidos no se incorporan a la serie.
- Los proyectos sin un estado reconocido cuentan en el total, pero no alteran los catálogos.
- Los saldos se toman del resumen financiero normalizado.
- El gráfico anterior se destruye antes de dibujar una actualización para evitar duplicados.

## 10. Experiencia de usuario

- Los indicadores utilizan títulos breves y valores destacados.
- Los saldos se diferencian de los valores ejecutados.
- Los estados se presentan con texto, cantidad, color y barra proporcional.
- El gráfico incluye leyenda para facturación, pagos y costos.
- Las alertas explican la condición y su impacto.
- La tabla permite abrir directamente un proyecto.
- La distribución cambia a dos columnas y luego a una según el tamaño de pantalla.

## 11. Seguridad y privacidad

Los cálculos utilizan únicamente registros que superaron la autenticación y las políticas RLS. El navegador no necesita permisos administrativos. La demo pública ejecuta los mismos cálculos con información ficticia y no publica cifras empresariales.

## 12. Componentes

| Componente | Responsabilidad |
|---|---|
| domain/dashboard.js | Totales, porcentajes, estados y series mensuales |
| app.js | Presentación, alertas, gráfico y exportación |
| index.html | Indicadores, paneles y tabla |
| styles.css | Distribución y representación visual |
| application-data.js | Carga unificada de información |
| dashboard-indicators.test.mjs | Pruebas matemáticas y cronológicas |

## 13. Casos verificados

| Caso | Resultado esperado | Resultado |
|---|---|---|
| Sumar dos proyectos | Consolida todos los valores | Aprobado |
| Calcular avance global | Usa ponderación contractual | Aprobado |
| Calcular relación de costos | Divide por el contrato total | Aprobado |
| Clasificar estados | Cuenta cada estado correctamente | Aprobado |
| Agrupar varios proyectos en un mes | Suma sus movimientos | Aprobado |
| Ordenar meses | Mantiene orden cronológico | Aprobado |
| Limitar la serie | Conserva los seis más recientes | Aprobado |
| No tener información | Presenta ceros y estado vacío | Aprobado |
| Exportar | Genera los indicadores visibles | Aprobado |

## 14. Resultado

La Actividad 18 queda implementada con indicadores generales calculados, distribución de estados, comportamiento mensual, alertas automáticas, proyectos con mayor avance y exportación del resumen.

- Cálculos: src/assets/js/domain/dashboard.js
- Interfaz: index.html
- Comportamiento: src/assets/js/app.js
- Estilos: src/assets/css/styles.css
- Pruebas: tests/dashboard-indicators.test.mjs
