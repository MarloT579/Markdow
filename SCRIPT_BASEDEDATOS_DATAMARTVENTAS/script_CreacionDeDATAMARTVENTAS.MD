# Documentación del Esquema de Base de Datos `DATAMARTVENTAS` y ETL

## Equipo: 
* Juan Mateo de Luna 
* Cristian Andres Cid Rodriguez
* Marlot Martinez Sandoval

## Creación de la Base de Datos

```sql
CREATE DATABASE DATAMARTVENTAS;
USE DATAMARTVENTAS;
```
## Creación de Dimensión Producto
```sql
CREATE TABLE [dbo].DimProducto(
    SkProductoID int NOT NULL IDENTITY(1,1) PRIMARY KEY,
    ProductID int NOT NULL,
    ProductName nvarchar(40) NOT NULL,
    CategoryName nvarchar(15) NOT NULL,
    UnitsInStock smallint NULL
); 
```

## Creación de Dimensión Empleado
```sql
CREATE TABLE [dbo].DimEmpleado(
    SkEmpleadoId int NOT NULL IDENTITY(1,1) PRIMARY KEY,
    EmployeeID int NOT NULL,
    Fullname nvarchar(50) NOT NULL,
    Address nvarchar(60) NULL,
    City nvarchar(15) NULL,
    Region nvarchar(15) NULL,
    Country nvarchar(15) NULL
);
```

## Creación de Dimensión Cliente
```sql
CREATE TABLE [dbo].DimCliente(
    SkCustomerID int NOT NULL IDENTITY(1,1) PRIMARY KEY,
    CustomerID nchar(5) NOT NULL,
    CompanyName nvarchar(40) NOT NULL,
    Address nvarchar(60) NULL,
    City nvarchar(15) NULL,
    Region nvarchar(15) NULL,
    Country nvarchar(15) NULL
);
```

## Creación de Dimensión Proveedor
```sql
CREATE TABLE [dbo].DimProveedor(
    SkSupplierId int NOT NULL IDENTITY(1,1) PRIMARY KEY,
    SupplierID int NOT NULL,
    CompanyName nvarchar(40) NOT NULL,
    Address nvarchar(60) NULL,
    City nvarchar(15) NULL,
    Region nvarchar(15) NULL,
    Country nvarchar(15) NULL
);
```

## Creación de Dimensión Tiempo
```sql
CREATE TABLE [dbo].DimTiempo(
    SkTimeId int NOT NULL IDENTITY(1,1) PRIMARY KEY,
    TimeFecha date NOT NULL,
    TimeAnio int NOT NULL,
    TimeTrimestre int NOT NULL,
    TimeMes int NOT NULL,
    TimeDescripcionMes nvarchar(20) NOT NULL,
    TimeDescripcionTrimestre nvarchar(20) NOT NULL
);
```


## Creación de Tablas de Hechos *Fact Ventas*
```sql
CREATE TABLE [dbo].factVentas(
    sk_cliente int NOT NULL,
    sk_proveedor int NOT NULL,
    sk_producto int NOT NULL,
    sk_empleado int NOT NULL,
    sk_TimeId int NOT NULL,
    precioVenta money NOT NULL,
    cantidadUnidades int NOT NULL,
    importe money NOT NULL,
    CONSTRAINT pk_fact_ventas PRIMARY KEY(sk_cliente, sk_proveedor, sk_producto, sk_empleado, sk_TimeId),
    CONSTRAINT fk_fact_cliente FOREIGN KEY(sk_cliente) REFERENCES DimCliente,
    CONSTRAINT fk_fact_proveedor FOREIGN KEY(sk_proveedor) REFERENCES DimProveedor,
    CONSTRAINT fk_fact_producto FOREIGN KEY(sk_producto) REFERENCES DimProducto,
    CONSTRAINT fk_fact_empleado FOREIGN KEY(sk_empleado) REFERENCES DimEmpleado,
    CONSTRAINT fk_fact_TimeId FOREIGN KEY(sk_TimeId) REFERENCES DimTiempo
);
```
![Ejemplo de Imagen](DATAMARTVENTAS.png)

<br>
<br>

# Flujo de control
![Ejemplo de Imagen](1.jpeg)

Aqui se muestran todas las tablas.

<br>
<br>
<br>

# Tabla DimProducto (Flujo de Datos)
![Ejemplo de Imagen](2.jpeg)<br>
Aqui esta el Mapeado de la Tabla DimProducto que esta conformado por los siguientes herramientas:

