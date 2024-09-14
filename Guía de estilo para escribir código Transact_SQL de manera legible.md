
# Guía para Mantener la Legibilidad y Buenas Prácticas en Transact-SQL (T-SQL)

La legibilidad y el mantenimiento de código son aspectos clave para un buen desarrollo en **Transact-SQL (T-SQL)**, el dialecto de SQL utilizado en Microsoft SQL Server y Azure SQL Database. 

Esta guía cubre recomendaciones y técnicas para mejorar la legibilidad del código T-SQL, fomentar buenas prácticas, y facilitar su mantenimiento.

---

## 1. **Indentación**

La indentación es clave para mejorar la legibilidad. Asegúrate de aplicar una indentación consistente, preferiblemente de **4 espacios** por nivel de indentación (evita usar tabulaciones).

```sql
-- Correcto
SELECT 
    Columna1,
    Columna2,
    Columna3
FROM 
    Tabla
WHERE 
    Columna1 = 'Valor'
    AND Columna2 > 100;

-- Incorrecto
SELECT Columna1,Columna2,Columna3 FROM Tabla WHERE Columna1='Valor' AND Columna2>100;
```

### Reglas generales:
- Siempre pon cada cláusula SQL (`SELECT`, `FROM`, `WHERE`, `JOIN`, etc.) en líneas separadas.
- Indenta las condiciones dentro de las cláusulas `WHERE` o `JOIN`.

---

## 2. **Uso de Mayúsculas y Minúsculas**

Por convención, las palabras reservadas de T-SQL deben estar en **mayúsculas**, mientras que los nombres de tablas, columnas y variables deben estar en **minúsculas** o en **PascalCase**.

```sql
-- Correcto
SELECT Columna1, Columna2
FROM Tabla
WHERE Columna1 = 'Valor';

-- Incorrecto
select columna1, columna2 from tabla where columna1 = 'Valor';
```

### Consejos:
- El uso de mayúsculas para las palabras clave hace que el código sea más fácil de escanear y distinguir entre lógica de negocio (tablas y columnas) y comandos SQL.
- Usa **minúsculas** o **PascalCase** para tus nombres de tablas, columnas y variables. Esto da uniformidad y claridad.

---

## 3. **Comentarios**

Añadir comentarios es esencial para que otros desarrolladores puedan entender rápidamente el propósito del código. Existen dos tipos de comentarios en T-SQL:

- **Comentarios de una línea**: Usa `--` para añadir comentarios simples.
- **Comentarios de múltiples líneas**: Usa `/* */` para comentar bloques grandes de código o explicaciones.

```sql
-- Este SELECT obtiene todas las ventas del último mes
SELECT VentaID, Monto, FechaVenta
FROM Ventas
WHERE FechaVenta >= DATEADD(MONTH, -1, GETDATE());

/*
    Este bloque selecciona todos los empleados con más de 5 años de servicio
    y calcula el bono de antigüedad.
*/
SELECT EmpleadoID, Nombre, Bono
FROM Empleados
WHERE Antiguedad >= 5;
```

### Buenas Prácticas para los Comentarios:
- Comenta **por qué** haces algo, no solo **qué** hace.
- Mantén los comentarios actualizados si el código cambia.
- Evita comentarios excesivos; solo comenta cuando sea necesario.

---

## 4. **Uso de Alias Claros**

Utiliza **alias** para nombrar tablas y columnas de manera clara y significativa. Evita alias difíciles de entender.

```sql
-- Correcto
SELECT c.CustomerName, o.OrderDate
FROM Customers AS c
INNER JOIN Orders AS o ON c.CustomerID = o.CustomerID;

-- Incorrecto
SELECT a.Name, b.Date
FROM Customers AS a
INNER JOIN Orders AS b ON a.ID = b.ID;
```

### Consejos:
- Los alias deben tener sentido lógico (por ejemplo, `c` para `Customers`, `o` para `Orders`).
- Usa alias en consultas largas para evitar confusión o repeticiones innecesarias.

---

## 5. **Consulta Dividida en Bloques Lógicos**

Divide las consultas en bloques lógicos que faciliten su comprensión, colocando cada cláusula en una línea independiente y aplicando indentación para las subconsultas.

```sql
-- Correcto
SELECT 
    e.EmpleadoID, 
    e.Nombre, 
    d.Nombre AS Departamento
FROM 
    Empleados e
INNER JOIN 
    Departamentos d ON e.DepartamentoID = d.DepartamentoID
WHERE 
    e.Salario > 5000
ORDER BY 
    e.Nombre;

-- Incorrecto
SELECT e.EmpleadoID, e.Nombre, d.Nombre FROM Empleados e INNER JOIN Departamentos d ON e.DepartamentoID = d.DepartamentoID WHERE e.Salario > 5000 ORDER BY e.Nombre;
```

### Ventajas:
- Cada parte de la consulta está separada y es fácil de leer.
- Permite detectar más rápidamente los errores o mejorar partes específicas de la consulta.

