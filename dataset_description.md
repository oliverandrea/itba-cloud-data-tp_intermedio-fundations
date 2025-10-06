# Dataset Elegido: NYC Air Quality Surveillance Data (Air Quality and Health Impacts)

## Fuente
**URL del Dataset:** https://catalog.data.gov/dataset/air-quality
**Fuente de Datos:** NYC Department of Health and Mental Hygiene (DOHMH)

## Descripción Breve
Este dataset contiene **18.9K registros** de mediciones de calidad del aire en la Ciudad de Nueva York, actualizado anualmente. Cada fila representa un **promedio de vecindario** (Neighborhood average) para un indicador (contaminante) específico durante un período de tiempo definido. Contiene 12 columnas, incluyendo el `Indicator ID`, `Name` (del contaminante), `Geo Place Name` (nombre del vecindario/CD), la fecha de inicio (`Start_Date`), y el `Data Value` (el valor de la medición). El objetivo es caracterizar la calidad del aire y la salud en NYC a lo largo del tiempo y la geografía.

## Estructura de Datos (Tablas Propuestas)
La normalización se basará en las columnas del CSV para evitar redundancia y mejorar la integridad de los datos.

1.  **`contaminantes`**:
    * **PK:** `indicator_id` (basado en la columna `Indicator ID`).
    * **Contiene:** Nombre (`Name`) y unidad de medida (`Measure Info`).
2.  **`lugares_geo`**:
    * **PK:** Se recomienda usar la columna `geo_join_id` o crear una clave artificial (SERIAL). Usaremos `geo_join_id` si es un identificador único de área.
    * **Contiene:** Tipo de geografía (`Geo Type Name`), Nombre del lugar (`Geo Place Name`).
3.  **`monitoreo_data`**:
    * **PK:** `unique_id` (si es el identificador único de registro) o `medicion_id` (BIGSERIAL).
    * **Contiene:** Fecha de inicio (`Start_Date`), Valor del dato (`Data Value`), Período de tiempo (`Time Period`), y las claves foráneas a `contaminantes` y `lugares_geo`.

## Preguntas de Negocio (Mínimo 4)

1.  **Comparación Inter-vecindario:** ¿Cuáles son los **5 vecindarios** (`Geo Place Name`) con la mayor concentración promedio del contaminante **Nitrogen dioxide (NO2)** en el último período registrado (comparando `Data Value` promedio)?
2.  **Tendencia a Largo Plazo:** Mostrar la tendencia del contaminante **PM2.5** (Material Particulado) a lo largo del tiempo, agrupando los datos por **año** (extrayendo del `Start_Date`). ¿Ha mejorado o empeorado su valor promedio anualmente?
3.  **Análisis de Medida:** ¿Cuál es la distribución de los **tipos de medida** (`Measure`) que tienen un `Data Value` **mayor a un umbral X** (ej. 15), y en qué proporción se relacionan con diferentes períodos de tiempo (`Time Period`)?
4.  **Identificación de Picos Atípicos:** Listar los 10 registros (Unique ID, Geo Place Name, Name) con los valores de datos más altos (`Data Value máximo`) de cualquier contaminante, para detectar picos de contaminación históricos.