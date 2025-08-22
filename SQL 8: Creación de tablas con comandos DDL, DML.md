

# SQL 8: Creación de tablas con comandos DDL, DML y CRUD
## 🔹 DDL (Data Definition Language)

Sirve para **crear y modificar la estructura** de la base de datos (tablas, columnas, relaciones, etc.).

### CREATE TABLE

Para crear tablas con sus distintas funciones

```sql
CREATE DATABASE IF NOT EXISTS EmpresaDB;
USE empresaDB
```

```sql
CREATE TABLE IF NOT EXISTS Departamentos(
	depto_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(255) NOT NULL,
    ubicacion VARCHAR(255)
 );
```

```sql
 CREATE TABLE IF NOT EXISTS Empleados(
 empleado_id INT auto_increment PRIMARY KEY,
 nombre varchar(255) NOT NULL,
 apellido varchar(255) NOT NULL,
 email varchar(255) UNIQUE NOT NULL,
 depto_id INT, FOREIGN KEY(depto_id) REFERENCES Departamentos(depto_id)
 ON DELETE SET NULL
 );
```

```sql

CREATE TABLE IF NOT EXISTS Proyectos (
proyecto_id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(255) NOT NULL,
descripcion TEXT,
fecha_inicio DATE,
fecha_fin DATE
);

CREATE TABLE IF NOT EXISTS AsignacionesDeProyectos (
asignacion_id INT AUTO_INCREMENT PRIMARY KEY,
proyecto_id INT,
empleado_id INT,
horas_asignadas INT NOT NULL,
FOREIGN KEY (proyecto_id) REFERENCES Proyectos(proyecto_id),
FOREIGN KEY (empleado_id) REFERENCES Empleados(empleado_id)
);
```

### ALTER TABLE

Sirve para **cambiar una tabla ya creada** (agregar columnas, cambiar nombres, etc.):

```sql
ALTER TABLE Departamentos ADD COLUMN email_jefe VARCHAR(255)
```

### DROP TABLE

Sirve para **borrar una tabla completa** (incluye todos sus datos):

```sql
DROP TABLE  IF EXISTS AsignacionesDeProyectos 
```

## 🔹DML(Data Manipulating Language)

Se utilizan para insertar, modificar y eliminar datos

### INSERT (Agregar datos)

```sql
INSERT INTO Departamentos (nombre, ubicacion) VALUES
('Recursos Humanos','Edificio B'),
('Marketing','Edificio Central');

```

👉 Agrega filas nuevas a la tabla.

---

### UPDATE (Modificar datos)

```sql
UPDATE Departamentos
SET ubicacion = 'Edificio M'
WHERE nombre = 'Marketing';

```

👉 Cambia la ubicación del departamento "Marketing".

---

### DELETE (Eliminar datos)

```sql
DELETE FROM Departamentos WHERE nombre = 'Marketing';

```

👉 Borra todos los departamentos que se llamen "Marketing".

---

## 🔹 Índices

Un **índice** es como el índice de un libro: sirve para **buscar más rápido**.

```sql
CREATE INDEX idx_apellido ON Empleados(apellido);

```

👉 Ahora si buscas empleados por apellido, la consulta será más rápida.

⚠️ Pero cada vez que insertas o cambias un dato, el índice también se actualiza, por eso no conviene poner muchos.

---

## 🔹 Usuarios y permisos (seguridad)

### Crear usuario

```sql
CREATE USER 'usuario1'@'localhost' IDENTIFIED BY 'claveSegura123';

```

👉 Crea un nuevo usuario que se conecta desde tu PC (`localhost`).

---

### Dar permisos

```sql
GRANT SELECT, INSERT, UPDATE, DELETE
ON EmpresaDB.*
TO 'usuario1'@'localhost';

```

👉 Permite que ese usuario pueda **leer, insertar, modificar y borrar** datos en la base `EmpresaDB`.

---

### Revocar permisos

```sql
REVOKE DELETE ON EmpresaDB.* FROM 'usuario1'@'localhost';

```

👉 Le quitas el permiso de borrar datos.

---

### Ver permisos

```sql
SHOW GRANTS FOR 'usuario1'@'localhost';

```

👉 Muestra qué permisos tiene ese usuario.

---

### Eliminar usuario

```sql
DROP USER 'usuario1'@'localhost';

```

👉 Borra al usuario de la base de datos.