* `Origen Producto North`<br>
![Ejemplo de Imagen](3.jpeg)<br>
Este siempre va estar ubicado a la izquierda es nuestra base de datos de Northwind<br>
![Ejemplo de Imagen](4.jpeg)<br>
En este siempre iran nuestras consultas que extrae los datos

* `Origen DimProducto`<br>
![Ejemplo de Imagen](5.jpeg)<br>
Este siempre va estar a la derecha y es nuestra base de datos creada llamada DATAMARTVENTAS<br>
![Ejemplo de Imagen](6.jpeg)<br>
Este es en donde se mostaran los datos.

* `Ordenar Producto north / DimProducto`<br>
![Ejemplo de Imagen](7.jpg)<br>
Esto es para seleccionar que se quiere mostrar en este caso se ordenaran por ID.

* `Combinacion de Mezcla`<br>
![Ejemplo de Imagen](8.jpeg)<br>
Esto es para que traer todos la tabla incuyendo los  nuevos datos.<br>
![Ejemplo de Imagen](9.jpeg)<br>

* `Division Condicional`<br>
![Ejemplo de Imagen](10.JPG)<br>
![Ejemplo de Imagen](11.jpeg)<br>
*Esto detecta si hay Producto ID nulos y si es nulo  inserta en la tabla DimProducto el valor de los nuevos campos.*

* `OLE DB Destination`<br>
![Ejemplo de Imagen](13.jpg)<br>
![Ejemplo de Imagen](12.jpg)<br>
Aqui se muestra en la tabla que queremos seleccionar que en este caso es la tabla DimProducto.

<br>
<br>
<br>

# Tabla DimEmpleado (Flujo de Datos)
![Ejemplo de Imagen](16.jpeg)<br>
Aqui esta el Mapeado de la Tabla DimEmpleado que esta conformado por los siguientes herramientas:

* `Origen empleado North`<br>
![Ejemplo de Imagen](14.jpeg)<br>
Este siempre va estar ubicado a la izquierda es nuestra base de datos de Northwind<br>
![Ejemplo de Imagen](17.jpeg)<br>
En este siempre iran nuestras consultas que extrae los datos

* `Origen DimEmpleado`<br>
![Ejemplo de Imagen](15.jpeg)<br>
Este siempre va estar a la derecha y es nuestra base de datos creada llamada DATAMARTVENTAS<br>
![Ejemplo de Imagen](18.jpeg)<br>
Este es en donde se mostaran los datos.

* `Ordenar Empleado north / DimEmpleado`<br>
![Ejemplo de Imagen](19.jpeg)<br>
![Ejemplo de Imagen](20.jpeg)<br>
Esto es para seleccionar que se quiere mostrar en este caso se ordenaran por ID.

* `Combinacion de Mezcla`<br>
![Ejemplo de Imagen](8.jpeg)<br>
Esto es para que traer todos la tabla incuyendo los  nuevos datos.<br>
![Ejemplo de Imagen](21.jpeg)<br>

* `Division Condicional`<br>
![Ejemplo de Imagen](10.JPG)<br>
![Ejemplo de Imagen](22.jpeg)<br>
*Esto detecta si hay Empleado ID nulos y si es nulo  inserta en la tabla DimEmpleado el valor de los nuevos campos.*

* `OLE DB Destination`<br>
![Ejemplo de Imagen](13.jpg)<br>
![Ejemplo de Imagen](23.jpeg)<br>
Aqui se muestra en la tabla que queremos seleccionar que en este caso es la tabla DimEmpleado.

<br>
<br>
<br>

# Tabla DimCliente (Flujo de Datos)
![Ejemplo de Imagen](24.jpeg)<br>
Aqui esta el Mapeado de la Tabla DimCliente que esta conformado por los siguientes herramientas:

* `Origen cliente North`<br>
![Ejemplo de Imagen](25.jpg)<br>
Este siempre va estar ubicado a la izquierda es nuestra base de datos de Northwind<br>
![Ejemplo de Imagen](26.jpg)<br>
En este siempre iran nuestras consultas que extrae los datos

* `Origen DimCliente`<br>
![Ejemplo de Imagen](27.jpeg)<br>
Este siempre va estar a la derecha y es nuestra base de datos creada llamada DATAMARTVENTAS<br>
![Ejemplo de Imagen](28.jpeg)<br>
Este es en donde se mostaran los datos.

* `Ordenar Cliente north / DimCliente`<br>
![Ejemplo de Imagen](29.jpg)<br>
Esto es para seleccionar que se quiere mostrar en este caso se ordenaran por ID.

