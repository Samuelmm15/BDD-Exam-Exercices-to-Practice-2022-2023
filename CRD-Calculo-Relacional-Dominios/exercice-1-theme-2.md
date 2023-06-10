# Exercice 1 CRD

Sean los esquemas de relación siguientes: 

HOMBRE( nombre-hombre, edad )\
CLAVE: (nombre-hombre) 

MUJER( nombre-mujer, edad )\
CLAVE: (nombre-mujer)

HSIM( nombre-hombre, nombre-mujer )\
CLAVE: (nombre-hombre, nombre-mujer)\
SIGNIFICADO: El hombre nombre-hombre cae simpático a la mujer nombre-mujer.

MSIM( nombre-hombre, nombre-mujer)\
CLAVE: (nombre-hombre, nombre-mujer)\
SIGNIFICADO: La mujer nombre-mujer cae simpática al hombre nombre-hombre.

MATRIMONIO( nombre-hombre, nombre-mujer )\
CLAVE: (nombre-hombre)

Escribir las siguientes consultas en cálculo relacional de dominio:

a) Hallar las parejas de hombres y mujeres que se caen mutuamente simpáticos.\
Se necesitan las tablas: HSIM, MSIM
```sql
{<nombre-hombre, nombre-mujer> | ∃(nombre-hombre`, nombre-mujer`)((<nombre-hombre, nombre-mujer> ϵ HSIM) ^
    (<nombre-hombre`, nombre-mujer`> ϵ MSIM) ^ (nombre-hombre = nombre-hombre`) ^ (nombre-mujer = nombre-mujer`)}
```

b) Hallar las parejas casadas cuyos componentes se caen mutuamente simpáticos.\
Se necesitan las tablas: MATRIMONIO, HSIM, MSIM
```sql
{<nombre-hombre, nombre-mujer> | ∃(nombre-hombre`, nombre-mujer`)((<nombre-hombre, nombre-mujer> ϵ MATRIMONIO)
    ^ (<nombre-hombre`, nombre-mujer`> ϵ HSIM) ^ (<nombre-hombre`, nombre-mujer`> ϵ MSIM) ^ 
    (nombre-hombre = nombre-hombre`) ^ (nombre-mujer = nombre-mujer`)}
```

c) Hallar las mujeres casada a quienes no cae simpático su marido.\
Se necesitan las tablas: MATRIMONIO, HSIM
```sql
{<nombre-mujer> | ∃(nombre-hombre)((<nombre-hombre, nombre-mujer> ϵ MATRIMONIO) ^
    ¬∃(nombre-hombre`)(<nombre-hombre`, nombre-mujer> ϵ HSIM) ^ (nombre-hombre = nombre-hombre`)}
```

d) Hallar los hombres misóginos a quienes no cae simpática ninguna mujer.\
Se necesitan las tablas: HOMBRES, MSIM
```sql
{<nombre-hombre> | ∃(edad)((<nombre-hombre, edad> ϵ HOMBRE) ^ 
    ∀(nombre-mujer)¬∃(nombre-hombre`)((<nombre-hombre`, nombre-mujer> ϵ MSIM) ^ (nombre-hombre` = nombre-hombre))}
```

e) Hallar los hombres y mujeres asociales a quienes no cae nadie simpático.

f) Hallar las mujeres casadas que caen simpáticas a algún hombre.

g) Hallar los hombres a quienes sólo caen simpáticas mujeres casadas.

h) Hombres a quienes sólo cae simpática su esposa.