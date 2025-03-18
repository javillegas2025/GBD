# Lenguaje SQL
## Contenidos
1. Lenguaje de Manipulación de Datos
2. Consultas SELECT básicas
3. Consultas SELECT de AGRUPACIÓN
4. Consultas SELECT multitabla

## Lenguaje de Manipulación de Datos
El **Lenguaje de Manipulación de Datos** (LMD, en inglés Data Manipulation Language, DML) es un lenguaje proporcionado por el sistema de gestión de base de datos que permite a los usuarios de la misma llevar a cabo las tareas de consulta o manipulación de los datos (inserción, borrado, actualización y consultas) basado en el modelo de datos adecuado.

!!!NOTE [MySQL Oficial – Sintaxis de las instrucciones DML](http://dev.mysql.com/doc/refman/8.0/en/sql-syntax-data-manipulation.html)

Las **consultas** recuperan información de las tablas de una base de datos mediante la selección de campos, la realización de filtros y la transformación de los datos recuperados.

El uso de consultas permite:
* **Elegir tablas.** Se puede obtener información de una sola tabla o de varias.
* **Elegir campos.** Se pueden especificar los campos a visualizar de cada tabla.
* **Elegir registros.** Se pueden seleccionar los registros a mostrar en la hoja de respuestas dinámica, especificando un criterio.
* **Ordenar registros.** Se puede ordenar la información en forma ascendente o
descendente.
* **Realizar cálculos.** Se pueden emplear las consultas para hacer cálculos con los datos de las tablas.

Además, en consultas más complejas se puede:
* **Crear tablas.** Se puede generar otra tabla a partir de los datos combinados de una consulta. La tabla se genera a partir de la hoja de respuestas dinámica.
* **Consultas encadenadas.** Utilizar una consulta como origen de datos para otras consultas (subconsulta). Se pueden crear consultas adicionales basadas en un conjunto de registros seleccionados por una consulta anterior.


## Consultas SELECT básicas
#### Consultas con operaciones numéricas y literales
Comenzaremos por utilizar la instrucción SELECT como si fuera una calculadora. 
Para ello utilizaremos los siguientes operadores aritméticos:
|Operador |Función |
|:---------|:--------|
|**\+** |Suma|
|**\-** |Resta|
|**\*** |Producto o Multiplicación|
|**/**  |División|
|**DIV**|División entera|
|**MOD** o **%**| Resto división|

**Ejemplos**
Los siguientes ejemplos evaluan las expresiones que hay después de la cláusula SELECT y muestran el resultado por pantalla. El último ejemplo muestra tres columnas.
```sql
SELECT 20*150;
```

```sql
SELECT ((5*4.5)/3)+7;
```
```sql
SELECT 100/3, 100 DIV 3, 100 MOD 3;
```
!!!NOTE  <u>Referencias</u> <br>[MySQL – Operadores aritméticos](http://mysql.conclase.net/curso/?cap=010b#OPE_ARITMETICOS)<br>[MySQL Oficial - Operadores](https://dev.mysql.com/doc/refman/8.0/en/non-typed-operators.html)

Para el uso de literales o cadenas de caracteres basta con colocar unas comillas:

```sql
SELECT 'Felicidades';
```

```sql
SELECT 'Precio', '25*1.21=', 25*1.21;
```

#### Consultas con funciones predefinidas de información
A través de funciones ya predefinidas en el MySQL podemos obtener información sobre diferentes aspectos del  estado de nuestra conexión y del sistema. Algunos ejemplos son:

La versión de MySQL y nuestro identificador (ID) de conexión.
```sql
SELECT version(), connection_id();
```
El usuario actual con el que estamos conectados y la base de datos seleccionada.
```sql
SELECT user(), database();
```
Para saber la fecha y hora (now()) o sólo la fecha current_date()) del sistema.
```sql
SELECT now(), current_date();
```
!!!NOTE  <u>Referencias</u> <br>[MySQL – Funciones de información](https://conclase.net/mysql/curso/sqlfun/CURRENT_USER)<br>[MySQL Oficial – Funciones de información](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html)

#### Consultas básicas a tablas
```sql
SELECT [DISTINCT] campos
[FROM table_references
[WHERE where_definition]
[ORDER BY {col_name | expr | position} [ASC | DESC] , ...]
[LIMIT [offset,] row_count]
```
Es importante conocer el orden de las cláusulas:
```txt
SELECT … FROM … WHERE … ORDER … LIMIT
```
#### Utilidad de captura
En muchas ocasiones nos interesaría llevar un registro de las instrucciones SQL ejecutadas y su resultrado. Conectados con la utilidad `mysql.exe` a MySQL podemos grabar toda la salida a un archivo a través de un comando:


```text
mysql> tee archivo.txt
mysql> … instrucciones SQL …
```

Para terminar la salida:

```text
mysql> notee
```

#### Ejemplos de las primeras consultas
Con XAMPP-MySQL importar la BD de jardinería que facilitará el profesor.

**Ejemplo E52501 - Tablas completas**
Mostrar todos los campos de todos los clientes.
```sql
SELECT *
FROM clientes;
```

**Ejemplo E52502 - Seleccionar campos a mostrar**
Mostrar los campos CodigoCliente, NombreCliente, Telefono, Ciudad, Region, Pais de todos los clientes.
```sql
SELECT CodigoCliente, NombreCliente, Telefono, Ciudad, Region, Pais
FROM clientes;
```

**Ejemplo E52503 – Campos calculados**
Mostrar los campos CodigoCliente, NombreCliente, LimiteCredito de todos los clientes añadiendo un campo calculado que se el LimiteCreditoMensual como LimiteCredito/12. Haced la division entera para evitar decimales. Utilizaremos el **alias de campo** con la palabra reservada **AS**.

```sql
SELECT CodigoCliente, NombreCliente, LimiteCredito, LimiteCredito DIV 12 AS LimiteCreditoMensual
FROM clientes;
```

**Ejemplo E52504 – No mostrar repetidos**
Mostrar las regiones (campo Region) todos los clientes evitando resultandos repetidos.

```sql
SELECT DISTINCT Region
FROM clientes;
```

**Ejemplo E52506 – Ordenar registros**
Mostrar todos los campos de todos los clientes ordenados por LimiteCredito ordenado descendentemente.

```sql
SELECT * FROM clientes
ORDER BY LimiteCredito DESC;
```

**Ejemplo E52507 – Limitar el número de registros a mostrar del resultado**
Utilizando la consulta del ejercicio anterior E5106 mostrar:
* Sólo los 5 primeros registros
* Empezando en el tercer registro, mostrar 10 registros

```sql
SELECT * 
FROM clientes
ORDER BY LimiteCredito DESC
LIMIT 5;
```

```sql
SELECT *
FROM clientes
ORDER BY LimiteCredito DESC
LIMIT 2,10;
```

#### Consultas con selección de registros
Para poder seleccionar registros es necesario indicar la condición que deben cumplir los campos de un registro para ser mostrado. Para ello se utiliza la sección **WHERE** de la instrucción SQL.
Los operadores relacionales que nos permiten comparar el valor de los campos son:

|Operador              | Función                                 |
|:----------------------|:-----------------------------------------|
| **=**                    | Igual a                                 |
| **<**                   | Menor que                               |
| **>**                    | Mayor que                               |
| **<=**                   | Menor o igual                           |
| **>=**                   | Mayor ou igual                          |
| **LIKE**                 | Patrón que debe cumprir un campo cadena |
| **BETWEEN**              | Intervalo de valores                    |
| **IN**                   | Conjunto de valores                     |
| **IS NULL**              | Es nulo                                  |
| **IS NOT NULL**         | No es nulo                              |

Veamos unos cuantos ejemplos:

**Ejemplo E52601 – Buscar un registro por el valor de su clave**
Mostrar todos los campos del cliente con código igual a 12.
>**Nota:** Al ser el campo clave, el resultado sólo mostrará un registro.

```sql
SELECT *
FROM clientes
WHERE CodigoCliente = 12;
```

**Ejemplo E52602 – Buscar registros por el valor de un campo**
Mostrar todos los campos de los clientes de la Región Madrid.
>**Nota:** Al no ser un campo clave, el resultado puede mostrar más de un registro.

```sql
SELECT *
FROM clientes
WHERE Region = 'Madrid';
```

**Ejemplo E52603 – Buscar registros por comparación del valor de un campo**
Mostrar todos los campos de los clientes con Límite de Crédito mayor de 50000€.

```sql
SELECT *
FROM clientes
WHERE LimiteCredito > 50000;
```

**Ejemplo E52604 – Buscar registros por comparación del valor de un campo**
Mostrar todos los campos de los clientes con Límite de Crédito entre 10000€ y 60000€.

```sql
SELECT *
FROM clientes
WHERE LimiteCredito BETWEEN 10000 AND 60000;
```

**Ejemplo E52605 – Buscar registros por comparación del valor de un campo**
Mostrar todos los campos de los clientes con la Región nula.

```sql
SELECT *
FROM clientes
WHERE Region IS NULL;
```

**Ejemplo E52606 – Ejemplo combinando con el apartado anterior**
Mostrar los Países de los clientes con la Región nula, sin repetir valores.

```sql
SELECT DISTINCT Pais 
FROM clientes
WHERE Region IS NULL;
```

**Ejemplo E52607 – Ejemplo con LIKE**
Mostrar los Empleados cuyo email contenga "jardin".

```sql
SELECT *
FROM Empleados
WHERE Email LIKE '%jardin%';
```
<table>
<tr><td><b>%</b></td><td>es una cadena de caracteres cualquiera</td></tr>
<tr><td><b>_</b></td><td>es un sólo carácter cualquiera</td></tr>
</table>

**Ejemplo E52608 –Ejemplo con IN**
Mostrar los productos de las gamas 'Aromáticas', 'Herramientas' y Ornamentales'.

```sql
SELECT *
FROM productos
WHERE Gama IN ('Aromáticas', 'Herramientas', 'Ornamentales');
```

!!!NOTE  <u>Referencias</u> <br>[MySQL – Consultas](http://mysql.conclase.net/curso/?cap=009)<br>[MySQL – Operadores de comparación](http://mysql.conclase.net/curso/?cap=010a#OPE_COMPARACION)<br>[MySQL Oficial – Operadores de comparación](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html)


#### Consultas con varias condiciones
Para poder realizar varias varias condiciones necesitamos combinarlas con operadores lógicos, que en MySQL son:

|Operador|Función|
|:--------:|:-------:|
|**AND**|Y|
|**OR**|O|
|**NOT**|No|

Veamos unos cuantos ejemplos:


**Ejemplo E52701 – Ejemplo con varias condiciones**
Mostrar codigo y nombre de los Productos que sean de la Gama 'Frutales' y CantidadEnSock sea mayor que 50 unidades.

>**Nota:** Mostremos también los campos implicados en las condiciones para comprobar el resultado.

```sql
SELECT CodigoProducto, Nombre, Gama,
FROM productos
WHERE Gama='Frutales' AND CantidadEnStock > 50);
```

**Ejemplo E52702 – Ejemplo con varias condiciones**
Mostrar codigo y nombre de los Clientes que sean de la Ciudad 'Madrid' o 'Barcelona'.

>**Nota:** Mostremos también los campos implicados en las condiciones para comprobar el resultado.

```sql
SELECT CodigoCliente, NombreCliente, Ciudad
FROM clientes
WHERE Ciudad='Madrid' OR Ciudad='Barcelona';
```

!!!NOTE  <u>Referencias</u> <br>[MySQL – Consultas y operadores de comparación](http://mysql.conclase.net/curso/?cap=010#OPE_LOGICOS)<br>[MySQL Oficial – Consultas y operadores de comparación](https://dev.mysql.com/doc/refman/8.0/en/logical-operators.html)


#### Consultas con funciones
Las funciones nos permiten realizar transformaciones de los datos para obtener información. Existen multitud de funciones que procesan diferentes tipos de datos.

***<u>Algunas funciones con cadenas de caracteres</u>***
| Función                     | Descripción                                                    |
|:-----------------------------|:---------------------------------------------------------------|
| **CONCAT(cad1, cad2, ...)**   | Concatena cadenas                                             |
| **UPPER(cad)**                 | Pasa a mayúsculas una cadena                                  |
| **LOWER(cad)**                 | Pasa a minúsculas una cadena                                  |
| **LTRIM(cad)**                | Elimina de la cadena los espacios iniciales y finales         |
| **LEFT(cad, X)**              | Obtiene los X primeros caracteres de la cadena                |
| **RIGHT(cad, X)**           | Obtiene los X últimos caracteres de la cadena                 |
| **LENGTH(cad)**               | Longitud de una cadena                                        |
| **REPLACE(cad, ant, pos)**     | Obtiene una cadena tomando **cad** como origen y cambiando **ant** por **pos**|

***<u>Algunas funciones numéricas de un campo</u>***
| Función          | Descripción                                     |
|:------------------|:------------------------------------------------|
| **RAND()**           | Número aleatorio entre 0 y 1                   |
| **POW(num, exp)**    | Obtiene la potencia de num^exp                  |
| **FLOOR(num)**       | Obtiene la parte entera de un número decimal    |
| **ROUND(num, X)**    | Redondea un número a X decimales                |
| **SIGN(num)**        | Devuelve 1 para positivo, 0 para 0 y -1 para negativo |
| **ABS(num)**         | Obtiene el valor absoluto de un número          |

***<u>Algunas funciones de fechas de un campo*</u>**
| Función                  | Descripción                                      |
|:-------------------------|:-------------------------------------------------|
| **YEAR(campo)**            | Muestra el año del valor de un campo de tipo fecha |
| **MONTH(campo)**           | Muestra el mes del valor de un campo de tipo fecha |
| **DAY(campo)**             | Muestra el día del valor de un campo de tipo fecha |
| **DAYNAME(campo)**         | Muestra el nombre del día de la semana del campo   |
| **DAYOFWEEK(campo)**       | Devuelve un número indicando el día de la semana de un campo fecha: <br>**Nota:** 1=Domingo, 2=Lunes, 3=Martes, 4=Miércoles, 5=Jueves, 6=Viernes, 7=Sábado. |

***<u>Algunas funciones numéricas con varios registros</u>***
| Función       | Descripción                                      |
|:--------------|:-------------------------------------------------|
| **COUNT(*)**     | Cuenta los registros seleccionados               |
| **MIN(campo)**   | Valor mínimo del campo de los registros seleccionados |
| **MAX(campo)**   | Valor máximo del campo de los registros seleccionados |
| **SUM(campo)**   | Suma de los valores del campo                    |
| **AVG(campo)**   | Media de los valores del campo                   |

!!!Note <u>Referencias</u><br>[MySQL w3schools – Funciones de MySQL](https://www.w3schools.com/sql/sql_ref_mysql.asp)<br>[MySQL Oficial – Funciones](https://dev.mysql.com/doc/refman/8.0/en/functions.htm)


**<u>Ejemplos de funciones para campos calculados y para condiciones</u>**

**Ejemplo E52801 – Ejemplo con funciones**
Mostrar código, nombre y "precio de venta al público" de los productos, pero ese precio debe ser con IVA incluido, es decir, agregarle al PrecioVenta el 21% multiplicándolo por 1.21. Asignar el alias **pvp** al nuevo campo calculado.

```sql
SELECT CodigoProducto, Nombre, PrecioVenta, ROUND(PrecioVenta * 1.21, 2) AS pvp
FROM Productos;
```

**Ejemplo E52802 – Ejemplo con funciones en las condiciones**
Seleccionar en la consulta anterior los productos que tengan el nuevo campo pvp mayor de 100€

```sql
SELECT CodigoProducto, Nombre, PrecioVenta, ROUND(PrecioVenta * 1.21, 2) AS pvp
FROM Productos
WHERE ROUND(PrecioVenta * 1.21, 2) > 100;
```

**<u>Ejemplos de funciones con cadenas de caracteres</u>**

**Ejemplo E52803 – Ejemplo con funciones**
Seleccionar de los empleados el nombre completo (NombreCompleto) concatenando el nombre y los dos apellidos.

```sql
SELECT CONCAT(Nombre,' ',Apellido1,' ',Apellido2) AS NombreCompleto
FROM empleados;
```

**Ejemplo E52804 – Ejemplo con funciones**
Obtener la inicial del nombre de todos los empleados.

```sql
SELECT LEFT(Nombre,1) AS inicial 
FROM empleados;
```

**Ejemplo E52805 – Ejemplo con funciones**
Obtener el nombre de los empleados todo en mayúsculas.

```sql
SELECT UCASE(Nombre)
FROM empleados;
```

**Ejemplo E52806 – Ejemplo con funciones**
Obtener el código de las oficinas pero sustituyendo el guión por la barra, es decir, el '-' por '/'.

```sql
SELECT REPLACE(CodigoOficina,'-','/')
FROM oficinas;
```

**Ejemplo E52807 – Ejemplo con funciones**
Obtener la primera palabra del nombre del proveedor de todos los productos.

```sql
SELECT SUBSTRING_INDEX(Proveedor, ' ', 1)
FROM productos;
```

**Ejemplo E52808 – Ejemplo con funciones**
Obtener el código de los productos pero conel orden de los caracteres invertido.

```sql
SELECT CodigoProducto, REVERSE(CodigoProducto)
FROM productos;
```

**Ejemplo E52809 – Ejemplo con funciones**
Obtener un gráfico de barras para el stock de cada producto. Utilizaremos el carácter '|' por cada unidad de stock.

```sql
SELECT Codigoproducto,REPEAT('|',CantidadEnStock)
FROM productos;
```

**Ejemplo E52810 – Ejemplo con funciones**
Obtener el email de cada empleado teniendo en cuenta que el usuario es su nombre en minusculas y el dominio '@iesdoctorbalmis.com'

```sql
SELECT CONCAT(LCASE(nombre),'@iesdoctorbalmis.com') AS email
FROM empleados;
```

**Ejemplo E52811 – Ejemplo con funciones**
Obtener la abreviatura del nombre y primer apellidos de los empleados concatenando la inicial del nombre y la inicial del apellido1

```sql
SELECT CONCAT(LEFT(Nombre,1),LEFT(Apellido1,1)) AS inicial
FROM empleados;
```

**<u>Ejemplos de funciones numéricas</u>**

**Ejemplo E52812 – Ejemplo con funciones**
Obtener un número aleatorio entre 0 y 9.

```sql
SELECT FLOOR(RAND()*10);
```

**<u>Ejemplos de funciones de fechas</u>**
**Ejemplo E52813 – Ejemplo con funciones**
Mostrar en campos diferentes el año, el mes y el día de la fecha de los pedido.

```sql
SELECT YEAR(FechaPedido), MONTH(FechaPedido), DAY(FechaPedido)
FROM pedidos;
```

**Ejemplo E52814 – Ejemplo con funciones**
Mostrar todos los campos de pedidos realizados en enero del 2009.

```sql
SELECT *
FROM pedidos
WHERE YEAR(FechaPedido)='2009' AND MONTH(FechaPedido)='01';
```

**Ejemplo E52815 – Ejemplo con funciones**
Mostrar todos los campos de pedidos realziados en lunes.

```sql
SELECT *
FROM pedidos
WHERE DAYOFWEEK(FechaEntrega)=2;
```

**<u>Ejemplos de funciones numéricas con varios registros</u>**

**Ejemplo E52816 – Ejemplo con funciones**
Mostrar el número total de pagos que se han recibido.

```sql
SELECT COUNT(*)
FROM pagos;
```

**Ejemplo E52817 – Ejemplo con funciones**
Mostrar la cantidad en euros total que nos han ingresado.

```sql
SELECT SUM(Cantidad)
FROM pagos;
```

**Ejemplo E52818 – Ejemplo con funciones**
Mostrar el importe del pago más pequeño y el importe de pago más grande recibido.

```sql
SELECT MIN(Cantidad), MAX(Cantidad)
FROM pagos;
```
