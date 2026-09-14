# Guía paso a paso para la prueba de uso

**Sistema:** Control de costos, gastos y ejecución de proyectos  
**Versión:** 1.1.0  
**Propósito:** permitir que un usuario valide el ciclo completo con datos ficticios y resultados calculables.

## 1. Preparación

1. Abra la demostración pública.
2. Pulse **Entrar al prototipo**. La demostración no solicita credenciales reales.
3. Confirme que el encabezado indique modo demostrativo.
4. Si ya realizó pruebas anteriores, use la opción de restablecer datos del navegador cuando esté disponible o abra una ventana privada.
5. Trabaje con el periodo **Septiembre 2026**, que está abierto en los datos de prueba.
6. No use nombres, contratos, clientes, facturas ni soportes reales.

## 2. Datos del caso

| Campo | Valor de prueba |
|---|---|
| Centro de costo | PT-VAL-001 |
| Proyecto | Proyecto Validación Uno |
| Cliente | Cliente de prueba |
| Municipio | Ibagué |
| Tipo de servicio | Construcción EPC |
| Inicio | 01/09/2026 |
| Fin | 31/12/2026 |
| Contrato | CTR-VAL-001 |
| Valor inicial | $100.000.000 |
| Adiciones | $20.000.000 |
| Deducciones | $5.000.000 |
| Factura | FAC-VAL-001 por $40.000.000 |
| Pago | PAG-VAL-001 por $30.000.000 |
| Costo | Materiales por $20.000.000 |
| Gasto | Transporte por $5.000.000 |
| Valor reconocido mensual | $40.000.000 |

## 3. Prueba por módulo

### Paso 1. Inicio

Compruebe que aparecen indicadores generales, distribución por estado, alertas y comportamiento mensual. Los valores deben reaccionar a los registros creados posteriormente.

### Paso 2. Crear el proyecto

1. Abra **Proyectos**.
2. Pulse **Nuevo proyecto**.
3. Ingrese los datos del caso.
4. Guarde.
5. Busque **PT-VAL-001**.
6. Abra el detalle y confirme nombre, estado, municipio y tipo de servicio.

**Resultado esperado:** el proyecto aparece una sola vez y puede localizarse por nombre o centro de costo.

### Paso 3. Crear el contrato

1. Dentro del detalle, pulse **Gestionar contrato**.
2. Ingrese CTR-VAL-001, las fechas y los tres valores.
3. Guarde.
4. Verifique el valor contractual vigente.

**Cálculo esperado:** $100.000.000 + $20.000.000 − $5.000.000 = **$115.000.000**.

### Paso 4. Registrar la factura

1. Abra **Facturación y pagos**.
2. Pulse **Nueva factura**.
3. Seleccione el proyecto, su contrato y Septiembre 2026.
4. Registre FAC-VAL-001, fecha 10/09/2026 y $40.000.000.
5. Guarde.

**Resultados esperados:**

- Total facturado: $40.000.000.
- Avance financiero: 40.000.000 ÷ 115.000.000 × 100 = **34,78 %**.
- Saldo contractual: 115.000.000 − 40.000.000 = **$75.000.000**.

### Paso 5. Registrar el pago

1. Pulse **Nuevo pago**.
2. Seleccione FAC-VAL-001.
3. Registre PAG-VAL-001, fecha 20/09/2026 y $30.000.000.
4. Guarde.

**Resultados esperados:**

- Total pagado: $30.000.000.
- Pago pendiente: 40.000.000 − 30.000.000 = **$10.000.000**.
- Recaudo: 30.000.000 ÷ 40.000.000 × 100 = **75 %**.

### Paso 6. Registrar costo y gasto

1. Abra **Costos y gastos**.
2. Registre un costo de Materiales por $20.000.000, fecha 12/09/2026.
3. Registre un gasto de Transporte por $5.000.000, fecha 18/09/2026.
4. Filtre por PT-VAL-001 y Septiembre 2026.

**Resultados esperados:**

- Costos: $20.000.000.
- Gastos: $5.000.000.
- Total costos y gastos: **$25.000.000**.
- Tasa de costos: 25.000.000 ÷ 115.000.000 × 100 = **21,74 %**.
- Diferencia facturación-costos: 40.000.000 − 25.000.000 = **$15.000.000**.

### Paso 7. Seguimiento mensual

1. Abra **Seguimiento mensual**.
2. Seleccione Septiembre 2026 y PT-VAL-001.
3. Registre $40.000.000 como valor reconocido y una observación de prueba.
4. Guarde el registro.
5. Valídelo si el rol lo permite.

**Resultado esperado:** la aplicación presenta el mes, la facturación, el pago, los costos, el avance mensual y el acumulado sin volver a digitar los totales derivados.

### Paso 8. Reporte gerencial

1. Abra **Reportes**.
2. Filtre por PT-VAL-001.
3. Compare la fila del proyecto con los resultados anteriores.
4. Pulse **Exportar CSV** y abra el archivo.

**Resultado esperado:** pantalla y CSV muestran contrato $115.000.000, facturación $40.000.000, pago $30.000.000, costos/gastos $25.000.000, saldo $75.000.000, pago pendiente $10.000.000, avance 34,78 %, recaudo 75 % y diferencia $15.000.000.

### Paso 9. Historial

1. Abra **Historial**.
2. Filtre por PT-VAL-001 o por las tablas utilizadas.
3. Revise creación de proyecto, contrato, factura, pago, costo/gasto y seguimiento.

**Resultado esperado:** cada acción indica tabla, registro, fecha y valores antes/después cuando corresponda.

### Paso 10. Administración

1. Abra **Administración** con un usuario administrador.
2. Compruebe que aparecen perfiles existentes y sus roles.
3. Cambie únicamente un perfil de prueba.
4. Confirme que el sistema no solicita ni muestra contraseñas.

**Resultado esperado:** el perfil cambia de forma controlada; las cuentas nuevas continúan gestionándose en Supabase Auth.

## 4. Pruebas negativas obligatorias

| Intento | Resultado esperado |
|---|---|
| Crear otra factura con número FAC-VAL-001 | Rechazo por duplicado |
| Facturar más de $75.000.000 adicionales | Rechazo porque supera el contrato |
| Pagar más de $10.000.000 adicionales sobre FAC-VAL-001 | Rechazo porque supera el saldo de la factura |
| Registrar movimiento de agosto en el periodo de septiembre | Rechazo por fecha fuera del periodo |
| Registrar en un periodo cerrado | Rechazo por periodo cerrado |
| Crear otro proyecto con PT-VAL-001 | Rechazo por centro de costo duplicado |

## 5. Registro del evaluador

| Dato | Registro |
|---|---|
| Nombre del evaluador | |
| Rol | |
| Fecha | |
| Navegador y versión | |
| Dispositivo | |
| Resultado general | Aprobado / Aprobado con observaciones / No aprobado |
| Observaciones | |
| Firma o evidencia de aprobación | |
