# Hipotesis_Streaming

Este proyecto de análisis de datos busca comprobar o descartar cinco hipótesis sobre los factores que podrían influir en el éxito de una canción en plataformas de streaming como Spotify. 

## Contenido

**Tabla de contenido**

- [Introducción](#introducción)
- [Hipótesis](#hipótesis)
- [Herramientas](#herramientas)
- [Proceso](#proceso)
- [Características de la muestra](#características de la muestra)


## Introducción 

Las plataformas de streaming marcaron un antes y un después en la industria musical, estos servicios permiten a los artistas llegar a una audiencia global de manera instantánea, facilitan la difusión y popularidad de sus temas. Pero, ¿cómo lo logran? En este proyecto se busca explorar el comportamiento de la industria en los últimos años para encontrar patrones que puedan influir en la popularidad de las canciones.

## Hipótesis

Hipótesis planteadas sobre qué hace que una canción sea más escuchada:
1. Las canciones con un mayor BPM (Beats Por Minuto) tienen más éxito en términos de cantidad de streams en Spotify.
2. Las canciones más populares en el ranking de Spotify también tienen un comportamiento similar en otras plataformas como Deezer.
3. La presencia de una canción en un mayor número de playlists se relaciona con un mayor número de streams.
4. Los artistas con un mayor número de canciones en Spotify tienen más streams.
5. Las características de la canción influyen en el éxito en términos de cantidad de streams en Spotify.

## Herramientas

SQL (Google BigQuery)
Python
Power BI

## Proceso

- Importación de dataset a BigQuery.
- Identificación de NULOS con WHERE, COUNT, IS NULL.
- Identificación de duplicados. 
- Eliminación de variables fuera del alcance por medio de EXCEPT.
- Eliminación de datos discrepantes en variables categoricas.
- Modificación de formato de datos (string a interger) para mejor procesamiento por medio de CAST.
- Creación de nuevas variables: fecha de lanzamiento y cantidad de canciones por artista.
- Unión de tablas con LEFT JOIN.
- Creación de tablas auxiliares con WITH.

## Características de la muestra

<img width="709" alt="image" src="https://github.com/user-attachments/assets/741bddff-6e7b-46ea-9830-2e3c7b03b9f7">




  
