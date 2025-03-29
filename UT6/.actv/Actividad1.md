---
title:
    Apuntes
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

[TOC]

## Scripts con variables

Realizad los siguientes scripts en SQL utilizando variables para almacenar el resultado de las subconsultas. Utilizaremos la base de datos de la NBA en todos los ejercicios.

!!! Example Ejemplo
    Jugador (o jugadores) de mayor peso de la NBA (**`Nombre`**).

    ```sql
    SELECT Nombre
    FROM jugadores
    WHERE Peso = (SELECT MAX(Peso) FROM jugadores);
    ```
    
    ***Solución***
  
    ```sql
    -- Seleccionar base de datos de la NBA
    USE nba;

    -- Cargar el peso máximo en la variable @maxpeso
    SELECT MAX(Peso) INTO @maxpeso FROM jugadores;
    
    -- Mostrar el nombre del jugador más pesado de la NBA 
    SELECT Nombre FROM jugadores WHERE Peso = @maxpeso;
    ```

<div class="ejercicio">

### :white_check_mark: Ejercicio 1

Jugadores que pesan más que todos los jugadores españoles (**`Nombre`**, **`Peso`**).

```sql
SELECT nombre, peso
FROM jugadores
WHERE peso > 
(SELECT MAX(peso) FROM jugadores WHERE procedencia = 'Spain');
```

</div> <!-- fin ejercicio -->

<div class="ejercicio">

### :white_check_mark: Ejercicio 2

Nombre de la ciudad de procedencia del máximo reboteador de la historia de la NBA (**`Procedencia`**).

```sql
SELECT Procedencia
FROM jugadores
WHERE codigo = 
(SELECT jugador FROM estadisticas
ORDER BY Rebotes_por_partido DESC
LIMIT 1);
```

</div> <!-- fin ejercicio -->

<div class="ejercicio">

### :white_check_mark: Ejercicio 3

Jugador (o jugadores) que miden igual o más que el jugador más alto de la conferencia Este (*East*). Mostrar la altura en cm (**`Nombre`**, **`Altura`**).

```sql
SELECT Nombre, 2.54 * heightToInch(Altura) AS Altura
FROM jugadores
WHERE heightToInch(Altura) >=  
(SELECT MAX(heightToInch(j.Altura)) 
FROM jugadores j, equipos e
WHERE j.Nombre_equipo = e.Nombre AND e.Conferencia = 'East');
```

> :pushpin: La función **highToInch** está incluída en la base de datos de la NBA.

</div> <!-- fin ejercicio -->

<div class="ejercicio">

### :white_check_mark: Ejercicio 4

Jugadores que han conseguido más puntos en la temporada *02/03* que los que consiguió Lebron James en la temporada *04/05* (**`Nombre`**, **`Puntos_por_partido`**).

```sql
SELECT j.Nombre, st.Puntos_por_partido 
FROM jugadores j, estadisticas st
WHERE j.codigo = st.jugador AND st.temporada = '02/03' AND 
st.Puntos_por_partido >  
(SELECT Puntos_por_partido FROM estadisticas WHERE 
jugador = (SELECT codigo FROM jugadores WHERE Nombre = 'Lebron James')
AND temporada = '04/05');
```



</div> <!-- fin ejercicio -->

<div class="ejercicio">

### :white_check_mark: Ejercicio 5

Jugadores que en la temporada *03/04* anotaron más puntos *Jose  Calderon* en en la temporada *05/06* y cogieron más rebotes que el mayor número de rebotes que cogió *Yao Ming* en una temporada (**`Nombre`**, **`Puntos_por_partido`**, **`Rebotes_por_partido`**).

```sql
SELECT j.Nombre, st.Puntos_por_partido, st.Rebotes_por_partido
FROM jugadores j, estadisticas st
WHERE j.codigo = st.jugador AND st.temporada = '02/03' AND
st.Puntos_por_partido > 
(SELECT Puntos_por_partido FROM estadisticas WHERE
jugador = (SELECT codigo FROM jugadores WHERE Nombre = 'Jose Calderon')
AND temporada = '05/06'
)
AND st.Rebotes_por_partido >
(SELECT MAX(Rebotes_por_partido) FROM estadisticas WHERE 
jugador = (SELECT codigo FROM jugadores WHERE Nombre = 'Yao Ming')
);
```

</div> <!-- fin ejercicio -->
