---
title: Apuntes
date: 16/06/2021
export_on_save:
    puppeteer: true
    html: true
puppeteer:
    scale: 1
    landscape: false
    format: "A4"
    printBackground: true
    margin:
        top: "1cm"
        right: "1cm"
        bottom: "2.5cm"
        left: "1cm"
    displayHeaderFooter: true
    headerTemplate: "&nbsp;"
    footerTemplate: "
        <span style=\"font-size: 9pt; display: flex;\">
            <span class=\"pageNumber\" style=\"margin-left: 1cm;\"></span>
            /
            <span class=\"totalPages\"></span>
            <span class=\"title\" style=\"margin-left: 1cm;\"></span>
            <span style=\"margin-left: 1cm;\">Xusa García y Juanjo Guarinos</span>
        </span>
                    "
toc:
    depth_from: 1
    depth_to: 5
    ordered: false
---

# Llenguatge SQL

## Continguts

1. Llenguatge de Manipulació de Dades
2. Consultes **`SELECT`** bàsiques
3. Consultes **`SELECT`** d'AGRUPACIÓ
4. Consultes **`SELECT`** multitaula

## Llenguatge de Manipulació de Dades

<div class="caso_estudio">

El **Llenguatge de Manipulació de Dades** (**LMD**, en anglès Data Manipulation Language, **DML**) és un llenguatge proporcionat pel sistema de gestió de bases de dades que permet als usuaris de la mateixa dur a terme les tasques de consulta o manipulació de les dades (inserció, esborrat, actualització i consultes) basat en el model de dades adequat.
</div> <!-- fi cas d'estudi -->

