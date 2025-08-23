# Programación en SQL y Bases de datos
## Procedimientos almacenados (Store procedures)

Un **procedimiento almacenado** es como una **función guardada dentro de la base de datos**.

Sirve para no repetir código y ejecutar varias instrucciones de una sola vez.

👉 Ventajas:

- Encapsulas lógica en un solo bloque.
- Evitas repetir queries largas.
- Mejoras seguridad (el usuario llama al procedimiento, no escribe queries complejas).
- Son más rápidos porque quedan **compilados** en la BD.

---

```sql
DELIMITER //

CREATE PROCEDURE AgregarEmpleado(IN _nombre VARCHAR(255), IN _apellido VARCHAR(255),
 IN _email VARCHAR(255), IN _depto_id INT)
BEGIN
    INSERT INTO Empleados (nombre,apellido,email,depto_id) VALUES (_nombre,_apellido,
    _email,_depto_id);
END //

DELIMITER ;

```

```sql
CALL AgregarEmpleado('Elena','Torres','elena.torres@gmail.com', 3);
SELECT * FROM empleados;
```

## 🔹 Transacciones

Una **transacción** es como un **paquete de instrucciones** que se ejecutan todas juntas.

👉 Reglas básicas:

- Si **todo sale bien**, se confirman los cambios (**COMMIT**).
- Si algo **falla**, se revierten los cambios (**ROLLBACK**) y la base de datos queda como estaba antes.

Esto asegura que los datos siempre estén **consistentes** y no queden a medias.

```sql
BEGIN;
SAVEPOINT PreValidacion;
INSERT INTO AsignacionesDeProyectos (proyecto_id, empleado_id, horas_asignadas) VALUES
(5,1,10);
INSERT INTO AsignacionesDeProyectos (proyecto_id, empleado_id, horas_asignadas) VALUES
(5,2,15);
ROLLBACK TO PreValidacion;
```

```sql
DELIMITER //

CREATE PROCEDURE AsignarHorasAProyecto(IN proyectoId INT, IN empleadoId INT,
 IN horasAsignadas INT)
BEGIN
    DECLARE horasTotales INT DEFAULT 0;
    DECLARE horasMaximas INT DEFAULT 100;
    
    -- Iniciar una transacción
    START TRANSACTION;
    
    -- Establecer un punto de guardado
    SAVEPOINT PreValidacion;
    
    -- Calcular el total actual de horas asignadas al proyecto
    SELECT SUM(horas_asignadas) INTO horasTotales 
    FROM AsignacionesDeProyectos 
    WHERE proyecto_id = proyectoId;
    
    -- Asumiendo que SUM() puede devolver NULL si no hay filas, lo convertimos a 0
    SET horasTotales = IFNULL(horasTotales, 0) + horasAsignadas;
    
    -- Verificar si el total excede las horas máximas permitidas
    IF horasTotales > horasMaximas THEN
        -- Revertir a SAVEPOINT si se excede el total de horas
        ROLLBACK TO PreValidacion;
        -- Aunque el ROLLBACK TO SAVEPOINT mantiene la transacción activa, decidimos terminar la operación con un mensaje de error.
        SELECT 'Error: La asignación excede el total de horas permitidas para el proyecto.'
         AS mensaje;
    ELSE
        -- Insertar la nueva asignación si el total está dentro del límite
        INSERT INTO AsignacionesDeProyectos (proyecto_id, empleado_id, horas_asignadas) 
        VALUES (proyectoId, empleadoId, horasAsignadas);
        
        -- Confirmar la transacción si todas las operaciones fueron exitosas
        COMMIT;
    END IF;
END //

DELIMITER ;
```
