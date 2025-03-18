# Lenguaje SQL
## Contenidos
1. Lenguaje de Manipulación de Datos
2. Consultas SELECT básicas
3. Consultas SELECT de AGRUPACIÓN
4. Consultas SELECT multitabla

## Lenguaje de Manipulación de Datos
El **Lenguaje de Manipulación de Datos** (LMD, en inglés Data Manipulation
Language, DML) es un lenguaje proporcionado por el sistema de gestión de base de
datos que permite a los usuarios de la misma llevar a cabo las tareas de consulta o
manipulación de los datos (inserción, borrado, actualización y consultas) basado en
el modelo de datos adecuado.

!!!NOTE [MySQL Oficial – Sintaxis de las instrucciones DML](http://dev.mysql.com/doc/refman/8.0/en/sql-syntax-data-manipulation.html)

Las **consultas** recuperan información de las tablas de una base de datos mediante la
selección de campos, la realización de filtros y la transformación de los datos
recuperados.

El uso de consultas permite:
* **Elegir tablas.** Se puede obtener información de una sola tabla o de varias.
* **Elegir campos.** Se pueden especificar los campos a visualizar de cada tabla.
* **Elegir registros.** Se pueden seleccionar los registros a mostrar en la hoja de
respuestas dinámica, especificando un criterio.
* **Ordenar registros.** Se puede ordenar la información en forma ascendente o
descendente.
* **Realizar cálculos.** Se pueden emplear las consultas para hacer cálculos con los
datos de las tablas.

Además, en consultas más complejas se puede:
* **Crear tablas.** Se puede generar otra tabla a partir de los datos combinados de
una consulta. La tabla se genera a partir de la hoja de respuestas dinámica.
* **Consultas encadenadas.** Utilizar una consulta como origen de datos para otras
consultas (subconsulta). Se pueden crear consultas adicionales basadas en un
conjunto de registros seleccionados por una consulta anterior.


## Consultas SELECT básicas
#### Consultas con operaciones numéricas y literales
Comenzaremos por utilizar la instrucción SELECT como si fuera una calculadora. 
Para ello utilizaremos los siguientes operadores aritméticos:
|Operador |Función |
|---------|--------|
|\+ |Suma|
|\- |Resta|
|\* |Producto o Multiplicación|
|/  |División|
|DIV|División entera|
|MOD o %| Resto división|

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
!!!NOTE  Referencias: <br>[MySQL – Operadores aritméticos](http://mysql.conclase.net/curso/?cap=010b#OPE_ARITMETICOS)<br>[MySQL Oficial - Operadores](https://dev.mysql.com/doc/refman/8.0/en/non-typed-operators.html)

Para el uso de literales o cadenas de caracteres basta con colocar unas comillas:

```sql
SELECT 'Felicidades';
```

```sql
SELECT 'Precio', '25*1.21=', 25*1.21;
```
