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

## Título 1

Aquí viene texto y a continuación el diagrama de actividad 1.

```puml {align="center", style="zoom:1"}
@startuml
start
:Instrucción 1;
:Instrucción 2;
:Instrucción 3;
end
@enduml
```

Aquí viene más texto y a continuación el diagrama de actividad 2.

```puml {align="center", style="zoom:1"}
@startuml
start
if (condición) then (Sí)
#lightgreen:bloque sentencias 1;
note
Este bloque de sentencias sólo se
ejecuta **SI** se cumple la condición.
end note
endif
:bloque sentencias común;
note right
Este bloque de sentencias se ejecuta
**siempre**, se cumpla o no la condición.
end note
stop
@enduml
```

Aquí viene más texto y a continuación el diagrama de actividad 3.

```puml {align="center", style="zoom:1"}
@startuml
start
if (condición) then (Sí)
#lightgreen:bloque sentencias 1;
note
Este bloque de sentencias sólo se
ejecuta **SI** se cumple la condición.
end note
else (No)
#lightgreen:bloque sentencias 2;
note right
Este bloque de sentencias sólo se
ejecuta si **NO** se cumple la condición.
end note
end if
:bloque sentencias común;
note
Este bloque de sentencias se ejecuta
**siempre**, se cumpla o no la condición.
end note
stop
@enduml
```

Aquí viene más texto y a continuación el diagrama de actividad 4.

```puml {align="center", style="zoom:1"}
@startuml
!pragma useVerticalIf on
start
if (condición A) then (Sí)
#lightgreen: bloque sentencias 1;
note right
Las sentencias sólo se ejecutan **SI**
se cumple la condición A.
end note
(No) elseif (condition B) then (Sí)
#lightgreen:bloque sentencias 2;
note right
Las sentencias sólo se ejecutan si **NO** se
cumple la condición A y **SÍ** se cumple la B.
end note
else (No)
#lightgreen:bloque sentencias 3;
note right
Las sentencias sólo se ejecutan si **NO** 
se cumple ninguna condición.
end note
end if
:bloque sentencias común;
note
Este bloque de sentencias se ejecuta
**siempre**, se cumpla o no la condición.
end note
stop
@enduml
```
