# JOINS
## INNER JOIN

```sql
select s.titulo as titulo_serie , e.titulo as titulo_episodio , e.duracion 
from series as s
inner join 
episodios as e on e.serie_id = s.serie_id
where s.titulo = 'Stranger Things'
```

## LEFT JOIN

El **`LEFT JOIN`** es un tipo de **unión** en SQL que combina las filas de dos o más tablas, **retorna todas las filas de la tabla 1** , e incluye las filas coincidentes de la **tabla 2.** Si no hay coincidencias aparecerán como **`NULL`**

```sql
SELECT Series.titulo as 'Título de la Serie', Episodios.titulo as 'Título del Episodio', Episodios.rating_imdb as 'Rating IMDB'
from Series
left join episodios
on Episodios.serie_id = Series.serie_id
order by Series.titulo, Episodios.titulo asc
```

## RIGHT JOIN

Lo mismo que el LEFT pero al revés, tomando la tabla 2 como principal.

```sql
select series.titulo as 'Título de la Serie',
episodios.titulo as 'Título del Episodio',
episodios.duracion as 'Duración'
from episodios 
right join series on series.serie_id = episodios.serie_id
where episodios.duracion >30
order by series.titulo asc
```

## UNION ALL

**Combina** el conjunto de **resultados** de 2 o más **SELECT, deben tener el mismo numero de columnas los select (PERMITE DUPLICADOS)**

```sql
select * from Series 
where genero = 'Ciencia ficción'

UNION ALL

select * from Series 
where genero = 'Drama'
```

## UNION

**Combina** el conjunto de **resultados** de 2 o más **SELECT, deben tener el mismo numero de columnas los select (UNE SOLO LOS VALORES DISTINTOS)**

```sql
select episodios.titulo from episodios
where duracion>20

UNION

select episodios.titulo from episodios
where rating_imdb>9
```
