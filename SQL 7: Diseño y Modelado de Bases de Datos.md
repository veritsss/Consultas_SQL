# SQL 7: Diseño y Modelado de Bases de Datos

En el contexto del modelado de datos, un MER (Modelo Entidad-Relación) es un esquema conceptual que representa la estructura de una base de datos, mientras que un DER (Diagrama Entidad-Relación) es la representación visual de ese esquema utilizando símbolos. En resumen, **el MER es la abstracción teórica, y el DER es su representación gráfica**. 

## Diagrama de Entidad y **R**elación (DER)

Entidad: Una Tabla

Atributos: Columnas las cuales tienen filas con la información de los usuarios

Tipo de variables: PK - FK 


<img width="978" height="797" alt="image (1)" src="https://github.com/user-attachments/assets/062de742-e93a-459e-be27-6261928afa27" />

---

🔑 **PK (Clave primaria):** el identificador único de cada fila (como el RUT).

- Sirve para identificar de forma única a cada registro en una tabla.
- Ejemplo: en una lista de clientes, cada cliente tiene su **número de cliente** que no se repite.

---

🌍 **FK (Clave foránea) :** el dato que conecta con otra tabla (como decir "esta compra pertenece a este cliente").

- Une una tabla con otra.
- Ejemplo: en una lista de compras, aparece el **número de cliente** que hizo la compra. Ese número se busca en la lista de clientes para saber a quién pertenece.
---

## CARDINALIDADES

🔗 **Cardinalidad:** Es la manera de describir **cómo se relacionan dos listas entre sí**

1. **Uno a uno (1:1):** una persona tiene un solo pasaporte.
2. **Uno a muchos (1:N):** un cliente puede hacer muchos pedidos, pero cada pedido solo pertenece a un cliente.
3. **Muchos a muchos (N:M):** un estudiante puede estar en muchos cursos, y un curso puede tener muchos estudiantes.


<img width="680" height="450" alt="image" src="https://github.com/user-attachments/assets/bbe8ded5-bc23-4726-bf37-f12afe75db37" />

---
## NORMALIZACIÓN DE BASES DE DATOS

La **normalización** es como “ordenar” la base de datos para que no se repita información innecesaria y los datos queden consistentes.

Se hace en **niveles** (llamados “formas normales”).

**Normalización** = orden y sin repeticiones (pero más tablas, consultas más complejas).

**Denormalización** = algunos datos repetidos (más espacio en disco), pero consultas más rápidas y simples.

### ✅ Tipos principales de normalización (las más usadas en la práctica):

### 🔹 **Primera Forma Normal (1FN)**

👉 Regla: **no mezcles cosas en una misma casilla**.

- Mal: poner en una sola celda “123, 456, 789” como si fueran varios teléfonos.
- Bien: poner un teléfono por fila.

### 🔹 **Segunda Forma Normal (2FN)**

👉 Regla: **separa la información que no corresponde**.

- Ejemplo: si en cada pedido de “Juan” vuelves a escribir su dirección, estás repitiendo de más.
- Lo correcto: tener una tabla de clientes con su dirección, y la tabla de pedidos solo guarda a qué cliente pertenece.

### 🔹 **Tercera Forma Normal (3FN)**

👉 Regla: **no guardes datos que dependen de otra cosa**.

- Ejemplo: si en la tabla de pedidos además guardas el “nombre de la ciudad” junto con el “ID de ciudad”, eso sobra.
- Mejor: dejas solo el ID de ciudad y creas una tabla de ciudades donde esté el nombre.

---

## TIPOS DE DATOS

### **🔢 Números**

- **INT** → números enteros (1, 25, -300). Ej: ID de cliente, cantidad.
- **DECIMAL / NUMERIC** → números con decimales exactos (10.50, 99.99). Ej: precios, dinero.
- **FLOAT / DOUBLE** → números con decimales pero más “flexibles” (menos exactos). Ej: medidas científicas.

### 🔤 **Texto**

- **CHAR(n)** → texto fijo (siempre ocupa n caracteres). Ej: códigos como “CL” o “USA”.
- **VARCHAR(n)** → texto variable hasta n caracteres. Ej: nombres, correos.
- **TEXT** → textos largos. Ej: descripciones, comentarios.

### 📅 **Fechas y tiempos**

- **DATE** → solo la fecha (2025-08-18).
- **TIME** → solo la hora (14:30:00).
- **DATETIME / TIMESTAMP** → fecha + hora (2025-08-18 14:30:00).

### ✅ **Booleanos**

- **BOOLEAN / BIT** → verdadero o falso (1 o 0). Ej: ¿activo?, ¿sí/no?

### 🔑 **Otros que a veces se usan**

- **BLOB** → archivos binarios (imágenes, PDFs).
- **UUID** → identificadores únicos en formato largo.
