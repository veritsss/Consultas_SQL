# SQL 10: Optimización y Automatización en SQL
## 🔹 VISTAS

Es una consulta que guarda el contenido de una tabla

Una **vista** es como una **ventana a la base de datos**:

- No guarda datos nuevos, solo muestra una consulta guardada con nombre.
- Si cambian los datos en la tabla original, la vista también muestra los cambios automáticamente.

👉 Beneficios:

- Simplifican consultas largas.
- Mejoran la seguridad (puedes dar acceso a la vista y no a toda la tabla).
- Aseguran consistencia: todos usan la misma “receta de consulta”.

```sql
CREATE VIEW VistaEmpleadosTecnología AS
Select nombre, apellido, email
FROM Empleados
WHERE depto_id = 1;

SELECT * FROM VistaEmpleadosTecnología
```

## 🔹 VISTAS MATERIALIZADAS

Parecen vistas normales, pero **sí guardan los datos en disco** (como una tabla temporal).

- Esto hace que sean más rápidas (mejor rendimiento).
- Pero no se actualizan solas automáticamente (debes refrescarlas).
- ⚠️ **MySQL no las soporta**, pero otros gestores como Oracle o PostgreSQL sí.

```sql
CREATE MATERIALIZED VIEW VistaEmpleadosTecnología AS
Select nombre, apellido, email
FROM Empleados
WHERE depto_id = 1;
SELECT * FROM VistaEmpleadosTecnología
```

## 🔹 TRIGGERS (Disparadores)

Son reglas que le decimos a nuestra base de datos que tienen que seguir cuando ocurra cierto evento especifico:

- Se activa cuando ocurre un evento (INSERT, UPDATE, DELETE).
- Permite ejecutar código automáticamente en respuesta a ese evento.

En este caso cada vez que se agregue un nuevo empleado se va a insertar en el trigger LogEmpleados el id del empleado y la fecha en la que se registró este empleado

```sql
DELIMITER $$
CREATE TRIGGER RegistrarNuevoEmpleado
AFTER INSERT ON Empleados
FOR EACH ROW
BEGIN
	INSERT INTO LogEmpleados (empleado_id,fecha_registro) VALUES
	(NEW.empleado_id, NOW());
END$$

DELIMITER ;
```

```sql
INSERT INTO Empleados (nombre,apellido,email) VALUES ('Juan',
 'Robles', 'juan.robles@gmail.com');
 SELECT * FROM LogEmpleados
```
