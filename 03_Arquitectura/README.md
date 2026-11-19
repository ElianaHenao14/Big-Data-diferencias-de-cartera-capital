# 3. Arquitectura Propuesta 

## Arquitectura Medallion

# Capa Bronze - Datos Originales

La capa Bronze representa la ingesta de los archivos originales, sin modificar su contenido de origen. En el workflow esta capa corresponde a los cuatro nodos **Excel Reader**.

## Fuentes

| Fuente | Campos mínimos | Función |
| --- | --- | --- |
| Plano del día anterior | NIT, capital | Proporcionar el saldo base de la conciliación |
| Desembolsos | NIT, valor desembolsado | Identificar los incrementos de capital |
| Recaudos | NIT, concepto, efectivo, cheque | Identificar los pagos aplicados a capital |
| Plano del día de la diferencia | NIT, capital | Proporcionar el capital reportado para la comparación |

Los archivos originales se conservan sin modificaciones para garantizar la trazabilidad del proceso.

# Capa Silver - Datos Depurados y Consolidados

La capa Silver reúne las transformaciones necesarias para preparar, estandarizar e integrar la información.

## Procesos Realizados

* Selección de columnas mediante **Column Filter**.
* Conversión del NIT a texto mediante **Number to String**.
* Filtrado de los recaudos correspondientes a capital mediante **Row Filter**.
* Cálculo del pago total como efectivo más cheque mediante **Math Formula**.
* Agrupación y suma de valores por NIT mediante **GroupBy**.
* Integración de las diferentes fuentes mediante **Joiner**.
* Tratamiento de valores ausentes mediante **Missing Value**.
* Cálculo del capital esperado.

## Regla de Negocio

> **Capital esperado = Capital del día anterior + Desembolsos - Pagos de capital**

## Resultado de la Capa Silver

Una tabla consolidada por NIT que contiene:

* Capital del día anterior.
* Desembolsos.
* Pagos de capital.
* Capital esperado.

* Capa Gold - Información Lista para el Negocio
La capa Gold contiene los resultados finales de la conciliación y la información preparada para análisis y visualización.

Procesos realizados
Cruce del capital esperado con el capital reportado.
Tratamiento controlado de valores ausentes.
Cálculo de la diferencia de capital.
Clasificación del resultado.
Filtrado de los registros con diferencias.
Selección de las columnas finales.
Exportación del reporte a Excel.
Regla de comparación
Diferencia de capital = Capital esperado - Capital reportado

Variables finales
NIT anonimizado.
Capital esperado.
Capital reportado.
Diferencia de capital.
Estado de conciliación.
Fecha del proceso.
Clasificación
Conciliado: diferencia dentro de la tolerancia aprobada.
Diferencia positiva: capital esperado mayor que el reportado.
Diferencia negativa: capital esperado menor que el reportado.
Excepción de cobertura: NIT presente únicamente en una de las fuentes.