---

## 6. **Evitar el Uso de SELECT * (Wildcard)**

Evita utilizar `SELECT *`, ya que esto puede afectar el rendimiento y la legibilidad del código. Siempre especifica las columnas que realmente necesitas.

```sql
-- Correcto
SELECT Columna1, Columna2
FROM Tabla;

-- Incorrecto
SELECT *
FROM Tabla;
```

### Razones:
- Mejora el rendimiento al traer solo las columnas necesarias.
- Hace que el código sea más fácil de entender y mantener.

---

## 7. **Utilizar JOINs Claramente**

Usa los **JOINs** de forma clara y siempre especifica las relaciones explícitamente.

```sql
-- Correcto
SELECT e.EmpleadoID, e.Nombre, d.Nombre AS Departamento
FROM Empleados e
INNER JOIN Departamentos d ON e.DepartamentoID = d.DepartamentoID;

-- Incorrecto
SELECT EmpleadoID, Nombre, DepartamentoNombre
FROM Empleados, Departamentos
WHERE Empleados.DepartamentoID = Departamentos.DepartamentoID;
```

### Consejos:
- Prefiere `INNER JOIN`, `LEFT JOIN`, etc., en lugar de las uniones implícitas (separadas por comas), ya que son más claras y fáciles de mantener.

---

## 8. **Uso de Variables y Nombres Descriptivos**

En T-SQL, las variables deben tener nombres descriptivos y seguir una convención uniforme (como `snake_case` o `PascalCase`), para facilitar su comprensión.

```sql
-- Correcto
DECLARE @total_empleados INT;
DECLARE @nombre_departamento NVARCHAR(100);

-- Incorrecto
DECLARE @x INT;
DECLARE @y NVARCHAR(100);
```

### Buenas Prácticas:
- Usa prefijos como `@total_`, `@nombre_`, `@fecha_` para identificar claramente lo que contiene la variable.
- Mantén nombres claros y fáciles de leer.

---

## 9. **Cuidado con la Anidación de Subconsultas** *(CONSULTAR LA NOTA)*

Aunque las subconsultas son útiles, evita anidarlas profundamente. Las subconsultas complejas pueden volverse difíciles de leer y de mantener. Considera usar **Common Table Expressions (CTE)** cuando sea necesario.

>NOTA: SOLO APLICA PARA CUANDO SE TRABAJA CON SPARK SQL

```sql
-- Correcto: Uso de CTE para mejorar la legibilidad
WITH EmpleadosCTE AS (
    SELECT EmpleadoID, Nombre, Salario
    FROM Empleados
    WHERE Salario > 5000
)
SELECT *
FROM EmpleadosCTE
ORDER BY Nombre;

-- Incorrecto: Subconsulta anidada
SELECT *
FROM (
    SELECT EmpleadoID, Nombre, Salario
    FROM Empleados
    WHERE Salario > 5000
) AS EmpleadosFiltrados
ORDER BY Nombre;
```

### Ventajas de los CTE:
- Mejora la legibilidad de consultas complejas.
- Facilita la depuración y el mantenimiento.

---

## 10. **Evitar Funciones Escalares en el SELECT o WHERE**

Evita el uso de funciones escalares en la cláusula `SELECT` o `WHERE`, ya que pueden afectar negativamente el rendimiento. Si necesitas aplicar lógica compleja, considera utilizar una CTE o una columna calculada.

Aquí tienes un ejemplo que muestra por qué es recomendable evitar funciones escalares en las cláusulas `SELECT` o `WHERE` y cómo utilizar una **Common Table Expression (CTE)** para mejorar la legibilidad y rendimiento.

### Ejemplo: Uso incorrecto de una función escalar en `SELECT` y `WHERE`

Supongamos que tienes una tabla llamada `Empleados` con una columna `Salario` y una función escalar llamada `fn_CalcularImpuesto` que calcula el impuesto sobre el salario de cada empleado.

#### Uso incorrecto (función escalar en `SELECT` y `WHERE`):

```sql
-- Mal rendimiento al usar una función escalar en el SELECT y WHERE
SELECT 
    EmpleadoID,
    Nombre,
    Salario,
    dbo.fn_CalcularImpuesto(Salario) AS Impuesto -- Función escalar en SELECT
FROM 
    Empleados
WHERE 
    dbo.fn_CalcularImpuesto(Salario) > 1000;  -- Función escalar en WHERE
```

### Problemas con este enfoque:

1. **Rendimiento**: La función `dbo.fn_CalcularImpuesto()` se evalúa **por cada fila** en la cláusula `SELECT` y `WHERE`. Esto puede resultar en problemas de rendimiento significativos cuando trabajas con grandes conjuntos de datos.
2. **Uso de índices**: El uso de una función escalar en la cláusula `WHERE` puede impedir que SQL Server use índices en la columna `Salario`, lo que degrada aún más el rendimiento.

---

### Solución: Usar una CTE para evitar funciones escalares

