# 04 - Pipeline de Datos

Este es el pipeline de Big Data para detectar diferencias de cartera de capital.

**Archivo del flujo:** `DIFERENCIAS DE CAPITAL BIG DATA.knwf`

**Que hace:**
1. Leer plano anterior y agrupar capital por NIT.
2. Leer desembolsos, tipar NIT y agrupar valor por NIT.
3. Cruzar y completar desembolsos ausentes con cero.
4. Leer recaudos, filtrar capital, calcular efectivo + cheque y agrupar por NIT.
5. Cruzar pagos y completar ausentes con cero.
6. Calcular capital esperado.
7. Leer y agrupar plano del dia.
8. Cruzar esperado contra reportado.
9. Calcular diferencia.
10. Filtrar excepciones y exportar.

**Pruebas obligatorias**
Validar esquema, tipos, duplicados, nulos, totales, cobertura de cruces, formulas, salida y reejecucion.

**Como abrirlo:** KNIME > File > Import KNIME Workflow
