# SQL 6: Consultas Nivel Avanzado
## Consultas recursivas (CTE)

**C**ommon **T**able **E**xpression: Es una consulta que existe temporalmente y para uso únicamente dentro del contexto de una consulta principal, es parecido a una subconsulta 

```sql
WITH 
ListaEpisodios AS (
Select serie_id, episodio_id, titulo FROM episodios
),

ListaSeries AS (
Select serie_id, descripcion FROM series
)

Select * FROM ListaEpisodios
LEFT JOIN ListaSeries 
on ListaSeries.serie_id = ListaEpisodios.serie_id
```

## ROW_NUMBER

Sirve para añadir una tabla que enumere los datos en base a una columna en específico, en este caso agrega una columna que enumera según el año lanzamiento desde el mas nuevo al mas antiguo

```sql
SELECT titulo, año_lanzamiento,
ROW_NUMBER() OVER (order by año_lanzamiento DESC) AS orden_lanzamiento
FROM series
```

```sql
with OrdenSeries as (
SELECT titulo, año_lanzamiento,
ROW_NUMBER() OVER (order by año_lanzamiento DESC) AS orden_lanzamiento
FROM series
)
select * from OrdenSeries
Where orden_lanzamiento in (1,2,3)
```

## PARTITION BY

Lo que hace es particionar en grupos mas pequeños el orden del row number, es decir puedes enumerar por año de lanzamiento y luego filtrar además según el genero

```sql
Select titulo,genero,año_lanzamiento,
row_number() OVER (partition by genero ORDER BY año_lanzamiento DESC) AS ranking_por_genero
from series
```

```sql
Select temporada,titulo,rating_imdb,
row_number() OVER (partition by temporada ORDER BY episodios.rating_imdb DESC) AS'Ranking IMDb'
from episodios
where serie_id = (select serie_id from series where titulo = 'Stranger Things')
```

## RANK

Se diferencia con el ROW_NUMBER en como maneja los empates si 2 o mas filas tienen el mismo valor, en **RANK** todas reciben el **mismo numero** si tienen el mismo valor. En **ROW_NUMBER** va sumando siempre un numero aunque el valor sea el mismo ya que es una especie de **CONTEO**

```sql
Select titulo,rating_imdb,
RANK() OVER (ORDER BY episodios.rating_imdb DESC) AS'Ranking IMDb'
from episodios
```

## DENSE_RANK

Se utiliza para una secuencia continua de rangos, es decir **no se salta** del 1 al 7 si hay 6 datos que compartan el numero 1, sino que **va de forma continua: 1,2,3,4,5,6** sin importar cuantos datos comparten el mismo filtro

```sql
select titulo, duracion,
dense_RANK() over(order by duracion desc) as 'ranking_por_duracion'
from episodios
```

## REGEXP

Son una secuencia de caracteres que forman un patrón de búsqueda, **no distingue mayúsculas de minúsculas**

```sql
select titulo, descripcion
from series
where descripcion REGEXP '(?i) más'
```

## SOLUCIÓN PROYECTO DÍA 6

```sql
with
ListadoSeries as (
Select titulo, serie_id from series
),
ListadoEpisodios as (
Select count(*) as cantidad_episodios,avg(rating_imdb) as rating_promedio ,serie_id from episodios
group by serie_id

)
select cantidad_episodios, ListadoSeries.titulo, rating_promedio  from ListadoEpisodios
inner join ListadoSeries on ListadoSeries.serie_id = ListadoEpisodios.serie_id
ORDER BY rating_promedio desc, cantidad_episodios desc;
 
```