!!!NOTE Referències
    [MySQL Oficial – Sintaxi de les instruccions DML](http://dev.mysql.com/doc/refman/8.0/en/sql-syntax-data-manipulation.html)

<div class="caso_estudio">

Les **consultes** recuperen informació de les taules d'una base de dades mitjançant la selecció de camps, la realització de filtres i la transformació de les dades recuperades.
</div> <!-- fi cas d'estudi -->

L'ús de consultes permet:

* **Triar taules.** Es pot obtenir informació d'una sola taula o de diverses.
* **Triar camps.** Es poden especificar els camps a visualitzar de cada taula.
* **Triar registres.** Es poden seleccionar els registres a mostrar al full de respostes dinàmic, especificant un criteri.
* **Ordenar registres.** Es pot ordenar la informació en forma ascendent o descendent.
* **Realitzar càlculs.** Es poden emprar les consultes per fer càlculs amb les dades de les taules.

A més, en consultes més complexes es pot:

* **Crear taules.** Es pot generar una altra taula a partir de les dades combinades d'una consulta. La taula es genera a partir del full de respostes dinàmic.
* **Consultes encadenades.** Utilitzar una consulta com a origen de dades per a altres consultes (subconsulta). Es poden crear consultes addicionals basades en un conjunt de registres seleccionats per una consulta anterior.

## Consultes **`SELECT`** bàsiques

### Consultes amb operacions numèriques i literals

Començarem per utilitzar la instrucció **`SELECT`** com si fos una calculadora. Per a això utilitzarem els següents operadors aritmètics:

| Operador        | Funció                   |
| :-------------- | :----------------------- |
| **\+**          | Suma                    |
| **\-**          | Resta                   |
| **\***          | Producte o Multiplicació |
| **/**           | Divisió                 |
| **DIV**         | Divisió entera          |
| **MOD** o **%** | Resta divisió           |

Els següents exemples avaluen les expressions que hi ha després de la clàusula **`SELECT`** i mostren el resultat per pantalla.

!!!Example Exemple 1
    La última sentència mostra tres columnes.
    ```sql
    -- Eixida: 3000
    SELECT 20 * 150;

    -- Eixida: 14.5
    SELECT ((5 * 4.5) / 3) + 7;

    -- Eixida: 33.333333333, 33, 1
    SELECT 100 / 3, 100 DIV 3, 100 MOD 3;
    ```

!!!Example Exemple 2
    Per a l'ús de literals o cadenes de caràcters n'hi ha prou amb col·locar unes cometes.
    ```sql
    -- Eixida: Felicidades
    SELECT 'Felicidades';

    -- Eixida: Precio, 25 * 1.21 =, 30.25
    SELECT 'Precio', '25 * 1.21 =', 25 * 1.21;
    ```

!!!NOTA Referències
    [MySQL – Operadors aritmètics](http://mysql.conclase.net/curso/?cap=010b#OPE_ARITMETICOS)
    [MySQL Oficial - Operadors](https://dev.mysql.com/doc/refman/8.0/en/non-typed-operators.html)

### Consultes amb funcions predefinides d'informació

A través de funcions ja predefinides en el MySQL podem obtenir informació sobre diferents aspectes de l'estat de la nostra connexió i del sistema. Alguns exemples són:

!!! Example Exemple 1
    La **versió** de MySQL i el nostre identificador (**ID**) de connexió.
    ```sql
    SELECT version(), connection_id();
    ```

!!! Example Exemple 2
    L'usuari actual amb el qual estem connectats i la base de dades seleccionada.
    ```sql
    SELECT user(), database();
    ```

!!! Example Exemple 3
    Data i hora (**`now()`** o només la data **`current_date()`**) del sistema.
    ```sql
    SELECT now(), current_date();
    ```

!!!NOTA Referències
    [MySQL – Funcions d'informació](https://conclase.net/mysql/curso/sqlfun/CURRENT_USER)
    [MySQL Oficial – Funcions d'informació](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html)

### Consultes bàsiques a taules

```sql
SELECT [DISTINCT] camps
[FROM table_references]
[WHERE where_definition]
[ORDER BY {col_name | expr | position} [ASC | DESC] , ...]
[LIMIT [offset,] row_count]
```

<div class="caso_estudio">

:hand: **Important**
L'ordre de les clausules, **obligatòriament**, és:
**SELECT … FROM … WHERE … ORDER … LIMIT**

</div> <!-- fi cas d'estudi -->

### Utilitat de captura

En moltes ocasions ens interessaria portar un registre de les instruccions SQL executades i el seu resultat. Connectats amb la utilitat **`mysql.exe`** a MySQL podem gravar tota la sortida a un fitxer a través d'una comanda:

```text
mysql> tee archivo.txt
mysql> … instruccions SQL …**
```

Per acabar la sortida:

```text
mysql> notee
```

#### Exemples de les primeres consultes

Amb XAMPP-MySQL importar la BD de jardineria que facilitarà el professor.

!!! Example Exemple 1
    **Taules completes**
    Mostrar tots els camps de tots els clients.
    ```sql
    SELECT *
    FROM clientes;
    ```

!!! Example Exemple 2
    **Seleccionar camps a mostrar**
    Mostrar els camps **`CodigoCliente`**, **`NombreCliente`**, **`Telefono`**, **`Ciudad`**, **`Region`**, **`Pais`** de tots els clients.
    ```sql
    SELECT CodigoCliente, NombreCliente, Telefono, Ciudad, Region, Pais
    FROM clientes;
    ```

!!! Example Exemple 3
    **Camps calculats**
    Mostrar els camps **`CodigoCliente`**, **`NombreCliente`**, **`LimiteCredito`** de tots els clients afegint un camp calculat que és el **`LimiteCreditoMensual`** com **`LimiteCredito / 12`**. Feu la divisió entera per evitar decimals. Utilitzarem l'**àlies de camp** amb la paraula reservada **`AS`**.
    ```sql
    SELECT CodigoCliente, NombreCliente, LimiteCredito, LimiteCredito DIV 12 AS LimiteCreditoMensual
    FROM clientes;
    ```

!!! Example Exemple 4
    **No mostrar repetits**
    Mostrar les regions (camp **`Region`**) de tots els clients evitant resultats repetits.
    ```sql
    SELECT DISTINCT Region
    FROM clientes;
    ```

!!! Example Exemple 5
    **Ordenar registres**
    Mostrar tots els camps de tots els clients ordenats per **`LimiteCredito`** ordenat descendentment.
    ```sql
    SELECT * FROM clientes
    ORDER BY LimiteCredito DESC;
    ```

!!! Example Exemple 6
    **Limitar el nombre de registres a mostrar del resultat**
    Utilitzant la consulta de l'exemple anterior mostrar:

    * Només els 5 primers registres
        ```sql
        SELECT * 
        FROM clientes
        ORDER BY LimiteCredito DESC
        LIMIT 5;
        ```
    * Començant en el tercer registre, mostrar 10 registres
        ```sql
        SELECT *
        FROM clientes
        ORDER BY LimiteCredito DESC
        LIMIT 2,10;
        ```

### Consultes amb selecció de registres

Per poder seleccionar registres és necessari indicar la condició que han de complir els camps d'un registre per ser mostrat. Per això s'utilitza la secció **`WHERE`** de la instrucció SQL.
Els operadors relacionals que ens permeten comparar el valor dels camps són:

| Operador        | Funció                                 |
| :-------------- | :------------------------------------- |
| **=**           | Igual a                                |
| **<**           | Menor que                              |
| **>**           | Major que                              |
| **<=**          | Menor o igual                          |
| **>=**          | Major o igual                          |
| **LIKE**        | Patró que ha de complir un camp cadena |
| **BETWEEN**     | Interval de valors                     |
| **IN**          | Conjunt de valors                      |
| **IS NULL**     | És nul                                 |
| **IS NOT NULL** | No és nul                              |

Vegem uns quants exemples:

!!! Example Exemple 1
    **Buscar un registre pel valor de la seva clau**
    Mostrar tots els camps del client amb codi igual a 12.
    ```sql
    SELECT *
    FROM clientes
    WHERE CodigoCliente = 12;
    ```
    > :pushpin: Sent el camp clau, el resultat només mostrarà un registre.

!!! Example Exemple 2
    **Buscar registres pel valor d'un camp**
    Mostrar tots els camps dels clients de la **`Región`** *Madrid*.
    ```sql
    SELECT *
    FROM clientes
    WHERE Region = 'Madrid';
    ```
    >:pushpin: Al NO ser un camp clau, el resultat pot mostrar més d'un registre.

!!! Example Exemple 3
    **Buscar registres per comparació del valor d'un camp**
    Mostrar tots els camps dels clients amb Límite de Crédito major de 50000€.
    ```sql
    SELECT *
    FROM clientes
    WHERE LimiteCredito > 50000;
    ```

!!! Example Exemple 4
    **Buscar registres per comparació del valor d'un camp**
    Mostrar tots els camps dels clients amb Límite de Crédito entre 10000€ i 60000€.
    ```sql
    SELECT *
    FROM clientes
    WHERE LimiteCredito BETWEEN 10000 AND 60000;
    ```

!!! Example Exemple 5
    **Buscar registres per comparació del valor d'un camp**
    Mostrar tots els camps dels clients amb la **`Región`** nul·la.
    ```sql
    SELECT *
    FROM clientes
    WHERE Region IS NULL;
    ```

!!! Example Exemple 6
    **Exemple combinant amb l'exemple anterior**
    Mostrar els Països dels clients amb la **`Región`** nul·la, sense repetir valors.
    ```sql
    SELECT DISTINCT Pais
    FROM clientes
    WHERE Region IS NULL;
    ```

!!! Example Exemple 7
    **Exemple amb l'operador `LIKE`**
    Mostrar els Empleats el **`email`** dels quals contingui *jardin*.
    ```sql
    SELECT *
    FROM Empleados
    WHERE Email LIKE '%jardin%';
    ```
    > :pushpin: **%** és una cadena de caràcters qualsevol i **_** és un sol caràcter qualsevol.

!!! Example Exemple 8
    **Exemple amb l'operador **`IN`****
    Mostrar els productes de les gammes *Aromáticas*, *Herramientas* i *Ornamentales*.
    ```sql
    SELECT *
    FROM productos
    WHERE Gama IN ('Aromáticas', 'Herramientas', 'Ornamentales');
    ```

!!!NOTA Referències
    [MySQL – Consultes](http://mysql.conclase.net/curso/?cap=009)
    [MySQL – Operadors de comparació](http://mysql.conclase.net/curso/?cap=010a#OPE_COMPARACION)
    [MySQL Oficial – Operadors de comparació](https://dev.mysql.com/doc/refman/8.0/en/comparison-operators.html)

### Consultes amb diverses condicions

Per poder realitzar diverses condicions necessitem combinar-les amb operadors lògics, que en MySQL són:

| Operador | Funció |
| :------: | :-----: |
| **AND**  |    I    |
|  **OR**  |    O    |
| **NOT**  |   No    |

Vegem uns quants exemples:

!!! Example Exemple 1
    **Exemple amb diverses condicions**
    Mostrar codi (**`CodigoProducto`**) i **`nombre`** dels Productes que siguin de la **`Gama`** *Frutales* i **`CantidadEnSock`** sigui major que 50 unitats. Mostrem també els camps implicats en les condicions per comprovar el resultat.
    ```sql
    SELECT CodigoProducto, Nombre, Gama,
    FROM productos
    WHERE Gama = 'Frutales' AND CantidadEnStock > 50);
    ```
!!! Example Exemple 2
    **Exemple amb diverses condicions**
    Mostrar codi i nom dels Clients que siguin de la **`Ciudad`** *Madrid* o *Barcelona*. Mostrem també els camps implicats en les condicions per comprovar el resultat.
    ```sql
    SELECT CodigoCliente, NombreCliente, Ciudad
    FROM clientes
    WHERE Ciudad = 'Madrid' OR Ciudad = 'Barcelona';
    ```

!!!NOTA Referències
    [MySQL – Consultes i operadors de comparació (curs)](http://mysql.conclase.net/curso/?cap=010#OPE_LOGICOS)
    [MySQL Oficial – Operadors lògics](https://dev.mysql.com/doc/refman/8.0/en/logical-operators.html)

### Consultes amb funcions

Les funcions ens permeten realitzar transformacions de les dades per obtenir informació. Existeixen multitud de funcions que processen diferents tipus de dades.

***Algunes funcions amb cadenes de caràcters***

| Funció                     | Descripció                                                                    |
| :-------------------------- | :--------------------------------------------------------------------------- |
| **CONCAT(cad1, cad2, ...)** | Concatena cadenes                                                            |
| **UPPER(cad)**              | Passa a majúscules una cadena                                                |
| **LOWER(cad)**              | Passa a minúscules una cadena                                                |
| **LTRIM(cad)**              | Elimina de la cadena els espais inicials i finals                            |
| **LEFT(cad, X)**            | Obté els X primers caràcters de la cadena                                    |
| **RIGHT(cad, X)**           | Obté els X últims caràcters de la cadena                                     |
| **LENGTH(cad)**             | Longitud d'una cadena                                                        |
| **REPLACE(cad, ant, pos)**  | Obté una cadena prenent **cad** com a origen i canviant **ant** per **pos**  |

***Algunes funcions numèriques d'un camp***

| Funció           | Descripció                                           |
| :--------------- | :-------------------------------------------------- |
| **RAND()**        | Nombre aleatori entre 0 i 1                         |
| **POW(num, exp)** | Obté la potència de num^exp                         |
| **FLOOR(num)**    | Obté la part entera d'un nombre decimal             |
| **ROUND(num, X)** | Arrodoneix un nombre a X decimals                   |
| **SIGN(num)**     | Retorna 1 per a positiu, 0 per a 0 i -1 per a negatiu |
| **ABS(num)**      | Obté el valor absolut d'un nombre                   |

***Algunes funcions de dates d'un camp***

| Funció              | Descripció                                                                                                                                            |
| :------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------- |
| **YEAR(campo)**      | Mostra l'any del valor d'un camp de tipus data                                                                                                       |
| **MONTH(campo)**     | Mostra el mes del valor d'un camp de tipus data                                                                                                      |
| **DAY(campo)**       | Mostra el dia del valor d'un camp de tipus data                                                                                                      |
| **DAYNAME(campo)**   | Mostra el nom del dia de la setmana del camp                                                                                                         |
| **DAYOFWEEK(campo)** | Retorna un nombre indicant el dia de la setmana d'un camp data: <br>**Nota:** 1=Diumenge, 2=Dilluns, 3=Dimarts, 4=Dimecres, 5=Dijous, 6=Divendres, 7=Dissabte. |

***Algunes funcions numèriques amb diversos registres***

| Funció        | Descripció                                           |
| :------------ | :-------------------------------------------------- |
| **COUNT(*)**   | Compta els registres seleccionats                   |
| **MIN(campo)** | Valor mínim del camp dels registres seleccionats    |
| **MAX(campo)** | Valor màxim del camp dels registres seleccionats    |
| **SUM(campo)** | Suma dels valors del camp                           |
| **AVG(campo)** | Mitjana dels valors del camp                        |

!!! Nota Referències
    [MySQL w3schools – Funcions de MySQL](https://www.w3schools.com/sql/sql_ref_mysql.asp)
    [MySQL Oficial – Funcions](https://dev.mysql.com/doc/refman/8.0/en/functions.htm)

***Funcions per a camps calculats i per a condicions***

!!! Example Exemple 1
    **Exemple amb funcions**
    Mostrar codi (**`CodigoProducto`**), **`nombre`** i ***`preu de venda al públic`*** dels productes, però aquest preu ha de ser amb IVA inclòs, és a dir, afegir-li al **`PrecioVenta`** el 21% multiplicant-lo per 1.21. Assignar l'àlies **`pvp`** al nou camp calculat.
    ```sql
    SELECT CodigoProducto, Nombre, PrecioVenta, ROUND(PrecioVenta * 1.21, 2) AS pvp
    FROM Productos;
    ```

!!! Example Exemple 2
    **Exemple amb funcions en les condicions**
    Seleccionar en la consulta anterior els productes que tinguin el nou camp **`pvp`** superior a 100€.
    ```sql
    SELECT CodigoProducto, Nombre, PrecioVenta, ROUND(PrecioVenta * 1.21, 2) AS pvp
    FROM Productos
    WHERE ROUND(PrecioVenta * 1.21, 2) > 100;
    ```
***Funcions per al maneig de cadenes de caràcters***

!!! Example Exemple 3
    **Exemple amb funcions**
    Seleccionar dels empleats el nom complet (**`NombreCompleto`**) concatenant el nom i els dos cognoms.
    ```sql
    SELECT CONCAT(Nombre, ' ', Apellido1, ' ', Apellido2) AS NombreCompleto
    FROM empleados;
    ```

!!! Example Exemple 4
    **Exemple amb funcions**
    Obtenir la inicial del **`nombre`** de tots els empleats.
    ```sql
    SELECT LEFT(Nombre, 1) AS inicial 
    FROM empleados;
    ```

!!! Example Exemple 5
    **Exemple amb funcions**
    Obtenir el **`nombre`** dels empleats tot en majúscules.
    ```sql
    SELECT UCASE(Nombre)
    FROM empleados;
    ```

!!! Example Exemple 6
    **Exemple amb funcions**
    Obtenir el codi de les oficines (**`CodigoOficina`**) però substituint el guió per la barra, és a dir, el '-' per '/'.
    ```sql
    SELECT REPLACE(CodigoOficina, '-', '/')
    FROM oficinas;
    ```

!!! Example Exemple 7
    **Exemple amb funcions**
    Obtenir la primera paraula del nom del **`proveedor`** de tots els productes.
    ```sql
    SELECT SUBSTRING_INDEX(Proveedor, ' ', 1)
    FROM productos;
    ```

!!! Example Exemple 8
    **Exemple amb funcions**
    Obtenir el codi dels productes però amb l'ordre dels caràcters invertit.
    ```sql
    SELECT CodigoProducto, REVERSE(CodigoProducto)
    FROM productos;
    ```

!!! Example Exemple 9
    **Exemple amb funcions**
    Obtenir un gràfic de barres per a l'estoc de cada producte. Utilitzarem el caràcter '|' per cada unitat d'estoc.
    ```sql
    SELECT Codigoproducto, REPEAT('|', CantidadEnStock)
    FROM productos;
    ```

!!! Example Exemple 10
    **Exemple amb funcions**
    Obtenir el correu electrònic de cada empleat tenint en compte que l'usuari és el seu nom en minúscules i el domini *@iesdoctorbalmis.com*.
    ```sql
    SELECT CONCAT(LCASE(nombre), '@iesdoctorbalmis.com') AS email
    FROM empleados;
    ```

!!! Example Exemple 11
    **Exemple amb funcions**
    Obtenir l'abreviatura del nom i primer cognom dels empleats concatenant la inicial del nom i la inicial del cognom1.
    ```sql
    SELECT CONCAT(LEFT(Nombre, 1), LEFT(Apellido1, 1)) AS inicial
    FROM empleados;
    ```

***Funcions numèriques***

!!! Example Exemple 12
    **Exemple amb funcions**
    Obtenir un nombre aleatori entre 0 i 9.
    ```sql
    SELECT FLOOR(10 * RAND());
    ```

***Funcions de dates***

!!! Example Exemple 13
    **Exemple amb funcions**
    Mostrar en camps diferents l'any, el mes i el dia de la data de les comandes.
    ```sql
    SELECT YEAR(FechaPedido), MONTH(FechaPedido), DAY(FechaPedido)
    FROM pedidos;
    ```

!!! Example Exemple 14
    **Exemple amb funcions**
    Mostrar tots els camps de comandes realitzades al gener del 2009.
    ```sql
    SELECT *
    FROM pedidos
    WHERE YEAR(FechaPedido) = 2009 AND MONTH(FechaPedido) = 1;
    ```

!!! Example Exemple 15
    **Exemple amb funcions**
    Mostrar tots els camps de comandes realitzades en dilluns.
    ```sql
    SELECT *
    FROM pedidos
    WHERE DAYOFWEEK(FechaEntrega) = 2;
    ```

***Funcions numèriques amb diversos registres***

!!! Example Exemple 16
    **Exemple amb funcions**
    Mostrar el nombre total de pagaments que s'han rebut.
    ```sql
    SELECT COUNT(*)
    FROM pagos;
    ```

!!! Example Exemple 17
    **Exemple amb funcions**
    Mostrar la quantitat en euros total que ens han ingressat.
    ```sql
    SELECT SUM(Cantidad)
    FROM pagos;
    ```

!!! Example Exemple 18
    **Exemple amb funcions**
    Mostrar l'import del pagament més petit i l'import de pagament més gran rebut.
    ```sql
    SELECT MIN(Cantidad), MAX(Cantidad)
    FROM pagos;
    ```

## Consultes **`SELECT`** d'AGRUPACIÓ

### Consultes d'agrupació

Quan necessitem agrupar diversos registres per realitzar operacions per sumar, comptar, o calcular la mitjana, el mínim o el màxim, necessitarem realitzar una instrucció **`SELECT`** indicant quins registres agrupem, és a dir, quins camps mostrem dels registres comuns i sobre quins camps realitzem l'operació.
La sintaxi per realitzar una consulta agrupada a una taula és:

```sql
SELECT [DISTINCT] camps
[FROM table_references]
[WHERE where_definition]
[GROUP BY {col_name | expr | position} [ASC | DESC], ... ]
[HAVING where_definition]
[ORDER BY {col_name | expr | position} [ASC | DESC] , ...]
[LIMIT [offset,] row_count]
```

<div class="caso_estudio">

:hand: **Important**
L'ordre de les clausules, **obligatòriament**, és:
**SELECT … FROM … WHERE … GROUP BY … HAVING … ORDER … LIMIT**
</div> <!-- fi cas d'estudi -->


### Exemples de les primeres consultes agrupades
Amb XAMPP-MySQL importar la **BD de jardineria** que facilitarà el professor.

!!! Example Exemple 1
    **Comptar**
    Mostrar el nombre de clients (anomenar el nou camp **`NumClientes`**) que tenim a cada **`ciudad`**.
    ```sql
    SELECT Ciudad, COUNT(*) AS NumClientes
    FROM clientes
    GROUP BY Ciudad;
    ```

!!! Example Exemple 2
    **Sumar operacions numèriques**
    Mostrar la comanda i l'import total (**`ImpTotal`**) de totes les línies de cada comanda. A la taula detallepedidos tenim el **`CodigoPedido`**, la **`Cantidad`** i el **`PrecioUnidad`** de cada article de la comanda. Haurem de sumar **`Cantidad * PrecioUnidad`** de cada línia i després sumar-les totes.

    ```sql
    -- Sense Agrupar
    SELECT CodigoPedido, Cantidad * PrecioUnidad AS ImpLinea
    FROM detallepedidos;

    -- Agrupant per Comanda
    SELECT CodigoPedido, SUM(Cantidad * PrecioUnidad) AS ImpTotal
    FROM detallepedidos
    GROUP BY CodigoPedido;
    ```

Podem afegir també filtres que hagin de complir els registres mitjançant condicions a la clàusula **`WHERE`**.

!!! Example Exemple 3
    **Agrupacions amb condicions `WHERE`**
    Mostrar el nombre d'articles (**`NumArticulos`**) de cada Gama els **`PrecioVenta`** dels quals sigui superior a 20€.

    ```sql
    SELECT Gama, COUNT(*) AS NumArticulos
    FROM productos
    WHERE PrecioVenta > 20
    GROUP BY Gama;
    ```

Però pot ser que la condició a complir sigui sobre els camps calculats. En aquests casos, la condició anirà a la clàusula ****`HAVING`****.

!!! Example Exemple 4
    **Agrupacions amb condicions `HAVING`**
    Mostrar les gammes (**`Gama`**) d'articles que tinguin més de 100 diferents. Mostrar també el nombre d'articles (**`NumArticulos`**).
    ```sql
    SELECT Gama, COUNT(*) AS NumArticulos
    FROM productos
    GROUP BY Gama
    HAVING COUNT(*) > 100;
    ```

Fins i tot podem tenir consultes que combinin **`WHERE`** i **`HAVING`** simultàniament.

!!! Example Exemple 5
    **Agrupacions amb condicions **`WHERE`** i **`HAVING`****
    Mostrar els clients que hagin realitzat més d'1 pagament de quantitat superior als 3000€.
    ```sql
    SELECT CodigoCLiente, COUNT(*) AS NumPagos
    FROM pagos
    WHERE Cantidad > 3000
    GROUP BY CodigoCliente
    HAVING COUNT(*) > 1;
    ```

## Consultes SELECT multitaula
### Consultes multitaula de creuament (relació 1-N)
Les consultes multitaula ens permetran processar informació de diverses taules conjuntament. La sintaxi és la mateixa que hem vist anteriorment per a la sintaxi **`SELECT`**, però a la clàusula **`FROM`** tindrem **diverses taules**.

!!! Example Exemple 1
    **SELECT multitaula**
    Mostrar els valors de la taula **pagos** afegint el camp **`NombreCliente`**.
    ```sql
    SELECT clientes.NombreCliente, pagos.*
    FROM pagos, clientes
    WHERE pagos.CodigoCliente = clientes.CodigoCliente;
    ```

    * Quants registres té la taula pagos?  
    * Quants registres té la taula clientes?  
    * Quants registres retorna la consulta anterior?  
    * Si s'elimina la condició **`WHERE`**, quants registres obtenim? Justifica-ho.

!!! Example Exemple 2
    **SELECT multitaula**
    Mostrar els valors de la taula **pedidos** afegint el camp **`NombreCliente`**.
    ```sql
    SELECT clientes.NombreCliente, pedidos.*
    FROM pedidos, clientes
    WHERE pedidos.CodigoCliente = clientes.CodigoCliente;
    ```

    * Quants registres té la taula pedidos?  
    * Quants registres té la taula clientes?  
    * Quants registres retorna la consulta anterior?  
    * Si s'elimina la condició **`WHERE`**, quants registres obtenim? Justifica-ho.

!!! Example Exemple 3
    **SELECT multitaula**
    Mostrar els valors de la taula **empleados** afegint els camps **`Ciudad`**,  **`Pais`** i l'oficina del país **`CodigoOficina`**.
    ```sql
    SELECT oficinas.Ciudad, oficinas.Pais, empleados.*
    FROM oficinas, empleados
    WHERE oficinas.CodigoOficina = empleados.CodigoOficina;
    ```

    * Quants registres té la taula oficinas?
    * Quants registres té la taula empleados?  
    * Quants registres retorna la consulta anterior?  
    * Si s'elimina la condició **`WHERE`**, quants registres obtenim? Justifica-ho.

En relacions reflexives, hem de creuar una taula amb ella mateixa. Per poder fer això, hem d'assignar un àlies a cada taula per diferenciar-les.
Vegem un exemple.

!!! Example Exemple 4
    **SELECT multitaula amb relació reflexiva**
    Mostrar els valors de la taula **empleados** afegint els camps **`Nombre`** i **`Apellido1`** del seu cap.
    ```sql
    SELECT jefes.Nombre, jefes.Apellido1, trabajadores.*
    FROM empleados as trabajadores, empleados as jefes
    WHERE trabajadores.CodigoJefe = jefes.CodigoEmpleado;
    ```

    * Quants registres té la taula empleados?  
    * Quants registres retorna la consulta anterior?  
    * Si s'elimina la condició **`WHERE`**, quants registres obtenim? Justifica-ho.

!!! Example Exemple 5
    **SELECT multitaula amb diverses taules**
    Mostrar els següents valors:
    * De **pedidos**: **`CodigoPedido`** i **`FechaPedido`**
    * De **detallepedidos**: **`CodigoProducto`** i **`Cantidad`**
    * De **productos**: **`Nombre`** i **`Gama`**
    ```sql
    SELECT pedidos.CodigoPedido, pedidos.FechaPedido, detallepedidos.CodigoProducto, detallepedidos.Cantidad, productos.Nombre, productos.Gama
    FROM pedidos, detallepedidos, productos
    WHERE pedidos.CodigoPedido = detallepedidos.CodigoPedido AND detallepedidos.CodigoProducto = productos.CodigoProducto;
    ```

    * Quants registres té la taula empleados?
    * Quants registres retorna la consulta anterior?
    * Si s'elimina la condició **`WHERE`**, quants registres obtenim? Justifica-ho.

Es pot observar que quan es mostren dades de la taula pare de relacions amb cardinalitat 1-N, aquestes es repeteixen ja que poden tenir molts fills. A l'exemple anterior es pot veure clarament que les dades de la taula pedidos es repeteixen tantes vegades com línies de detall tinguin.

### Consultes multitaula mitjançant INTERSECCIÓ

Les consultes d'intersecció es poden executar amb la clàusula:

```sql
taula1 INNER JOIN taula2 ON condició
```

L'exemple anterior quedaria:

!!! Example Exemple 1
    **SELECT multitaula amb `INNER JOIN`**
    Mostrar els valors de la taula **pagos** afegint el camp **`NombreCliente`**.
    ```sql
    SELECT clientes.NombreCliente, pagos.*
    FROM (pagos INNER JOIN clientes ON pagos.CodigoCliente = clientes.CodigoCliente);
    ```
Vegem un altre exemple de consulta multitaula:

!!! Example Exemple 2
    **SELECT multitaula amb `INNER JOIN`**
    Mostrar els valors de la taula **detallepedidos** però afegint els camps **`FechaPedido`** i **`Estado`** de la taula pedidos. Comprova que el resultat conté el mateix nombre de registres que detallepedidos.
    ```sql
    SELECT pedidos.FechaPedido, pedidos.Estado, detallepedidos.*
    FROM (detallepedidos INNER JOIN pedidos ON detallepedidos.CodigoPedido = pedidos.CodigoPedido);
    ```

A les consultes multitaula mitjançant intersecció, **és possible que hi hagi registres que no es mostrin per no haver-hi creuament entre ells**. Per exemple, si volem saber el nombre de comandes realitzades per cada client, podríem pensar a realitzar la següent consulta:

!!! Example Exemple 3
    **SELECT multitaula amb `INNER JOIN`**
    Mostrar el nombre de comandes realitzades per cada client.
    ```sql
    SELECT clientes.CodigoCliente, clientes.NombreCliente, COUNT(pedidos.CodigoPedido)
    FROM clientes INNER JOIN pedidos ON clientes.CodigoCliente = pedidos.CodigoCliente
    GROUP BY clientes.CodigoCliente, clientes.NombreCliente;
    ```

A la taula de clients hi ha 36 registres, però el resultat de la consulta només mostra 19. Això passa perquè hi ha clients que no han realitzat encara cap comanda.

Per evitar això, s'introdueix un canvi a la consulta que permet incloure tots els registres d'una taula de la intersecció, que en el nostre cas és clientes. Ho farem amb la clàusula **`LEFT`** de la relació **clientes-pedidos**:

!!! Example Exemple 4
    **SELECT multitaula amb `LEFT JOIN`**
    Mostrar el nombre de comandes realitzades per cada client, incloent aquells que no n'han realitzat cap.
    ```sql
    SELECT clientes.CodigoCliente, clientes.NombreCliente, COUNT(pedidos.CodigoPedido)
    FROM clientes LEFT JOIN pedidos ON clientes.CodigoCliente = pedidos.CodigoCliente
    GROUP BY clientes.CodigoCliente, clientes.NombreCliente;
    ```

Vegem un altre exemple:

!!! Example Exemple 5
    **SELECT multitaula amb `LEFT JOIN`**
    Mostrar el **`CodigoProducto`** i nom de la taula **productos** juntament amb la suma de la quantitat (**`SumCantidad`**) demanada a totes les comandes existents.
    > :pushpin: Tingueu en compte que dels productes que no hi hagi hagut cap comanda ha d'aparèixer 0.
    ```sql
    SELECT productos.CodigoProducto, productos.Nombre, SUM(detallepedidos.Cantidad) AS SumCantidad
    FROM productos LEFT JOIN detallepedidos ON productos.CodigoProducto = detallepedidos.CodigoProducto
    GROUP BY productos.CodigoProducto, productos.Nombre;
    ```

La clàusula **`RIGHT`** permetria que es mostressin els de la segona taula en la relació, en comptes de **`LEFT`** que mostra la primera.

### Consultes multitaula mitjançant `UNION`

De vegades necessitem unir el resultat de dues consultes. Per això el resultat ha de mostrar els mateixos camps i amb els mateixos tipus de dades.
Per exemple, tenim d'una banda els clients que han realitzat pagaments posteriors al 31/01/2009:

```sql
SELECT DISTINCT CodigoCliente
FROM pagos
WHERE FechaPago > '2009-01-31';
```

D'altra banda, tenim els clients que han realitzat comandes posteriors al 31/03/2009:

```sql
SELECT DISTINCT CodigoCliente
FROM pedidos
WHERE FechaPedido > '2009-03-31';
```

Els clients que estan a les dues consultes són 16, 23 i 27, però hi ha clients que han realitzat pagaments i no comandes, i viceversa.

!!! Example Exemple 1
    **SELECT multitaula amb `UNION`**
    Mostrar el **`CodigoCliente`** dels clients que han realitzat pagaments posteriors al 31/03/2009 o comandes posteriors al 31/03/2009.
    ```sql
    SELECT DISTINCT CodigoCliente
    FROM pagos
    WHERE FechaPago > '2009-01-31'
    UNION
    SELECT DISTINCT CodigoCliente
    FROM pedidos
    WHERE FechaPedido > '2009-03-31';
    ```

### Consultes multitaula mitjançant SUBCONSULTES

<div class="caso_estudio">

Una **subconsulta** és una instrucció **`SELECT`** que s'utilitza dins d'una altra instrucció **`SELECT`**.
</div> <!-- fi cas d'estudi -->

<div class="caso_estudio">

Una **subconsulta** serà **correlacionada** si apareixen dades de la consulta, és a dir, si no es pot executar de forma independent.
</div> <!-- fi cas d'estudi -->

Hi ha diverses situacions on utilitzarem subconsultes. A continuació veurem alguns exemples.

#### Subconsultes en **`WHERE`** amb operador de comparació

Per al següent exemple, primer busquem la quantitat mitjana que paguen els clients.

```sql
SELECT AVG(Cantidad) AS CantidadMedia 
FROM pagos;
```

!!! Example Exemple 1
    **Subconsulta en **`WHERE`** amb operador de comparació**
    Mostrar els registres de pagaments que tinguin quantitats superiors a la mitjana.
    ```sql
    SELECT *
    FROM pagos
    WHERE Cantidad > (SELECT AVG(Cantidad) AS CantidadMedia FROM pagos);
    ```

#### Subconsultes en `WHERE` amb operador `IN`

L'operador **`IN`** retorna cert si el valor del camp d'un registre està al conjunt de valors retornat per la subconsulta.

!!! Example Exemple 2
    **Subconsulta en `WHERE` amb operador `IN`**
    Mostrar la **`Gama`** dels productes dels quals s'hagi demanat més de 30 unitats.
    ```sql
    SELECT DISTINCT Gama
    FROM productos
    WHERE CodigoProducto IN (SELECT CodigoProducto FROM detallepedidos WHERE Cantidad > 30);
    ```

També es pot utilitzar de forma negativa amb **`NOT IN`**

#### Subconsultes en `WHERE` amb operador `EXISTS`

L'operador **`EXISTS`** és cert si la subconsulta retorna almenys un registre.

!!! Example Exemple 3
    **Subconsulta en `WHERE` amb operador `EXISTS`**
    Obtenir les dades dels empleats que tinguin algun client assignat.
    ```sql
    SELECT *
    FROM empleados
    WHERE EXISTS (SELECT * FROM Clientes WHERE CodigoEmpleadoRepVentas = empleados.CodigoEmpleado);
    ```

També es pot utilitzar de forma negativa amb **NOT EXISTS**

#### Subconsultes en `FROM`

Podem utilitzar subconsultes com a taules i col·locar-les a la clàusula **`FROM`**.

!!! Example Exemple 4
    **Subconsulta en `FROM`**
    Encara que la següent consulta es pot obtenir mitjançant una consulta d'intersecció típica, utilitzarem una subconsulta per provar el seu funcionament a **`FROM`** de forma senzilla. Les subconsultes de **`FROM`** han de tenir un àlies que assignarem amb **`AS`**.
    Mostrar les dades dels empleats que treballin a oficines de Madrid.
    ```sql
    SELECT empleados.*
    FROM empleados, (SELECT * FROM oficinas WHERE Ciudad = 'Madrid') AS OficinasMadrid
    WHERE empleados.CodigoOficina = OficinasMadrid.CodigoOficina;
    ```
    La consulta anterior sense utilitzar subconsulta seria:
    ```sql
    SELECT empleados.*
    FROM empleados, oficinas
    WHERE empleados.CodigoOficina = oficinas.CodigoOficina AND oficinas.Ciudad = 'Madrid';
    ```
