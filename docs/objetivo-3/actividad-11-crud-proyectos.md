# Objetivo 3 — Actividad 11

## Desarrollar las operaciones de registro, consulta, actualización y eliminación de proyectos

**Producto:** CRUD de proyectos  
**Formato:** enlace al repositorio  
**Periodo previsto en el PA:** 15 al 21 de octubre de 2026  
**Versión implementada:** 0.3.1

## 1. Objetivo

Implementar un módulo que centralice la información básica de los proyectos y permita crear, consultar, editar y eliminar registros mediante controles consistentes con el modelo de datos y los permisos definidos para SOINSOLAR.

## 2. Alcance funcional

El módulo permite:

- Consultar todos los proyectos disponibles.
- Buscar por nombre del proyecto o cliente.
- Filtrar por centro de costo, municipio y estado.
- Crear un proyecto desde un formulario validado.
- Editar la información de un proyecto existente.
- Abrir el detalle y consultar su resumen financiero.
- Eliminar un proyecto únicamente cuando no tiene movimientos relacionados.
- Actualizar el dashboard después de una creación, modificación o eliminación.
- Mantener los datos de la demostración en el almacenamiento local del navegador.
- Ejecutar las mismas acciones sobre Supabase cuando el entorno real se encuentre configurado.

## 3. Información administrada

| Campo | Uso | Validación principal |
|---|---|---|
| Centro de costo | Identificador empresarial del proyecto | Obligatorio, entre 2 y 50 caracteres y único |
| Nombre | Identificación descriptiva | Obligatorio, entre 3 y 180 caracteres |
| Cliente | Cliente asociado | Opcional |
| Municipio | Ubicación del proyecto | Obligatorio |
| Tipo de servicio | Clasificación T.S. | Obligatorio |
| Potencia | Capacidad expresada en kWp | Numérica y no negativa |
| Estado | Situación del proyecto | Planeado, activo, suspendido, finalizado o cancelado |
| Fecha de inicio | Inicio previsto o real | Fecha válida |
| Fecha de finalización | Terminación prevista o real | Igual o posterior al inicio |
| Observaciones | Información complementaria | Opcional |

Los valores contractuales, facturados, pagados y ejecutados no se escriben directamente en el formulario del proyecto. Se calculan a partir de contratos y movimientos relacionados para evitar inconsistencias.

## 4. Flujo de operación

1. El usuario ingresa al módulo Proyectos.
2. La aplicación consulta la vista financiera de proyectos.
3. El usuario puede aplicar filtros o seleccionar un registro.
4. Para crear o editar, completa el formulario del proyecto.
5. La capa de modelos normaliza los datos y valida fechas, textos, estados y potencia.
6. El repositorio ejecuta la operación correspondiente.
7. Supabase aplica autenticación, permisos RLS y restricciones de base de datos en el entorno conectado.
8. La interfaz actualiza el listado, el dashboard y el detalle financiero.

## 5. Reglas implementadas

- El centro de costo identifica de manera única al proyecto.
- No se aceptan campos obligatorios vacíos.
- La fecha final no puede ser anterior a la fecha inicial.
- La potencia no puede ser negativa.
- Solo se aceptan estados definidos en el modelo.
- La aplicación solicita confirmación antes de eliminar.
- La eliminación se rechaza cuando existen facturas, pagos, costos, gastos u otras relaciones.
- Los proyectos con historia deben cambiar de estado en lugar de eliminarse.
- Los datos insertados desde la interfaz también son comprobados por PostgreSQL y RLS.

## 6. Componentes desarrollados

| Componente | Responsabilidad |
|---|---|
| `index.html` | Vista de proyectos, filtros, tabla y formulario |
| `styles.css` | Diseño adaptable del módulo y cuadro de edición |
| `app.js` | Eventos, mensajes, actualización de vista y detalle |
| `models.js` | Normalización y validación de los campos |
| `demo-store.js` | CRUD demostrativo con persistencia local |
| `repositories.js` | Operaciones reales sobre la tabla `projects` y la vista financiera |
| `application-data.js` | Selección transparente entre demostración y Supabase |

## 7. Casos verificados

| Caso | Resultado esperado | Resultado |
|---|---|---|
| Crear un proyecto válido | El registro aparece en la consulta | Aprobado |
| Consultar por centro de costo | Solo aparecen coincidencias | Aprobado |
| Editar el nombre de un proyecto | Se conserva el identificador y cambia el nombre | Aprobado |
| Eliminar un proyecto sin movimientos | El registro deja de aparecer | Aprobado |
| Repetir un centro de costo | La operación es rechazada | Aprobado |
| Finalizar antes de iniciar | La operación es rechazada | Aprobado |
| Eliminar un proyecto con movimientos | La operación es rechazada | Aprobado |

## 8. Seguridad

- La demostración no utiliza información empresarial real.
- En Supabase, toda consulta requiere un usuario autenticado y activo.
- Administrador y Gerencia pueden consultar y registrar según las políticas vigentes.
- La eliminación queda limitada al rol administrador.
- El navegador utiliza únicamente la URL y la llave publicable.
- Las credenciales secretas no forman parte del código del frontend.

## 9. Evidencia y resultado

La Actividad 11 queda implementada como un CRUD funcional en modo demostrativo y preparada para operar contra Supabase mediante la misma interfaz. Las pruebas automáticas verifican creación, consulta, actualización, eliminación, duplicidad y protección del histórico.

- [Diagramas del sistema](./diagramas-sistema.md)
- Código principal: `src/assets/js/app.js`
- Acceso a datos: `src/assets/js/services/application-data.js`
- Pruebas: `tests/project-cost-modules.test.mjs`
