|Tema|Joins|
|-|-|
|Fecha|?|
|Notas|El ON es lo que tienen en común|

```sql
-- 1 --
SELECT T1.Nombre, T1.Apellido, T2.Nombre AS Puesto, T3.Nombre AS Departamento, T4.Nombre AS Ciudad
FROM Empleado AS T1
INNER JOIN Puesto AS T2 ON T1.FkPuesto = T2.PkPuesto
LEFT JOIN Departamento AS T3 ON T1.FkDepartamento = T3.PkDepartamento
LEFT JOIN Ciudad AS T4 ON T1.Ciudad = T4.Abreviatura
```

```sql
-- 2 --
SELECT T1.Nombre, T1.Apellido, T2.Nombre AS Puesto, T3.Nombre AS Departamento, T4.Nombre AS Ciudad
FROM Empleado AS T1
INNER JOIN Puesto AS T2 ON T1.FkPuesto = T2.PkPuesto
LEFT JOIN Departamento AS T3 ON T1.FkDepartamento = T3.PkDepartamento
LEFT JOIN Ciudad AS T4 ON T1.Ciudad = T4.Abreviatura
WHERE T4.Nombre = 'QUINTANA ROO' AND T3.Nombre = 'Dirección'
ORDER BY T1.Apellido ASC
```

---

|Tema|Procedimientos almacenados|
|-|-|
|Fecha|Oct 13|
|Notas|Para ejecutar se usa ```execute <nombreDelProcedimiento>```|


```sql
-- 1 --
CREATE PROCEDURE spGetEmpleados
    @Puesto varchar(50),
    @Ciudad varchar(20)
AS
BEGIN
    SELECT A.Nombre,
            A.Apellido,
            A.Dirección,
            B.Nombre As Puesto
    FROM Empleado AS A
    INNER JOIN Puesto AS B ON a.FkPuesto = b.PkPuesto
    WHERE B.Nombre = @Puesto AND A.Ciudad = @Ciudad
END
```

```sql
-- 2 -- Insert y select
CREATE PROCEDURE spGeneratePuesto
    @Nombre varchar(50)
AS
BEGIN
    INSERT INTO Puesto VALUES(@Nombre)
    SELECT *
    FROM Puesto
END
```

```sql
-- 18/10/2023
CREATE PROCEDURE spCreateFullRecord
@AbreviaturaCiudad varchar(50) = null,
	@NombreCiudad varchar(50) = null,
	
	@NombreDepartamento varchar(50) = null,
	
	@NombrePuesto varchar(50) = null,

	@NombreEmpleado varchar(50) = null,
	@ApellidoEmpleado varchar(50) = null,
	@DireccionEmpleado varchar(50) = null
AS
BEGIN
	DECLARE @IdPuesto int = null;
	DECLARE @IdDepartamento int = null;

	EXECUTE spCreateCiudad @AbreviaturaCiudad, @NombreCiudad;
	
	EXECUTE spCreateDepartamento @NombreDepartamento;
	SET @IdDepartamento = @@Identity;
	
	EXECUTE spCreatePuesto @NombrePuesto;
	SET @IdPuesto = @@Identity;
	
	EXECUTE spCreateEmpleado @NombreEmpleado, @ApellidoEmpleado, @DireccionEmpleado, @AbreviaturaCiudad, @IdPuesto, @IdDepartamento;
END
```
