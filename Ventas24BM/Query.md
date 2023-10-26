```sql
-- 1 -- Eduardo Rodríguez 24BM
CREATE PROCEDURE spReportWithDates
    @StartDate DATE = NULL,
	@EndDate DATE = NULL
AS
BEGIN
	SELECT T1.nombre AS Nombre,
			T1.telefono AS Teléfono,
			T1.email AS Correo,
			T2.PkFactura AS Num_Factura,
			T2.fecha AS Fecha_Factura,
			T3.nombre AS Tipo_Pago
	FROM CLIENTE AS T1
	INNER JOIN FACTURA AS T2 ON T1.PkCliente = T2.FkCliente
	LEFT JOIN MODO_PAGO AS T3 ON T3.PkPago = T2.FkPago
	WHERE T2.fecha BETWEEN @StartDate AND @EndDate
END

-- Ejecutar
EXEC spReportWithDates '2023-01-01', '2024-01-01'
```

```sql
-- 2 -- Eduardo Rodríguez 24BM
CREATE PROCEDURE spReportWithClientOrProduct
    @ClientID INT = NULL,
	@ProductName VARCHAR(50) = NULL
AS
BEGIN
	SELECT T1.nombre AS Nombre,
			T2.PkFactura AS Num_Factura
	FROM CLIENTE AS T1
	RIGHT JOIN FACTURA AS T2 ON T1.PkCliente = T2.PkFactura
	WHERE (@ClientID IS NULL OR T1.PkCliente = @ClientID)
	
	SELECT Nombre AS Nombre_Producto
	FROM PRODUCTO
	WHERE (@ProductName IS NULL OR nombre = @ProductName)
END

-- Ejecutar
EXEC spReportWithClientOrProduct @ClientID = 1, @ProductName = 'Botas';
```

```sql
-- 3 -- Eduardo Rodríguez 24BM
CREATE PROCEDURE spReportSalesInfo
    @Sorting VARCHAR(5) = 'ASC',
	@ClientID INT = NULL
AS
BEGIN
	SELECT T1.nombre AS Nombre,
			T2.fecha AS Fecha_Factura,
			T5.nombre AS Nombre_Producto,
			T3.cantidad AS Cantidad,
			T4.otros_detalles AS Método_Pago,
			T3.precio * 2 AS Precio_Doble
	FROM CLIENTE AS T1
	RIGHT JOIN FACTURA AS T2 ON T1.PkCliente = T2.PkFactura
	RIGHT JOIN DETALLE AS T3 ON T2.PkFactura = T3.FkFactura
	RIGHT JOIN MODO_PAGO AS T4 ON T2.FkPago = T4.PkPago 
	INNER JOIN PRODUCTO AS T5 ON T3.FkProducto = T5.PkProducto
	WHERE (@ClientID IS NULL OR T1.PkCliente = @ClientID)
	ORDER BY
        CASE WHEN @Sorting = 'ASC' THEN T1.nombre END ASC,
        CASE WHEN @Sorting = 'DESC' THEN T1.nombre END DESC
END

-- Ejecutar
EXEC spReportSalesInfo 'DESC', 4
```

```sql
-- 4 -- Eduardo Rodríguez 24BM
CREATE PROCEDURE spCustomDiscounts
	@Discount INT
AS
BEGIN
	SELECT T1.nombre AS Nombre,
			T1.stock AS Cantidad,
			T1.precio AS Precio,
			T2.nombre AS Categoría,
			T2.descripcion AS Descripción,
			T1.precio * (1 - (@Discount * 0.01)) AS Precio_Con_Descuento
	FROM PRODUCTO AS T1
	INNER JOIN CATEGORIA AS T2 ON T1.FkCategoria = T2.PkCategoria
END

-- Ejecutar
EXEC spCustomDiscounts 30
```

```sql

```
