# 04. Pipeline de Ingesta y Transformación de Datos

## Descripción General
El pipeline desarrollado en KNIME automatiza la ingesta, preparación, integración y conciliación de cuatro fuentes de información utilizadas para identificar diferencias de capital de cartera por NIT.

El workflow procesa el plano del día anterior, los desembolsos, los recaudos y el plano del día de la diferencia. A partir de estas fuentes calcula el capital esperado, lo compara con el capital reportado y genera un archivo final con las diferencias identificadas.

## Archivo del Workflow
`DIFERENCIAS DE CAPITAL BIG DATA.knwf`

## Herramienta Utilizada
* KNIME Analytics Platform.
* Microsoft Excel como formato de entrada y salida.
* GitHub para documentación y control de versiones.

## Fuentes de Información

| Fuente | Propósito | Campos mínimos |
| --- | --- | --- |
| Plano del día anterior | Obtener el saldo base de capital | NIT, capital |
| Desembolsos | Identificar los incrementos de capital del día | NIT, valor desembolsado |
| Recaudos | Identificar los pagos aplicados a capital | NIT, concepto, efectivo, cheque |
| Plano del día de la diferencia | Obtener el capital reportado para la comparación | NIT, capital |

## Secuencia del Pipeline

### 1. Lectura y consolidación del plano anterior
* **Excel Reader:** carga el plano del día anterior.
* **Column Filter:** conserva únicamente los campos necesarios.
* **GroupBy:** agrupa por NIT y suma el capital.
* **Salida:** capital del día anterior consolidado por NIT.

### 2. Lectura y consolidación de desembolsos
* **Excel Reader:** carga el archivo de desembolsos.
* **Column Filter:** selecciona el NIT y el valor desembolsado.
* **Number to String:** convierte el NIT a texto para homologar la llave.
* **GroupBy:** suma los desembolsos por NIT.
* **Salida:** desembolsos totales por NIT.

### 3. Integración del plano anterior y los desembolsos
* **Joiner:** relaciona ambas fuentes mediante el NIT.
* **Missing Value:** completa con cero los desembolsos ausentes.
* Si un NIT no presenta desembolsos durante el día, el valor correspondiente se interpreta como cero, sin modificar el capital del día anterior.

### 4. Lectura y preparación de recaudos
* **Excel Reader:** carga el archivo de recaudos.
* **Column Filter:** conserva los campos requeridos.
* **Row Filter:** selecciona únicamente los pagos asociados al componente capital.
* **Math Formula:** calcula el pago total.

**Fórmula:**
> Pago total de capital = Efectivo + Cheque

* **GroupBy:** suma los pagos de capital por NIT.
* **Salida:** pagos totales de capital por NIT.

### 5. Integración de pagos
* **Joiner:** relaciona el resultado anterior con los pagos de capital.
* **Missing Value:** completa con cero los pagos ausentes.
* La ausencia de un pago se interpreta como que el NIT no presentó recaudo de capital durante el periodo analizado.

### 6. Cálculo del capital esperado
El nodo Math Formula aplica la siguiente regla:

> **Capital esperado = Capital del día anterior + Desembolsos - Pagos de capital**

### 7. Lectura del plano del día de la diferencia
* **Excel Reader:** carga el plano del día analizado.
* **Column Filter:** conserva el NIT y el capital reportado.
* **GroupBy:** consolida el capital por NIT.
* **Salida:** capital reportado por NIT.

### 8. Cruce final
El nodo Joiner compara el capital esperado con el capital reportado utilizando el NIT como llave.

Los NIT que aparezcan únicamente en una de las fuentes deberán conservarse como excepciones de cobertura para su revisión.

### 9. Cálculo de la diferencia
El nodo Math Formula aplica:

> **Diferencia de capital = Capital esperado - Capital reportado**

### 10. Filtrado y exportación
* **Row Filter:** conserva solamente los registros cuya diferencia supera la tolerancia aprobada.
* **Column Filter:** selecciona y ordena las columnas finales.
* **Excel Writer:** genera el archivo final de diferencias.

## Salida Final
El archivo exportado debe contener, como mínimo:

| Campo | Descripción |
| --- | --- |
| NIT anonimizado | Identificador utilizado para la conciliación |
| Capital del día anterior | Capital base |
| Desembolsos | Valor desembolsado durante el día |
| Pagos de capital | Recaudos aplicados a capital |
| Capital esperado | Resultado de la regla de negocio |
| Capital reportado | Saldo registrado en el plano del día |
| Diferencia de capital | Capital esperado menos capital reportado |
| Estado | Conciliado, diferencia o excepción de cobertura |
| Fecha del proceso | Fecha correspondiente a la ejecución |

## Reglas de Negocio

| Código | Regla |
| --- | --- |
| RN-01 | El NIT se utiliza como llave de integración y debe manejarse como texto. |
| RN-02 | Los valores monetarios se consolidan por NIT antes de realizar los cruces. |
| RN-03 | El pago de capital se calcula como efectivo más cheque. |
| RN-04 | El capital esperado es igual al capital anterior más desembolsos menos pagos. |
| RN-05 | La diferencia es igual al capital esperado menos el capital reportado. |
| RN-06 | Un valor ausente se reemplaza por cero únicamente cuando represente ausencia de movimiento. |
| RN-07 | Los NIT presentes en una sola fuente se reportan como excepciones de cobertura. |
| RN-08 | Solo se exportan diferencias superiores a la tolerancia aprobada. |

## Controles de Calidad
Antes de aceptar la ejecución se deben validar los siguientes controles:

* Existencia de los cuatro archivos de entrada.
* Presencia de las columnas requeridas.
* Conversión correcta del NIT a texto.
* Identificación de NIT vacíos o inválidos.
* Revisión de registros duplicados.
* Validación de campos monetarios nulos o no numéricos.
* Comparación de totales antes y después de cada agrupación.
* Identificación de NIT no emparejados en los cruces.
* Validación manual de la fórmula con casos conocidos.
* Comprobación de que la salida contiene únicamente las excepciones definidas.

## Casos de Prueba

| Caso | Datos de prueba | Resultado esperado |
| --- | --- | --- |
| Sin movimientos | Anterior 100, desembolsos 0, pagos 0, reportado 100 | Diferencia 0 |
| Con desembolso | Anterior 100, desembolsos 50, pagos 0, reportado 150 | Diferencia 0 |
| Con pago | Anterior 100, desembolsos 0, pagos 30, reportado 70 | Diferencia 0 |
| Diferencia positiva | Esperado 120, reportado 100 | Diferencia 20 |
| Diferencia negativa | Esperado 80, reportado 100 | Diferencia -20 |
| NIT exclusivo | El NIT aparece en una sola fuente | Excepción de cobertura |

## Automatización y Trazabilidad
Para garantizar la reproducibilidad del workflow se recomienda:

* Utilizar rutas relativas o variables de flujo.
* Parametrizar la fecha del proceso.
* Registrar el nombre de los archivos procesados.
* Conservar el número de filas de entrada y salida.
* Mantener nombres funcionales y anotaciones en los nodos.
* Controlar las versiones del archivo .knwf en GitHub.
* Utilizar datos sintéticos o anonimizados en el repositorio.

## Cómo Abrir el Workflow
1. Abrir KNIME Analytics Platform.
2. Seleccionar File.
3. Elegir Import KNIME Workflow.
4. Seleccionar el archivo `DIFERENCIAS DE CAPITAL BIG DATA.knwf`.
5. Importar el flujo.
6. Configurar las rutas de los archivos de entrada.
7. Ejecutar el workflow.
