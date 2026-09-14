# Actividad técnica complementaria 22 — Facturación y pagos

**Objetivo:** implementar el ciclo financiero que transforma el valor contractual en facturación y recaudo mensual.

## Facturación

Cada factura registra proyecto, contrato, periodo, número, fecha de emisión, valor, estado y ruta opcional del soporte. Solo se permite registrar la factura cuando el proyecto tiene contrato y el periodo está abierto.

## Pagos

Cada pago se registra contra una factura concreta e incluye periodo, referencia, fecha, valor, estado y ruta opcional del soporte. El pago no modifica el contrato: disminuye únicamente el valor pendiente de recaudo de la factura y del proyecto.

## Reglas financieras

- Facturación acumulada ≤ valor contractual vigente.
- Pagos acumulados de una factura ≤ valor de la factura.
- La fecha de factura o pago debe corresponder al mes y año del periodo.
- El periodo debe estar abierto.
- El número de factura y la referencia de pago son únicos.
- Una factura anulada no aporta al total facturado.
- Un pago anulado no aporta al total pagado.
- Un proyecto con movimientos financieros no se elimina de forma destructiva.
- El cierre del periodo comprueba los proyectos con actividad del mes.

## Indicadores derivados

- **Avance financiero = facturado ÷ contrato vigente × 100**
- **Saldo contractual = contrato vigente − facturado**
- **Pago pendiente = facturado − pagado**
- **Recaudo = pagado ÷ facturado × 100**

## Resultado

La aplicación diferencia correctamente tres valores que no deben confundirse: lo contratado, lo facturado al cliente y lo efectivamente pagado por el cliente.
