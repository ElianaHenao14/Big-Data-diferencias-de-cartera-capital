# Proyecto final Big Data

## Automatizacion de la identificacion de diferencias de capital de cartera mediante KNIME

**Integrantes:** Eliana Henao Galeano y Sara Lizeth Suarez Mona  
**Docente:** Yeis Livis Taborda Henao  
**Periodo:** II-2026

## Descripcion
El proyecto academico de big data para la automatizacion y  la conciliacion diaria del capital de cartera. Integra el plano del dia anterior, desembolsos, recaudos y el plano del dia de la diferencia; calcula el capital esperado por NIT y exporta las excepciones encontradas.

## Regla principal
`Capital esperado = capital dia anterior + desembolsos - pagos de capital`

`Diferencia = capital esperado - capital reportado`

## Arquitectura logica
- **Bronze:** lectura de los cuatro archivos originales.
- **Silver:** filtros, tipificacion, agrupaciones, cruces, nulos y capital esperado.
- **Gold:** diferencia, clasificacion, KPI y archivo final.

## Requisitos
- KNIME Analytics Platform.
- Extension de lectura/escritura de Excel.
- Cuatro archivos de entrada con estructura documentada.

## Ejecucion
1. Descargar o clonar el repositorio.
2. Importar el archivo `.knwf` en KNIME.
3. Configurar rutas o flow variables para las cuatro fuentes.
4. Ejecutar el workflow de izquierda a derecha.
5. Revisar el archivo Gold y los indicadores.

## Confidencialidad
El repositorio solo debe contener datos sinteticos o anonimizados.