En lugar de aplicar la función escalar directamente en el `SELECT` y `WHERE`, puedes usar una **Common Table Expression (CTE)** o una **columna calculada** para calcular el impuesto de manera más eficiente, calculándolo solo una vez.

#### Uso correcto con CTE:

```sql
-- Mejora del rendimiento utilizando una CTE
WITH EmpleadosConImpuesto AS (
    SELECT 
        EmpleadoID,
        Nombre,
        Salario,
        dbo.fn_CalcularImpuesto(Salario) AS Impuesto  -- Calcular el impuesto solo una vez
    FROM 
        Empleados
)
SELECT 
    EmpleadoID,
    Nombre,
    Salario,
    Impuesto
FROM 
    EmpleadosConImpuesto
WHERE 
    Impuesto > 1000;  -- Filtrar por el impuesto ya calculado
```

### Ventajas del uso de CTE:

1. **Mejor rendimiento**: El cálculo de `dbo.fn_CalcularImpuesto(Salario)` se realiza solo **una vez** por fila en la CTE, en lugar de realizarse dos veces (una para el `SELECT` y otra para el `WHERE`).
2. **Legibilidad**: El código es más claro y fácil de mantener.
---

## 11. **Documentación de Procedimientos Almacenados (Stored Procedures)**

Es fundamental documentar adecuadamente los **procedimientos almacenados** para facilitar su comprensión y mantenimiento. Cada **Stored Procedure** debe tener una descripción clara que incluya los siguientes aspectos:

1. **Parámetros**: Detallar cada parámetro de entrada y salida, su tipo de datos, y qué representa.
2. **Valor devuelto** (si aplica): Explicar qué valor devuelve el procedimiento.
3. **Propósito**: Describir en pocas palabras la lógica principal y el objetivo del procedimiento.
4. **Cualquier precondición** (si la hay): Explicar las condiciones bajo las cuales se debe invocar el procedimiento.
5. **Notas**: Si es necesario aclarar algo.

### Ejemplo de un Stored Procedure con documentación:
```SQL
/*
    Obtener el total de ventas mensuales para un cliente específico.
    
    Parámetros:
    - @ClienteID INT: El ID del cliente para el cual se generan las ventas.
    - @Mes INT: El mes para el cual se desea obtener el total de ventas (1=enero, 12=diciembre).
    - @Anio INT: El año para el cual se desea obtener el total de ventas.
    
    Valor devuelto:
    - Devuelve el total de ventas (SUM) en el mes y año especificado para el cliente.

    Ejemplo de uso:
    EXEC sp_ObtenerVentasMensuales @ClienteID = 1, @Mes = 7, @Anio = 2024;
    
    Nota:
    - Asegúrate de que el cliente y las ventas existen para el mes/año indicados.
    - Si no existen ventas, devolverá NULL.
*/
CREATE PROCEDURE sp_ObtenerVentasMensuales
    @ClienteID INT,
    @Mes INT,
    @Anio INT
AS
BEGIN
    -- Declaración de variables para el total de ventas
    DECLARE @TotalVentas DECIMAL(10, 2);

    -- Calcular el total de ventas del cliente en el mes y año especificado
    SELECT @TotalVentas = SUM(VentaTotal)
    FROM Ventas
    WHERE ClienteID = @ClienteID
        AND MONTH(FechaVenta) = @Mes
        AND YEAR(FechaVenta) = @Anio;

    -- Devolver el total de ventas o NULL si no hay registros
    IF @TotalVentas IS NOT NULL
    BEGIN
        SELECT @TotalVentas AS TotalVentas;
    END
    ELSE
    BEGIN
        SELECT NULL AS TotalVentas;
    END
END;
```

## 12. Documentación de Funciones en T-SQL
De manera similar a los procedimientos almacenados, las funciones deben documentarse con precisión para que otros desarrolladores puedan entender fácilmente su propósito y cómo utilizarlas.

### Ejemplo de una función documentada:
``` sql
/*
    Calcular el monto de descuento en una venta según un porcentaje.
    
    Parámetros:
    - @MontoTotal DECIMAL(10, 2): El monto total de la venta.
    - @PorcentajeDescuento DECIMAL(5, 2): El porcentaje de descuento a aplicar (ejemplo: 10 para 10%).

    Valor devuelto:
    - Devuelve el monto de descuento basado en el porcentaje aplicado al monto total.

    Ejemplo de uso:
    SELECT dbo.fn_CalcularDescuento(1000, 10);
    -- Resultado: 100.00 (10% de 1000)
*/
CREATE FUNCTION dbo.fn_CalcularDescuento
(
    @MontoTotal DECIMAL(10, 2),
    @PorcentajeDescuento DECIMAL(5, 2)
)
RETURNS DECIMAL(10, 2)
AS
BEGIN
    -- Calcular el descuento y devolver el resultado
    RETURN @MontoTotal * (@PorcentajeDescuento / 100);
END;
```
> *By Lorenzo Fuentes Soto*
