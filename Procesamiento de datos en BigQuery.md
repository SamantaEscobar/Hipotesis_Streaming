# PROCESAMIENTO DE DATOS EN BIG QUERY

## NULLS

Se identificaron 50 NULLS en la tabla competition, los cuales se mantuvieron en la base de datos:

```sql

SELECT
  COUNT (*)
FROM
  `proyecto-laboratoria-hipotesis.dataset_hipotesis.competition`
WHERE
  in_shazam_charts IS NULL

```

Identificación de NULLS en tabla TECHNICAL_INFO, los cuales se mantuveron en la base de datos:

```sql

SELECT 
*
FROM `proyecto-laboratoria-hipotesis.dataset_hipotesis. technical_info`
WHERE
  `key` IS NULL

```

## DUPLICADOS

Identificación de datos duplicados en tabla SPOTIFY:

```sql
SELECT
  track_name,
  artist_s__name,
  COUNT(*) AS cantidad
FROM
  `proyecto-laboratoria-hipotesis.dataset_hipotesis.spotify`
GROUP BY
  track_name,
  artist_s__name
HAVING
  COUNT(*) > 1;

```
## DATOS FUERA DEL ALCANCE

Esta QUERY elimina las columnas KEY y MODE de la tabla TECHNICAL INFO:

```sql

SELECT 
* EXCEPT (`key`, mode)
FROM `proyecto-laboratoria-hipotesis.dataset_hipotesis. technical_info` 

```

## DATOS DISCREPANTES 

### Variables categóricas 

Se identificó un dato discrepante en la columna STREAMS de la tabla SPOTIFY: 

```sql

SELECT * FROM `proyecto-laboratoria-hipotesis.dataset_hipotesis.spotify` 
WHERE streams = "BPM110KeyAModeMajorDanceability53Valence75Energy69Acousticness7Instrumentalness0Liveness17Speechiness3"

```

### Variables numéricas

STREAMS 

```sql

SELECT
  MAX(streams_int) AS max_streams,
  MIN(streams_int) AS min_streams,
  AVG(streams_int) AS avg_streams
FROM (
  SELECT 
    CAST(streams AS INT64) AS streams_INT
  FROM 
    `proyecto-laboratoria-hipotesis.dataset_hipotesis.spotify`
  WHERE 
    REGEXP_CONTAINS(streams, r'^[0-9]+$')

```

## CAMBIAR TIPO DE DATO

Esta QUERY convierte la variable STREAMS de un dato STRING a INTERGER; y elimina el valor discrepante: 

```sql

SELECT 
  *,
  CAST(streams AS INT64) AS streams_INT
FROM 
  `proyecto-laboratoria-hipotesis.dataset_hipotesis.spotify`
WHERE 
  REGEXP_CONTAINS(streams, r'^[0-9]+$');

```

## NUEVAS VARIABLES

Creación de nueva variable de fecha de lanzamiento de las canciones:

```sql

SELECT 
track_id, track_name, artist_s__name,
  PARSE_DATE('%Y-%m-%d', CONCAT(
    CAST(released_year AS STRING), '-', 
    LPAD(CAST(released_month AS STRING), 2, '0'), '-', 
    LPAD(CAST(released_day AS STRING), 2, '0')
  )) AS release_date
FROM 
  `proyecto-laboratoria-hipotesis.dataset_hipotesis.spotify`

```

## UNIÓN DE TABLAS

VIEW de
- Unión de tablas: SPOTIFY, TECHNICAL INFO y COMPETITION a través de LEFT JOIN,
- Creación de nueva variable de PARTICIPACIÓN EN PLAYLIST a través de SUMA,
- Creación de tabla auxiliar de TOTAL DE CANCIONES por artista a través de WITH:

```sql

WITH total_canciones_por_artista AS (
  SELECT 
    COUNT(*) AS total_canciones, 
    artist_s_name
  FROM
    `proyecto-laboratoria-hipotesis.dataset_hipotesis.spotify_cleaned` 
  WHERE 
    artist_count = 1  -- Filtrar solo canciones con un solo artista
  GROUP BY 
    artist_s_name
)

SELECT 
  A.track_id,
  A.track_name,
  A.artist_s_name,
  A.release_date,
  A.streams,
  A.artist_count,
  A.released_year,
  A.released_month,
  A.released_day,
  A.in_spotify_playlists,
  A.in_spotify_charts,
  B.bpm,
  B.`danceability_%`,
  B.`valence_%`,
  B.`energy_%`,
  B.`acousticness_%`,
  B.`instrumentalness_%`,
  B.`liveness_%`,
  B.`speechiness_%`,
  C.in_apple_playlists,
  C.in_apple_charts,
  C.in_deezer_playlists,
  C.in_deezer_charts,
  C.in_shazam_charts,
  t.total_canciones,
  A.in_spotify_playlists + C.in_apple_playlists + C.in_deezer_playlists AS total_playlists  -- Nueva columna SUMA DE IN PLAYLISTS
FROM 
  `proyecto-laboratoria-hipotesis.dataset_hipotesis.spotify_cleaned` AS A
LEFT JOIN (
  SELECT 
    track_id,
    bpm,
    `danceability_%`,
    `valence_%`,
    `energy_%`,
    `acousticness_%`,
    `instrumentalness_%`,
    `liveness_%`,
    `speechiness_%`
  FROM `proyecto-laboratoria-hipotesis.dataset_hipotesis.view_info_tecnica_limpia`
) AS B
ON A.track_id = B.track_id
LEFT JOIN (
  SELECT 
    track_id,
    in_apple_playlists,
    in_apple_charts,
    in_deezer_playlists,
    in_deezer_charts,
    in_shazam_charts
  FROM `proyecto-laboratoria-hipotesis.dataset_hipotesis.competition`
) AS C
ON A.track_id = C.track_id
LEFT JOIN
  total_canciones_por_artista AS t
ON A.artist_s_name = t.artist_s_name;

```



