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