* `Combinacion de Mezcla`<br>
![Ejemplo de Imagen](8.jpeg)<br>
Esto es para que traer todos la tabla incuyendo los  nuevos datos.<br>
![Ejemplo de Imagen](30.jpeg)<br>

* `Division Condicional`<br>
![Ejemplo de Imagen](10.JPG)<br>
![Ejemplo de Imagen](31.jpeg)<br>
*Esto detecta si hay Cliente ID nulos y si es nulo  inserta en la tabla DimCliente el valor de los nuevos campos.*

* `OLE DB Destination`<br>
![Ejemplo de Imagen](13.jpg)<br>
![Ejemplo de Imagen](12.jpg)<br>
Aqui se muestra en la tabla que queremos seleccionar que en este caso es la tabla DimCliente.

<br>
<br>
<br>

# Tabla DimProveedor (Flujo de Datos)
![Ejemplo de Imagen](32.jpeg)<br>
Aqui esta el Mapeado de la Tabla DimProveedor que esta conformado por los siguientes herramientas:

* `Origen proveedor North`<br>
![Ejemplo de Imagen](25.jpg)<br>
Este siempre va estar ubicado a la izquierda es nuestra base de datos de Northwind<br>
![Ejemplo de Imagen](26.jpg)<br>
En este siempre iran nuestras consultas que extrae los datos

* `Origen DimProveedor`<br>
![Ejemplo de Imagen](33.jpeg)<br>
Este siempre va estar a la derecha y es nuestra base de datos creada llamada DATAMARTVENTAS<br>
![Ejemplo de Imagen](34.jpeg)<br>
Este es en donde se mostaran los datos.

* `Ordenar Proveedor north / DimProveedor`<br>
![Ejemplo de Imagen](29.jpg)<br>
Esto es para seleccionar que se quiere mostrar en este caso se ordenaran por ID.

* `Combinacion de Mezcla`<br>
![Ejemplo de Imagen](8.jpeg)<br>
Esto es para que traer todos la tabla incuyendo los  nuevos datos.<br>
![Ejemplo de Imagen](35.jpeg)<br>

* `Division Condicional`<br>
![Ejemplo de Imagen](10.JPG)<br>
![Ejemplo de Imagen](36.jpeg)<br>
*Esto detecta si hay Proveedor ID nulos y si es nulo  inserta en la tabla DimProveedor el valor de los nuevos campos.*

* `OLE DB Destination`<br>
![Ejemplo de Imagen](13.jpg)<br>
![Ejemplo de Imagen](12.jpg)<br>
Aqui se muestra en la tabla que queremos seleccionar que en este caso es la tabla DimProveedor.

<br>
<br>
<br>

# Tabla DimTiempo (Flujo de Datos)
![Ejemplo de Imagen](40.jpeg)<br>
Aqui esta el Mapeado de la Tabla DimTiempo que esta conformado por los siguientes herramientas:

* `Origen tiempo North`<br>
![Ejemplo de Imagen](25.jpg)<br>
Este siempre va estar ubicado a la izquierda es nuestra base de datos de Northwind<br>
![Ejemplo de Imagen](26.jpg)<br>
En este siempre iran nuestras consultas que extrae los datos

* `Origen DimTiempo`<br>
![Ejemplo de Imagen](41.jpeg)<br>
Este siempre va estar a la derecha y es nuestra base de datos creada llamada DATAMARTVENTAS<br>
![Ejemplo de Imagen](37.jpeg)<br>
Este es en donde se mostaran los datos.

* `Ordenar Tiempo north / DimTiempo`<br>
![Ejemplo de Imagen](7.jpg)<br>
Esto es para seleccionar que se quiere mostrar en este caso se ordenaran por ID.

* `Combinacion de Mezcla`<br>
![Ejemplo de Imagen](8.jpeg)<br>
Esto es para que traer todos la tabla incuyendo los  nuevos datos.<br>
![Ejemplo de Imagen](38.jpeg)<br>

* `Division Condicional`<br>
![Ejemplo de Imagen](10.JPG)<br>
![Ejemplo de Imagen](39.jpeg)<br>
*Esto detecta si hay Tiempo ID nulos y si es nulo  inserta en la tabla DimTiempo el valor de los nuevos campos.*

* `OLE DB Destination`<br>
![Ejemplo de Imagen](13.jpg)<br>
![Ejemplo de Imagen](12.jpg)<br>
Aqui se muestra en la tabla que queremos seleccionar que en este caso es la tabla DimTiempo.