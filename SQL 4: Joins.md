# 🔗 JOINS en SQL 
Guía de los tipos de **JOIN** y cómo utilizarlos con ejemplos prácticos.  
---
## 🟢 INNER JOIN
Devuelve **solo las filas coincidentes** entre ambas tablas.  
```sql
select s.titulo as titulo_serie , e.titulo as titulo_episodio , e.duracion 
from series as s
inner join 
episodios as e on e.serie_id = s.serie_id
where s.titulo = 'Stranger Things'
```
INNER JOIN → muestra solo las coincidencias entre series y episodios.
En este caso, obtenemos todos los episodios de Stranger Things.

## 🟡 LEFT JOIN

Devuelve todas las filas de la tabla izquierda (Series), y solo las coincidentes de la tabla derecha (Episodios).
Si no hay coincidencias, aparecerán como NULL.

```sql
SELECT Series.titulo as 'Título de la Serie', Episodios.titulo as 'Título del Episodio', Episodios.rating_imdb as 'Rating IMDB'
from Series
left join episodios
on Episodios.serie_id = Series.serie_id
order by Series.titulo, Episodios.titulo asc
```
Se devuelven todas las series, aunque no tengan episodios cargados.

## 🔵 RIGHT JOIN

Lo mismo que el LEFT JOIN, pero al revés:
Devuelve todas las filas de la tabla derecha (Episodios) y las coincidentes de la izquierda (Series).

```sql
select series.titulo as 'Título de la Serie',
episodios.titulo as 'Título del Episodio',
episodios.duracion as 'Duración'
from episodios 
right join series on series.serie_id = episodios.serie_id
where episodios.duracion >30
order by series.titulo asc
```
Se listan todas las series y los episodios que duren más de 30 min.
Si una serie no tiene episodios con esa duración, igual aparecerá, con valores NULL en episodios.

## 🟣 UNION ALL

**Combina** el conjunto de **resultados** de 2 o más **SELECT, deben tener el mismo numero de columnas los select (PERMITE DUPLICADOS)**

```sql
select * from Series 
where genero = 'Ciencia ficción'

UNION ALL

select * from Series 
where genero = 'Drama'
```
Une todas las filas encontradas en ambas consultas.
Si una serie es Ciencia ficción y Drama, aparecerá dos veces.

## 🔴 UNION

**Combina** el conjunto de **resultados** de 2 o más **SELECT, deben tener el mismo numero de columnas los select (UNE SOLO LOS VALORES DISTINTOS, NO PERMITE DUPLICADOS)**

```sql
select episodios.titulo from episodios
where duracion>20

UNION

select episodios.titulo from episodios
where rating_imdb>9
```
Muestra los títulos de episodios con duración > 20 o con rating > 9.
Si cumplen ambas condiciones, solo aparecerán una vez.
