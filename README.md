# Análisis de Datos de New York Citi Bike

Este repositorio contiene un análisis detallado de los datos de New York Citi Bike utilizando BigQuery y visualizaciones en Google Data Studio.

## Descripción del Proyecto

El objetivo principal de este proyecto es explorar y comprender el comportamiento de los usuarios de New York Citi Bike a través de análisis de datos. El conjunto de datos proporciona información valiosa sobre los viajes realizados, incluyendo detalles sobre la duración, ubicación, usuarios y más.

### Author
[Claudia Chirinos](https://github.com/cchirinosdata)

### Pre-requisitos
`Google Big Query` on `Google Cloud Platform`
`Google Looker Studio`

### Instrucciones
Database can be accessed from the Big Query public data set:
`bigquery-public-data.new_york.citibike_trips`

## Consultas en BigQuery

### Consulta 1: Número de Viajes Diarios

```
-- Número de viajes que se realizan en promedio cada día
SELECT
  DATE(starttime) as fecha,
  COUNT(*) as viajes_diarios
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`
WHERE
  starttime IS NOT NULL
GROUP BY
  fecha
ORDER BY
  fecha;
```

Esta consulta calcula el número de viajes que se realizan en promedio cada día, proporcionando una visión general de la frecuencia diaria de uso del servicio.

### Consulta 2: Crecimiento del Número de Viajes Diarios

```
-- Crecimiento del número de viajes diarios a lo largo del tiempo
WITH ViajesDiarios AS (
  SELECT
    DATE(starttime) as fecha,
    COUNT(*) as viajes_diarios
  FROM
    `bigquery-public-data.new_york_citibike.citibike_trips`
  WHERE
    starttime IS NOT NULL
  GROUP BY
    fecha
)

SELECT
  fecha,
  viajes_diarios,
  SUM(viajes_diarios) OVER (ORDER BY fecha) as total_viajes_acumulados
FROM
  ViajesDiarios
ORDER BY
  fecha;
```
Esta consulta muestra el crecimiento del número de viajes diarios a lo largo del tiempo, ayudando a identificar patrones y tendencias en la utilización del servicio.

### Consulta 3: Total de Viajes por Usuarios, Género y Edad
```
-- Total de viajes por usuarios, según género, edad y/o tipo de subscripción
SELECT
  usertype,
  gender,
  EXTRACT(YEAR FROM CURRENT_DATE()) - birth_year AS age,
  COUNT(*) as total_viajes
FROM
  `bigquery-public-data.new_york_citibike.citibike_trips`
WHERE
  starttime IS NOT NULL
  AND gender != 'unknown'
GROUP BY
  usertype, gender, age;
```

Esta consulta calcula el total de viajes categorizados por tipo de usuario, género y edad, proporcionando una comprensión detallada de la demografía de los usuarios.

Dashboard en Google Data Studio
Además de las consultas en BigQuery, se ha creado un dashboard interactivo en Google Data Studio para visualizar los resultados. El dashboard ofrece una representación visual de los análisis y puede ser explorado para obtener información más detallada.

Para acceder al dashboard en Google Data Studio, sigue el enlace proporcionado y explora las visualizaciones interactivas.

