## Exercice-6 Book

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

Realizar la implementación de las siguientes consultas haciendo uso de álgebra relacional:

1) Indicar una expresión que permite obtener las tuplas de las relación MARCAS para las que el atributo ciudad es
'Barcelona'.\
Se necesitan las tablas: MARCAS
```sql
p(cifm, nombre, ciudad)(s(ciudad = 'Barcelona')(MARCAS))
```

2) Obtener las tuplas de la relación Distribución para las que el atributo cantidad toma un valor mayor que 15.\
Se necesitan las tablas: DISTRIBUCION
```sql
p(cifc, nombre, cantidad)(s(cantidad > 15)(DISTRIBUCION))
```

3) Obtener las tuplas de la relación CLIENTES para las que el atributo apellido es García y el atributo ciudad es
'Madrid'.
Se necesitan las tablas: CLIENTE
```sql
p(dni, nombre, apellidos, ciudad)(s((apellido = 'García') ^ (ciudad = 'Madrid'))(CLIENTES))
```

4) Obtener las tuplas de la relación CLIENTES para las que el atributo apellido es García o el atributo ciudad es
Madrid.\
Se necesitan las tablas: CLIENTES
```sql
p(dni, nombre, apellidos, ciudad)(s((apellidos = 'García') v (ciudad = 'Madrid'))(CLIENTES))
```

5) Obtener las tuplas de la relación CLIENTES para las que el atributo ciudad toma un valor distintos de Madrid.\
Se necesitan las tablas: CLIENTES
```sql
p(dni, nombre, apellidos, ciudad)(s(ciudad ¬= 'Madrid')(CLIENTES))
```

6) Obtener los nombres de las marcas que tienen modelos 'gtd'.\
Se necesitan las tablas: MARCAS, MARCO, COCHES
```sql
p(MARCAS.nombre)(s(modelo = 'gtd')(MARCAS * MARCO * COCHES))
```

6) Obtener el nombre de las marcas de las que se han vendido coches de color rojo.\
Se necesitan las tablas: MARCAS, MARCO, VENTAS
```sql
p(nombre)(s(color = 'rojo')(MARCAS * MARCO * VENTAS))
```

Solución del solucionario del libro:
```sql
V1 = p(codcoche)(s(color = 'rojo')(VENTAS))
p(nombre)(p(cifm)(V1 * MARCO) * MARCAS)
```

7) Obtener el nombre de los coches que tengan al menos los mismos modelos que el coche de nombre 'cordoba'.\
Se necesitan las tablas: COCHES
```sql
p(nombre, modelo)(COCHES) / p(modelo)(s(modelo = 'Cordoba')(COCHES))
```

8) Obtener los nombres de los coches que no tengan modelo 'gtd'.\
Se necesitan las tablas: COCHES
```sql
p(nombre)(COCHES) - p(nombre)(s(modelo = 'gtd')(COCHES))
```

9) Obtener el cifc de todos los concesionarios que disponen de una cantidad de coches mayor que 18 unidades.\
Se necesitan las tablas: DISTRIBUCION
```sql
p(cifc)(s(cantidad >= 18)(DISTRIBUCION))
```

10) Obtener todas las parejas de valores de los atributos cifm de la relación MARCAS y dni de la relación CLIENTES
que sean de la misma ciudad.\
Se necesitan las tablas: MARCAS, MARCO, VENTA, CLIENTES
```sql
p(cifm, dni)(p(nombre)(MARCAS) * CLIENTES)
```

11) Obtener los valores del atributo codcoche para los coches que se encuentran en algún concesionario de 'Barcelona'.\
Se necesitan las tablas: DISTRIBUCION, CONCESIONARIOS
```sql
p(codcoche)(s(ciudad = 'Barcelona')(DISTRIBUCION * CONCESIONARIOS))
```

12) Obtener el valor del atributo codcoche de aquellos coches vendidos a clientes de Madrid.\
Se necesitan las tablas: VENTAS, CLIENTES
```sql
p(codcoche)(s(ciudad = 'Madrid')(VENTAS * CLIENTES))
```

13) Obtener los valores del atributo codcoche para los coches que han sido adquiridos por un cliente de Madrid en 
un concesionario de Madrid.\
Se necesitan las tablas: VENTAS, CLIENTES, CONCESIONARIOS
```sql
p(codcoche)(s((CLIENTES.ciudad = 'Madrid') ^ (CONCESIONARIOS.ciudad = 'Madrid'))(VENTAS * CLIENTES * CONCESIONARIOS))
```

14) Obtener los valores del atributo codcoche para los coches comprados en un concesionario de distinta ciudad que 
la del cliente que lo compra.\
Se necesitan las tablas: VENTAS, CLIENTES, CONCESIONARIOS
```sql
p(codcoche)(s(CLIENTES.ciudad ¬= CONCESIONARIOS.ciudad)(VENTAS * CLIENTES * CONCESIONARIOS))
```

15) Obtener todas las parejas de nombres de marcas que sean de la misma ciudad.\
Se necesitan las tablas: MARCAS
```sql
M1 = MARCAS
p(nombre, cifm)(MARCAS) / p(cifm)(s(MARCAS.ciudad = M1.ciudad)(MARCAS * M1))
```