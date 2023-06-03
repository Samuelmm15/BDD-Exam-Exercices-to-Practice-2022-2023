# Exercice 3 CRT

`NOTA:` HAY QUE TENER CUIDADO CON LOS PARÉNTESIS DE LAS CONSULTAS EXPRESADAS A CONTINUACIÓN, YA QUE CADA UNO
DE LOS CUANTIFICADORES QUE SE PUEDEN OBSERVAR DEBEN DE ESTAR DELIMITADOS POR (), Y, CADA VEZ QUE SE HAGA
USO DE UN NUEVO CUANTIFICADOR DISTINTO PARA LA CONSULTA, SE DEBE DE CERRAR EL PARÉNTESIS DEL CUANTIFICADOR
ANTERIOR, NO COMO SE PUEDE OBSERVAR A LO LARGO DEL DOCUMENTO.

Sean los esquemas de relación siguientes: \
HOMBRE( nombre-hombre, edad )\
CLAVE: (nombre-hombre) 

MUJER( nombre-mujer, edad ) \
CLAVE: (nombre-mujer)

HSIM( nombre-hombre, nombre-mujer )\
CLAVE: (nombre-hombre, nombre-mujer)\
SIGNIFICADO: El hombre nombre-hombre cae simpático a la mujer nombre-mujer.

MSIM( nombre-hombre, nombre-mujer)\
CLAVE: (nombre-hombre, nombre-mujer)\
SIGNIFICADO: La mujer nombre-mujer cae simpática al hombre nombre-hombre.

MATRIMONIO( nombre-hombre, nombre-mujer )\ 
CLAVE: (nombre-hombre)

Escribir las siguientes consultas en cálculo relacional de tuplas:
a) Hallar las parejas de hombres y mujeres que se caen mutuamente simpáticos.\
Se necesitan las tablas: HSIM, MSIM
```
Para (dom(h) = HSIM), (dom(m) = MSIM)
{t | ∃h ∃m((h.nombre-hombre = m.nombre-hombre) ^ (h.nombre-mujer = m.nombre-mujer) ^
(h.nombre-hombre = t) ^ (h.nombre-mujer = t))}
```

Consulta anterior corregida por el solucionario del libro:
```
{t | HSIM(t) ^ MSIM(t)}
```

b) Hallar las parejas casadas cuyos componentes se caen mutuamente simpáticos.
Se necesitan las tablas: HSIM, MSIM, MATRIMONIO
```
{t | HSIM(t) ^ MSIM(t) ^ MATRIMONIO(t)}
```

c) Hallar las mujeres casadas a quienes no cae simpático su marido.
Se neceistan las tablas: HSIM, MATRIMONIO
```
{t | MATRIMONIO(t) ^ ¬HSIM(t)}
```

Consulta anterior corregida por el solucionario del libro:
```
Para (dom(h) = HSIM), (dom(m) = MATRIMONIO)
{t | ∃m((m.nombre-mujer = t) ^ ∀h((c ¬= m)))}
```

d) Hallar los hombres misóginos a quienes no cae simpática ninguna mujer.
Se necesitan las tablas: HOMBRE, MSIM
```
Para (dom(h) = HOMBRE), (dom(ms) = MSIM)
{t | ∃h((h.nombre-hombre = t) ^ ∀ms((ms.nombre-hombre) ¬= t))}
```

e) Hallar los hombres y mujeres asociales a quienes no cae nadie simpático.
Se necesitan las tablas: HSIM, MSIM
```
Para (dom(hs) = HSIM), (dom(ms) = MSIM)
{t | ∃hs ∃ms((hs.nombre-hombre = t) ^ (ms.nombre-mujer = t) ^ ∀hs ∀ms((hs.nombre-mujer ¬= t) ^ 
(ms.nombre-mujer ¬= t)))}
```

Consulta anterior corregida por el solucionario del libro:
```
Para (dom(hs) = HSIM), (dom(ms) = MSIM)
{t | (∃hs((hs.nombre-hombre = t) ^ ∀ms((ms.nombre-hombre ¬= t)) v ∃ms((ms.nombre-mujer = t) ^ 
∀hs((hs.nombre-mujer ¬= t))))}
```

`NOTA:` Hay que tener en cuenta que en cálculo relacional de tuplas cuando se quiere hacer uso o cuando se quieren
implementar dos soluciones para una consulta, se debe de poner una o (v) separando cada uno de los cuatificadores
existenciales, ya que, se producen como resultado de las consultas, es por ello que, como podemos ver en la
consulta anterior se querían los hombres y las mujeres cumpliéndose las mismas condiciones, por tanto, se
implementan ambas consultas de manera separada con un `v` como separador de estas.

f) Hallar las mujeres casadas que caen simpáticas a algún hombre.
Se necesitan las tablas: MATRIMONIO, MSIM
```
Para (dom(m) = MATRIMONIO), (dom(ms) = MSIM)
{t | ∃m((m.nombre-mujer = t) ^ ∃ms((ms.nombre-mujer = t)))}
```

g) Hallar los hombres a quienes sólo caen simpáticas mujeres casadas.
Se necesitan las tablas: MSIM, MATRIMONIO, HOMBRE
```
Para (dom(ms) = MSIM), (dom(m) = MATRIMONIO), (dom(h) = HOMBRE)
{t | ∃ms((ms.nombre-hombre = t)) ^ ∃m(ms.nombre-mujer = t)}
```

Consulta anterior corregida por el solucionario del libro:
```
Para (dom(ms) = MSIM), (dom(m) = MATRIMONIO), (dom(h) = HOMBRE)
{t | (∃h(h.nombre-hombre = t)) ^ (∀ms((ms.nombre-hombre ¬= t) v (ms.nombre-hombre = t) ^ 
(∃m((m.nombre-mujer = ms.nombre-mujer)))))}
```

`NOTA:` Nótese que para el caso anterior, se hace uso de un cuantificador dentro de otro cuantificador que ya
existe, para poder hacer uso de las tuplas que se han encontrado dentro de esta consulta, por ello, se puede realizar
esto para poder hacer uso de las tuplas que ya se encontraron de manera previa y poder emplearlas para las condciones
para la consulta original.

h) Hombres a quienes sólo cae simpática su esposa.
Se necesitan las tablas: MATRIMONIO, MSIM
```
Para (dom(m) = MATRIMONIO), (dom(ms) = MSIM)
{t | ∃m((m.nombre-hombre = t) ^ ∀ms((ms.nombre-hombre = t) ^ (ms.nombre-mujer = m.nombre-mujer)))}
```

