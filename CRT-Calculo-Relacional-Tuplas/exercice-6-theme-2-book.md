Sean las siguientes tablas:

MARCAS(cifm, nombre, ciudad)\
CLAVE: cifm

COCHES(codcoche, nombre, modelo)\
CLAVE: codcoche

CONCESIONARIOS(cifc, nombre, ciudad)\
CLAVE: cifc

CLIENTES(dni, nombre, apellidos, ciudad)\
CLAVE: dni

DISTRIBUCION(cifc, codcoche, cantidad)\
CLAVE: cifc, codcoche

VENTAS(cifc, dni, codcoche, color)\
CLAVE: cifc, dni, codcoche

MARCO(cifm, codcoche)\
CLAVE: cifm, codcoche

Realizar la implementación de las siguientes consultas haciendo uso del cálculo relacional de tuplas:

1) Obtener todas las tuplas de la tabla de clientes.\
Se necesitan las tablas: CLIENTE
```sql
{t1 | t ∈ CLIENTES}
```

2) Obtener todas las tuplas de las relación distribución cuyo valor en el atributo cantidad sea mayor que 10.
Se necesita la tabla: DISTRIBUCION
```sql
Para (dom(d) = DISTRIBUCION)
{t1 | ∃d ((d.cantidad > 10) ^ (d = t) ∈ DISTRIBUCION)}
```

3) Obtener los valores del atributo cifc que aparecen junto a un valor del atributo cantidad mayor que 10 en las
tuplas de la relación de distribución.\
Se necesitan las tablas: DISTRIBUCION
```sql
Para (dom(d) = DISTRIBUCION)
{t1 | ∃d ((d.cifc = t) ^ (d.cantidad > 10))}
```

4) Para el concesionario con cifc igual a 0005, obtener los valores del atributo codcoche que en distribución 
aparecen junto a un valor del atributo cantidad mayor que 10.
Se necesitan las tablas: DISTRIBUCION
```sql
Para (dom(d) = DISTRIBUCION)
{t1 | ∃d ((d.codcoche = t) ^ (d.cifc = '0005') ^ (d.cantidad > 10))}
```

5) Obtener los valores de los atributos dni y ciudad para los clientes que han comprado coches de color 'rojo'.\
Se necesitan las tablas: CLIENTES, VENTAS
```sql
Para (dom(c) = CLIENTES), (dom(v) = VENTAS)
{t2 | ∃v ∃c ((c.dni = t1) ^ (c.ciudad = t2) ^ (v.dni = c.dni) ^ (v.color = 'rojo'))}
```

6) Obtener los valores de los atributos dni y ciudad para los clientes que han comprado coches de color rojo
o de color blanco o de ambos colores.\
Se necesitan las tablas: CLIENTES, VENTAS
```sql
Para (dom(c) = CLIENTES), (dom(v) = VENTAS)
{t2 | ∃v ∃c ((c.dni = t1) ^ (c.ciudad = t2) ^ (v.dni = c.dni) ^ ((v.color = 'rojo') v (v.color = 'blanco') v 
    ((v.color = 'rojo') ^ (v.color = 'blanco'))))}
```

7) Obtener los valores de los atributos dni y ciudad para los clientes que han comprado al menos un coche de color
y otro de color blanco.\
Se necesitan las tablas: CLIENTES, VENTAS
```sql
Para (dom(c) = CLIENTES), (dom(v1) = VENTAS), (dom(v2) = VENTAS
{t2 | ∃v1 ∃v2 ∃c ((c.dni = t1) ^ (c.ciudad = t2) ^ (v1.dni = c.dni) ^ (v2.dni = c.dni) ^ (v1.color = 'rojo') ^ 
    (v2.color = 'blanco'))}
```

8) Obtener los valores de los atributos dni y ciudad para los clientes que han comprado coches de color rojo
y alguno que no sea de color blanco.\
Se necesitan las tablas: CLIENTES, VENTAS
```sql
Para (dom(c) = CLIENTES), (dom(v1) = VENTAS), (dom(v2) = VENTAS
{t2 | ∃v1 ∃v2 ∃c ((c.dni = t1) ^ (c.ciudad = t2) ^ (v1.dni = c.dni) ^ (v2.dni = c.dni) ^ (v1.color = 'rojo') 
    ^ (v2.color ¬= 'blanco'))}
```

9) Obtener los valores del atributo dni para los clientes que han comprado coches de color rojo pero no han 
comprado coches de color blanco.
Se necesita la tabla: VENTA
```sql
Para (dom(v1) = VENTA), (dom(v2) = VENTA)
{t1 | ∃v1 ∃v2 ((v1.dni = t) ^ (v1.color = 'rojo') ^ (v2.dni = v1.dni) ^ (v2.color ¬= 'blanco')))}
```

