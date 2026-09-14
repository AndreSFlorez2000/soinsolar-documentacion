# Actividad técnica complementaria 21 — Gestión contractual

**Objetivo:** cerrar el vacío entre la creación del proyecto y sus movimientos financieros.

## Funcionalidad implementada

Se incorporó la creación y edición de un contrato único asociado con cada proyecto. El formulario registra número de contrato, fecha inicial, fecha final, valor inicial, adiciones, deducciones y estado.

El sistema calcula automáticamente:

**Valor contractual vigente = valor inicial + adiciones − deducciones**

El contrato vigente se utiliza como denominador del avance financiero y como límite de la facturación acumulada.

## Reglas aplicadas

- Un proyecto debe existir antes de registrar su contrato.
- El número de contrato es obligatorio.
- Los valores monetarios no pueden ser negativos.
- La fecha final no puede ser anterior a la inicial.
- El valor vigente debe ser mayor que cero.
- Una modificación contractual no puede dejar el valor vigente por debajo de lo ya facturado.
- El contrato conserva relación con proyecto, facturas, pagos y seguimiento mensual.
- La operación queda registrada en el historial.

## Resultado

Un proyecto nuevo ya no queda aislado: puede recibir su contrato desde la interfaz y continuar de forma controlada hacia facturación, pagos, costos y seguimiento.
