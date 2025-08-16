# SQL 5: Consultas Nivel Intermedio
## Subconsultas

Se utiliza cuando quieres seleccionar columnas de una tabla basándote en valores que existen (o no existen) en otra tabla o en un subconjunto de la misma tabla.

```sql
Select titulo
from series
where serie_id in (
SELECT serie_id
FROM episodios
GROUP BY serie_id
Having avg(rating_imdb)> 8)
```

## Función condicional IF

Se crea una columna con valores relacionados a ciertas condición(1), en este caso se crea una columna según una condición del rating 

```sql
select titulo, rating_imdb, IF(rating_imdb>8,'Alto', 'Bajo') as 'Categoria de Rating' 
From episodios
```

## Función condicional CASE

Se utiliza para cuando se quiere crear una columna con valores de acuerdo a ciertas condiciones, es como usar varios where pero creando una columna a partir de condiciones relacionadas a otra columna ya existente.

```sql
Select titulo, case when año_lanzamiento <2010 THEN "Antigua"
else "Reciente"
END as "Antigüedad" from Series

Select titulo, año_lanzamiento,
CASE
	When año_lanzamiento >= 2020 THEN 'Nueva'
	When año_lanzamiento >= 2010 AND año_lanzamiento <=2019 THEN 'Clasica'
ELSE 'Antigua'
END AS categoria 
from series
```

## Función de Conversión CAST

Convierte datos de cualquier tipo en un tipo de datos específico (En general se usa cuando la base de datos está mal modelada)

```sql
-- Ver que tipo de datos tienen las variables de una columna
DESCRIBE EPISODIOS;

-- Cambiar una variable que era numerica a texto
select titulo, cast(año_lanzamiento as CHAR) as  "Año de lanzamiento" FROM Series 
-- cambiar una variable de INT por ejemplo a DATE 
select * from episodios 
where CAST(fecha_estreno as DATE)> '2010-01-01'
```

## Funciones de Fecha

```sql
--DATEADD()
--Para crear una tabla que agrege o quite días por ejemplo a una fecha.
Select fecha_estreno,date_add(fecha_estreno,interval **30** day) as agregar_30_dias from episodios
Select fecha_estreno,date_add(fecha_estreno,interval **-30** day) as agregar_30_dias from episodios

--DATEDIFF()
--Para saber cuánto tiempo pasó desde una fecha a otra
Select * , datediff(CURDATE(),fecha_estreno) as dias_desde_estreno from episodios
```

## **Manipular cadenas de texto**

```sql

-- Cambiar texto a mayúsculas
SELECT UPPER(titulo) AS Titulo_Mayusculas FROM Series;

-- Cambiar texto a minúsculas
SELECT LOWER(nombre) AS nombre_en_minusculas FROM Actores;

-- Unir los textos de 2 columnas
SELECT CONCAT(titulo, ' (', año_lanzamiento, ')') AS Titulo_Año FROM Series;

-- Seleccionar parte del texto de una columna en este caso 5 letras comenzando desde la primera
SELECT SUBSTRING(titulo, 1, 5) AS Extracto_Titulo FROM Episodios;
-- saber la cantidad de caracteres de una columna con texto
SELECT titulo, LENGTH(titulo) AS Longitud_Titulo FROM Series;

-- Seleccionar parte del texto de una columna, en este caso: las 3 primeras y las 3 ultimas letras 
SELECT
    titulo,
    LEFT(titulo, 3) AS Inicio_Titulo,
    RIGHT(titulo, 3) AS Fin_Titulo
FROM Series;
```

## Funciones matemáticas

```sql
-- Se redondea la duracion 
Select titulo, duracion/60.0 as hora_completa, ROUND(duracion/60.0,0) as horas_completas_redondeado from episodios;

-- Se redondea la duracion hacia la hora superior es decir si son 1.01 hrs se redondea a 2 hrs
Select titulo,duracion, ceiling(duracion/60.0) as Horas_completas From episodios;

-- Se redondea la duracion hacia la hora inferior es decir si son 0.99 hrs se redondea a 0 hrs
Select titulo, duracion, floor(duracion/60.0) as Horas_completas From episodios;
```