10) Obtener los valores del atributo dni para los clientes que no han comprado coches de color rojo o han comprado
al menos un coche de color blanco.\
Se necesitan las tablas: VENTAS
```sql
Para (dom(v1) = VENTAS), (dom(v2) = VENTAS)
{t1 | ∃v1 ∃v2 ((v1.dni = t) ^ (v1.color='rojo') ^ (v2.color='blanco') ^ (v2.dni=v1.dni))}
```

11) Obtener el nombre de los clientes que han comprado coches en todos los concesionarios.\
Se necesitan las tablas: VENTAS, CONCESIONARIOS, CLIENTES.
```sql
Para (dom(v) = VENTAS), (dom(co) = CONCESIONARIOS), (dom(cl) = CLIENTES)
{t1 | ∃v ∃cl ((cl.nombre = t) ^ (cl.dni = v.dni) ^ ∀co ((co.cifc = v.cifc)))}
```

12) Sean COCHES1 y COCHES2 dos relaciones en las que aparecen las tuplas de la relación COCHES correspondientes al
modelo gti y las tuplas de la relación COCHES que tienen por nombre ibiza. Indicar una expresión CRT similar a 
COCHES1 ∩ COCHES2 en álgebra relacional.
```sql
Para (dom(c1) = COCHES1), (dom(c2) = COCHES2)
{t3 | ∃c1 ∃c2 ((c1.codcoche = t1) ^ (c1.nombre = t2) ^ (c1.modelo = t3) ^ (c2.codcoche = c1.codcoche) ^ 
    (c2.nombre = c1.nombre) ^ (c2.modelo = c1.modelo))}
```

13) Obtener los nombres de las marcas que tienen modelos 'gtd'.\
Se necesitan las tablas: MARCAS, MARCO, COCHE
```sql
Para (dom(ma) = MARCA), (dom(mo) = MARCO), (dom(c) = COCHE)
{t1 | ∃ma ∃mo ∃c ((ma.nombre = t) ^ (ma.cifm = mo.cifm) ^ (mo.codcoche = c.codcoche) ^ (c.modelo = 'gtd'))}
```

14) Obtener los nombres de las marcas de las que se han vendido coches de color rojo.\
Se necesitan las tablas: MARCAS, MARCO, VENTA
```sql
Para (dom(ma) = MARCA), (dom(mo) = MARCO), (dom(v) = VENTA)
{t1 | ∃ma ∃mo ∃v ((ma.nombre = t) ^ (ma.cifm = mo.cifm) ^ (mo.codcoche = v.codcoche) ^ (v.color = 'rojo'))}
```

15) Obtener los nombres de los coches que tengan al menos todos los modelos que tiene el coche de nombre 'cordoba'.\
Se necesitan las tablas: COCHES
```sql
Para (dom(c1) = COCHES), (dom(c2) = COCHES)
{t1 | ∃c1 ((c1.nombre = t) ^ ∀c2 ((c2.nombre = 'cordoba') ^ (c1.modelo = c2.modelo)))}
```

16) Obtener el nombre de los coches que no tengan modelo 'gtd'.\
Se necesitan las tablas: COCHES
```sql
Para (dom(c) = COCHES)
{t1 | ∃c ((c.nombre = t) ^ (c.modelo ¬= 'gtd'))}
```

17) Obtener todas las tuplas de las relación de concesionarios.\
Se necesitan las tablas: CONCESIONARIOS.
```sql
{t | t ∈ CONCESIONARIOS}
```

18) Obtener todas las tuplas de todos los clientes de Madrid.\
Se necesitan las tablas: CLIENTES
```sql
Para (dom(c) = CLIENTES)
{t | t ∈ CLIENTES ^ t.ciudad = 'Madrid'}
```

19) Obtener todas las parejas de atributos cifm de la realción de MARCAS y dni de la relación CLIENTES que sean
de la misma ciudad.\
Se necesitan las tablas: MARCAS, CLIENTES
```sql
Para (dom(m) = MARCAS), (dom(c) = CLIENTES)
{t2 | ∃m ∃c ((m.cifm = t1) ^ (c.dni = t2) ^ (m.ciudad = c.ciudad))}
```

20) Obtener todas las parejas de valores de los atributos cifm de la relación MARCAS y dni de la relación CLIENTES
que no sean de la misma ciudad.\
```sql
Para (dom(m) = MARCAS), (dom(c) = CLIENTES)
{t2 | ∃m ∃c ((m.cifm = t1) ^(c.dni = t2) ^ (m.ciudad ¬= c.ciudad))}
```