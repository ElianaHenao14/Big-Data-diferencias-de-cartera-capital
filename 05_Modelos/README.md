# 05. Modelo de Ciencia de Datos

## Enfoque Metodológico
El proyecto implementa un modelo descriptivo y de detección de anomalías basado en reglas de negocio.

No se desarrolla un modelo predictivo, debido a que el objetivo del proyecto no consiste en predecir el comportamiento futuro de la cartera, sino en determinar si el capital reportado para cada NIT coincide con el capital esperado calculado a partir de los movimientos del periodo.

La salida de la capa Gold se utiliza para calcular indicadores de conciliación, analizar la magnitud de las diferencias y priorizar las excepciones que requieren revisión.

## Unidad de Análisis
La unidad de análisis es cada NIT consolidado dentro del proceso de conciliación.

## Variables Analizadas

| Variable | Tipo | Descripción |
| --- | --- | --- |
| NIT anonimizado | Categórica | Identificador utilizado para integrar las fuentes |
| Capital del día anterior | Numérica | Saldo base de capital |
| Desembolsos | Numérica | Incrementos de capital del día |
| Pagos de capital | Numérica | Disminuciones por recaudos |
| Capital esperado | Numérica | Resultado calculado por el workflow |
| Capital reportado | Numérica | Capital registrado en el plano del día |
| Diferencia de capital | Numérica | Capital esperado menos capital reportado |
| Estado | Categórica | Resultado de la conciliación |

## Indicadores Principales

### KPI 1. Total de NIT procesados
Número de NIT distintos considerados en la unión de las fuentes.

### KPI 2. NIT conciliados
Cantidad de NIT cuya diferencia absoluta se encuentra dentro de la tolerancia definida.

### KPI 3. NIT con diferencias
Cantidad de NIT cuya diferencia absoluta supera la tolerancia.

### KPI 4. Porcentaje de conciliación
> Porcentaje de conciliación = (NIT conciliados / Total de NIT procesados) × 100

### KPI 5. Valor absoluto total de diferencias
> Valor absoluto total = Suma de |Diferencia de capital|

Este indicador mide la magnitud total de las excepciones, sin permitir que las diferencias positivas y negativas se compensen entre sí.

### KPI 6. Diferencia neta
> Diferencia neta = Suma de las diferencias de capital

Permite identificar el efecto agregado entre capital esperado y reportado.

### KPI 7. Diferencia promedio
> Diferencia promedio = Suma de las diferencias / Número de NIT con diferencia

Este indicador debe analizarse junto con la mediana, porque los valores extremos pueden afectar el promedio.

## Estadística Descriptiva
Sobre la variable diferencia de capital se calcularán:

* Media.
* Mediana.
* Valor mínimo.
* Valor máximo.
* Desviación estándar.
* Percentil 75.
* Percentil 90.
* Percentil 95.

La mediana representa la diferencia típica, mientras que los percentiles permiten reconocer los registros con mayor magnitud y priorizar su revisión.

## Clasificación de Anomalías

### Anomalía de Cobertura
Se presenta cuando un NIT aparece únicamente en una de las fuentes involucradas en el cruce.

### Anomalía de Calidad
Se presenta cuando existe:

* NIT vacío o inválido.
* Valor monetario no numérico.
* Registro duplicado inesperado.
* Campo requerido inexistente.
* Incompatibilidad de tipos.

### Anomalía de Conciliación
Se presenta cuando la diferencia absoluta supera la tolerancia definida.

### Anomalía Severa
Se presenta cuando la diferencia absoluta supera:

* El percentil 95 de las diferencias; o
* El umbral monetario autorizado por el área responsable.

## Regla de Clasificación

| Estado | Criterio |
| --- | --- |
| Conciliado | Diferencia absoluta menor o igual a la tolerancia |
| Diferencia positiva | Capital esperado mayor que capital reportado |
| Diferencia negativa | Capital esperado menor que capital reportado |
| Excepción de cobertura | NIT presente en una sola fuente |
| Anomalía severa | Diferencia superior al umbral crítico |

## Resultados
Esta sección debe completarse después de ejecutar el workflow:

| Indicador | Resultado |
| --- | --- |
| Total de NIT procesados | Pendiente |
| NIT conciliados | Pendiente |
| NIT con diferencias | Pendiente |
| Porcentaje de conciliación | Pendiente |
| Valor absoluto total de diferencias | Pendiente |
| Diferencia neta | Pendiente |
| Diferencia promedio | Pendiente |
| Diferencia máxima | Pendiente |
| Percentil 95 | Pendiente |

## Interpretación Esperada
El análisis permitirá determinar:

* Qué proporción de los NIT queda conciliada.
* Cuál es el valor total de las diferencias.
* Si predominan las diferencias positivas o negativas.
* Cuáles registros deben revisarse primero.
* Qué inconsistencias están relacionadas con cobertura o calidad de datos.
* Cuál es la magnitud típica de las diferencias encontradas.

## Limitaciones
* El análisis corresponde a los archivos cargados para una fecha de proceso.
* No se realizan predicciones debido a la ausencia de una serie histórica dentro del alcance.
* La calidad de los resultados depende de la estructura y confiabilidad de los archivos fuente.
* Las diferencias detectadas deben ser investigadas antes de realizar ajustes en los sistemas de origen.

