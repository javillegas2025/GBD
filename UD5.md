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

> MySQL Oficial – Sintaxis de las instrucciones DML<br>
> http://dev.mysql.com/doc/refman/8.0/en/sql-syntax-data-manipulation.html

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

```sql
SELECT column1, column2 FROM table_name;
```
Además, en consultas más complejas se puede:
* **Crear tablas.** Se puede generar otra tabla a partir de los datos combinados de
una consulta. La tabla se genera a partir de la hoja de respuestas dinámica.
* **Consultas encadenadas.** Utilizar una consulta como origen de datos para otras
consultas (subconsulta). Se pueden crear consultas adicionales basadas en un
conjunto de registros seleccionados por una consulta anterior.

