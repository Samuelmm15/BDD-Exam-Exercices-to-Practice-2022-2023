# Exercice 3 Álgebra relacional

Sean los esquemas de relación siguientes: 
HOMBRE( nombre-hombre, edad )
CLAVE: (nombre-hombre) 

MUJER( nombre-mujer, edad )
CLAVE: (nombre-mujer)

HSIM( nombre-hombre, nombre-mujer )
CLAVE: (nombre-hombre, nombre-mujer)
SIGNIFICADO: El hombre nombre-hombre cae simpático a la mujer nombre-mujer.

MSIM( nombre-hombre, nombre-mujer)
CLAVE: (nombre-hombre, nombre-mujer)
SIGNIFICADO: La mujer nombre-mujer cae simpática al hombre nombre-hombre.

MATRIMONIO( nombre-hombre, nombre-mujer ) 
CLAVE: (nombre-hombre)

Escribir las siguientes consultas en álgebra relacional:\
a) Hallar las parejas de hombres y mujeres que se caen mutuamente simpáticos.\
Se necesitan las tablas: HSIM, MSIN
```
p(nombre-hombre, nombre-mujer)(HSIM ∩ MSIM)
```

Solución del solucionario:
```
HSIM ∩ MSIM
```

b) Hallar las parejas casadas cuyos componentes se caen mutuamente simpáticos.\
Se necesitan las tablas: HSIM, MSIM, MATRIMONIO
```
V1 = p(nombre-hombre, nombre-mujer)(HSIM ∩ MSIM)
p(nombre-hombre, nombre-mujer)(MATRIMONIO ∩ V1)
```

Solución del solucionario:
```
HSIM ∩ MSIM ∩ MATRIMONIO
```

c) Hallar las mujeres casada a quienes no cae simpático su marido.\
Se necesitan las tablas: HSIM, MATRIMONIO
```
p(nombre-mujer)(MATRIMONIO - HSIM)
```

d) Hallar los hombres misóginos a quienes no cae simpática ninguna mujer.\
Se necesitan las tablas: HOMBRE, MSIM
```
p(nombre-hombre)(HOMBRE - MSIM)
```

Solución del solucionario:
```
p(nombre-hombre)(HOMBRE) - p(nombre-hombre)(MSIM)
```
**`NOTA`**: Cabe destacar que creo que la respuesta que yo tengo mal formulada, ya que se debe a que la tabla de
hombre no tiene exactamente los mismos atributos que la tabla de MSIM, por tanto, no se puede aplicar la operación
de diferencia de manera directa, es por ello, que para poder aplicar dicho operador debemos de generar primero
las tablas que tengan los mismo atributos mediante el operador de proyección.

e) Hallar los hombres y mujeres asociales a quienes no cae nadie simpático.\
Se necesitan las tablas: HOMBRE, MSIM, MUJER, HSIM
```
p(nombre-hombre, nombre-mujer)((HOMBRE - MSIM) ∪ (MUJER - HSIM))
```

Solución del solucionario:
```
p(nombre-hombre)(HOMBRE) - p(nombre-hombre)(MSIM) ∪ p(nombre-mujer)(MUJER) - p(nombre-mujer)(HSIM)
```
**`NOTA`**: cabe destacar lo mismo que ocurre para el caso anterior, por tanto, se debe de implementar la consulta
de dicha manera la cual queda representada.

f) Hallar las mujeres casadas que caen simpáticas a algún hombre.\
Se necesitan las tablas: MATRIMONIO, MSIM
```
p(nombre-mujer)(MSIM - MATRIMONIO)
```

Solución del solucionario:
```
p(nombre-mujer)(MSIM) ∩ p(nombre-mujer)(MATRIMONIO)
```

g) Hallar los hombres a quienes sólo caen simpáticas mujeres casadas.\
Se necesitan las tablas: MSIM, MATRIMONIO
```
p(nombre-hombre)(MSIM) - p(nombre-hombre)((MSIM - (MSIM * p(nombre-hombre)(MATRIMONIO)))
```

Esto anterior es la solución del solucionario, que de primera instancia lo tenía de manera similar a la solución
real, exceptuando algún aspecto relacionado con el operador de yunción natural.

h) Hombres a quienes sólo cae simpática su esposa.\
Se necesitan las tablas: MSIM, MATRIMONIO
```
p(nombre-hombre)(MSIM / (MSIM - (MSIM - MATRIMONIO)))
```
