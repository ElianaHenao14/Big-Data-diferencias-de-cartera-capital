# 05. Modelo de Ciencia de Datos

## Enfoque del Modelo
El proyecto utiliza un modelo descriptivo basado en reglas de negocio para analizar los resultados de la conciliación de capital de cartera.

El objetivo no es predecir comportamientos futuros, sino identificar y cuantificar las diferencias entre el capital esperado y el capital reportado para cada NIT.

## Regla de Análisis

> **Capital esperado = Capital del día anterior + Desembolsos - Pagos de capital**
>
> **Diferencia de capital = Capital esperado - Capital reportado**

Los registros se clasifican así:

* **Conciliado:** la diferencia se encuentra dentro de la tolerancia definida.
* **Diferencia positiva:** el capital esperado es mayor que el capital reportado.
* **Diferencia negativa:** el capital esperado es menor que el capital reportado.
* **Excepción de cobertura:** el NIT aparece únicamente en una de las fuentes.

## Estadística Descriptiva
Para evaluar el resultado del proceso se calculan los siguientes indicadores:

* **Total de NIT procesados:** cantidad de NIT incluidos en la conciliación.
* **NIT conciliados:** registros que no presentan diferencias superiores a la tolerancia.
* **NIT con diferencias:** registros que requieren revisión.
* **Porcentaje de conciliación:** proporción de NIT conciliados frente al total procesado.
* **Valor absoluto total de las diferencias:** magnitud total de las inconsistencias detectadas.
* **Diferencia promedio:** valor promedio de las diferencias encontradas.

## Fórmulas

> Porcentaje de conciliación = (NIT conciliados / Total de NIT procesados) × 100
>
> Valor absoluto total = Suma de |Diferencia de capital|

## Resultados

| Indicador | Resultado |
| --- | --- |
| Total de NIT procesados | Pendiente |
| NIT conciliados | Pendiente |
| NIT con diferencias | Pendiente |
| Porcentaje de conciliación | Pendiente |
| Valor absoluto total de diferencias | Pendiente |
| Diferencia promedio | Pendiente |

## Interpretación
Los indicadores permiten medir el nivel de conciliación, cuantificar la magnitud de las diferencias y priorizar los registros que deben ser revisados por el área responsable.
