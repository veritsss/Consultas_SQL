# SQL 3: Consultas básicas en SQL
## SELECT

Se utiliza para mostrar todos los valores de una variable dentro de una Tabla
```sql
select titulo, temporada from Episodios
```

## DISTINCT

Se utiliza para seleccionar todos los valores **ÚNICOS** de una variable

```sql
select distinct año_lanzamiento from Series
```

## ORDER BY

Se utiliza para ordenar los datos según alguna variable, ya sea de mayor a menor (DESC) o de menor a mayor (ASC)

```sql
select nombre, fecha_nacimiento from Actores
order by fecha_nacimiento DESC
```

## LIMIT

Se utiliza cuando se quieren mostrar solo cierta cantidad de valores

```sql
select nombre from actores
limit 5
```

## WHERE

Se utiliza para establecer cierta condición sobre los datos que se quieren mostrar

**¿Qué filtra?** Filtra **filas individuales**. Cada fila de la tabla se evalúa contra la condición `WHERE`, y solo las filas que cumplen la condición pasan a las siguientes etapas.

```sql
select * from Series where  genero = 'Comedia'
```

## OPERADORES DE COMPARACIÓN

`=` igualdad

`<>` desigualdad

`<` Menor que…

`>` Mayor que…

`<=` Menor o igual que…

`>=` Mayor o igual que…

```sql

select titulo, año_lanzamiento from Series
where año_lanzamiento >2020
```

## OPERADORES LÓGICOS

### AND

Cuando queremos que ambas condiciones se cumplan

```sql
Select * from Series
Where 
(genero = 'Comedia' and genero = 'Animación')
```

### OR

Cuando queremos que cualquiera de las condiciones se cumpla

```sql
Select * from Series
Where 
(genero = 'Comedia' or genero = 'Animación')
```
### XOR

Cuando queremos que cualquiera de las condiciones se cumpla pero no queremos que se cumplan las dos

```sql
Select * from Series
Where 
(genero = 'Comedia' xor genero = 'Animación')
```

## CLÁUSULA IN/NOT IN

### NOT IN

Se usa para seleccionar filas donde un valor **NO ESTÁ** en una lista de valores especificados.

```sql
SELECT * FROM Series 
WHERE genero NOT IN('Comedia')
```

### IN

Se usa para seleccionar filas donde un valor **ESTÁ** en una lista de valores especificados.

```sql
SELECT * FROM Series 
WHERE genero IN('Comedia','fantasía')
```

## CLÁUSULA LIKE

Esta cláusula se utiliza para **FILTRAR** ciertos **VALORES** que contengan algunas letras o palabras específicas

---

Contengan The en cualquier parte

```sql
select titulo from Series 
where titulo like '%The%'

```

Comiencen con The

```sql
select titulo from Series 
where titulo like 'The%'

```

Terminen con The

```sql
select titulo from Series 
where titulo like '%The'

```

## FUNCIONES DE AGREGADO

---

AVG() = Calcular promedio

COUNT() =Contar cantidad de valores

SUM() =Sumar los valores

MIN() =Calcular el valor mínimo

MAX() =Calcular el valor máximo

```sql
select SUM(duracion) from Episodios
```

## CLÁUSULA GROUP BY

La cláusula **`GROUP BY`** en SQL se utiliza para **agrupar filas que tienen los mismos valores en columnas específicas en un conjunto de una o más columnas**. Esto es fundamental cuando quieres aplicar funciones de agregación (como `COUNT`, `SUM`, `AVG`, `MAX`, `MIN`) a subconjuntos de datos, en lugar de a todo el conjunto de resultados.

```sql
SELECT año_lanzamiento, COUNT(serie_id) as cantidad_de_series from Series

group by año_lanzamiento
```

## CLÁUSULA HAVING

La cláusula **`HAVING`** en SQL se utiliza para **filtrar grupos de filas** que han sido creadas con la cláusula `GROUP BY`. A diferencia de `WHERE`, que filtra filas individuales **ANTES** de la agrupación, `HAVING` filtra los resultados de las funciones de agregación **DESPUÉS** de que se han aplicado y los grupos se han formado.

**¿Qué filtra?** Filtra **grupos completos**. Una vez que los datos han sido agrupados y se han calculado los valores agregados para cada grupo, `HAVING` evalúa esos resultados y descarta los grupos que no cumplen su condición.

```sql
select temporada, SUM(duracion) as duracion_total from Episodios
where serie_id = 2
group by temporada
Having sum(duracion)>400
```
