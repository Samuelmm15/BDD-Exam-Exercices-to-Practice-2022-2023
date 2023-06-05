# Exercice 5 SQL Book

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

Realizar la implementación de las siguientes consultas haciendo uso de SQL:

1) Obtener todos los valores de todos los concesionarios.\
Se necesita la tabla: CONCESIONARIOS
```SQL
SELECT *
FROM CONCESIONARIOS;
```

2) Obtener todos los campos de todos los clientes de Madrid.\
Se necesita la tabla: CLIENTES
```sql
SELECT *
FROM CLIENTES
WHERE ciudad='Madrid';
```

3) Obtener los nombres de todas las marcas de coches ordenadas alfabéticamente.\
Se necesita la tabla: MARCAS
```sql
SELECT nombre
FROM MARCAS
ORDER BY nombre DESC; 
-- Podemos especificar como vemos anteriormente en la consulta aterior, que queremos ordenar la tabla
-- de manera descendente alfabéticamente o ascendente en el caso que queramos.
```

4) Obtener el cifc de todos los concesionarios cuyo atributo cantidad en la tabla de distribución es mayor que 18.\
Se necesitan las tablas: CONCESIONARIOS, DISTRIBUCION
```sql
SELECT cifc
FROM CONCESIONARIOS NATURAL JOIN DISTRIBUCION
WHERE cantidad>18;
```

Corregido mediante el solucionario del libro, ya que se entendió mal el enunciado:
```sql
SELECT cifs
FROM DISTRIBUCION
WHERE cantidad>18;
```

5) Obtener el cifc de todos los concesionarios cuyo atributo cantidad en la tabla de distribución, está comprendido
entre 10 y 18 ambos inclusive.\
Se necesitan las tablas: DISTRIBUCION
```sql
SELECT cifc
FROM DISTRIBUCION
WHERE cantidad BETWEEN 10 AND 18;
-- Cabe destacar que para que 10 y 18 se encuentren incluidos también se deben de especificar, si no se quiere
-- que se incluyan, no deben de aparecer dichos números.
```

6) Obtener el cifc de todos los concecionarios que han adquiridos más de 10 coches o menos de 5 de algún tipo
(no en total). \
Se necesitan las tablas: DISTRIBUCIÓN
```sql
SELECT cifc
FROM DISTRIBUCION
WHERE cantidad>10 OR cantidad<5;
```

7) Obtener todas las parejas de cifm de marcas y dni de clientes que sean de la misma ciudad.\
Se necesitan las tablas: MARCAS, CLIENTES
```sql
SELECT cifm, dni
FROM MARCAS, CLIENTES
WHERE marcas.ciudad = clientes.ciudad;
```

8) Obtener todas las parejas de dni de clientes y cifm de marcas que NO sean de la misma ciudad.\
Se necesitan las tablas: MARCAS, CLIENTES
```sql
SELECT dni, cifm
FROM CLIENTES, MARCAS
WHERE NOT(CLIENTES.ciudad = MARCAS.ciudad);
```

9) Obtener los codcoche suministrados por algún concesionario de Barcelona.\
Se necesitan las tablas: COCHES, CONSECIONARIOS
```sql
SELECT codcoche
FROM COCHE, CONCESIONARIOS
WHERE (CONCESIONARIOS.ciudad = 'Barcelona');
```

Consulta corregida, ya que se me pasó la tabla distribución:
```sql
SELECT codcoche
FROM DISTRIBUCION, CONCESIONARIOS
WHERE DISTRIBUCION.cifc = CONCESIONARIOS.cifc AND CONCESIONARIOS.ciudad = 'Barcelona';
```

10) Obtener el codcoche de aquellos coches vendidos a clientes de 'Madrid'.\
Se necesitan las tablas: CLIENTES, VENTAS
```sql
SELECT codcoche
FROM CLIENTES, VENTAS
WHERE CLIENTES.dni = VENTAS.dni AND CLIENTES.ciudad = 'Madrid';
```

11) Obtener el codcohe de los coches que han sido adquiridos por un cliente de Madrid en un concesionario 
de Madrid.\
Se necesitan las tablas: VENTAS, CONCESIONARIOS, CLIENTES
```sql
SELECT codcoche
FROM VENTAS, CONCESIONARIOS, CLIENTES
WHERE VENTAS.dni=CLIENTES.dni AND CLIENTES.ciudad=Madrid AND VENTAS.cifc=CONCESIONARIOs.cifc
AND CONCESIONARIOS.ciudad=Madrid;
```

12) Obtener los codcoche de los coches comprados en un concesionario de la misma ciudad que el cliente que
lo compra.\
Se necesitan las tablas: VENTAS, CONCESIONARIOS, CLIENTES
```sql
SELECT codcoche
FROM VENTAS, CONCESIONARIOS, CLIENTES
WHERE VENTAS.dni=CLIENTES.dni
AND VENTAS.cifc=CONCESIONARIOS.cifc
AND CLIENTES.ciudad=CONCESIONARIOS.ciudad;
```

13) Obtener los codcoche de los coches comprados en un cocesionario de distintas ciudad que el cliente lo compra.\
Se necesitan las tablas: VENTAS, CONCESIONARIOS, CLIENTES
```sql
SELECT codcoche
FROM VENTAS, CONCESIONARIOS, CLIENTES
WHERE VENTAS.dni=CLIENTES.dni
AND VENTAS.cifc=CONCESIONARIOS.cifc
AND CLIENTES.ciudad!=CONCESIONARIOS.ciudad;
```

`NOTA:` en SQL también se puede hacer uso del operado `<>` para indicar distitnos en una consulta, es decir, que
también se puede hace uso de dicho comparador para poder indicar que algo es distinto a otro algo, de manera igual
que si se hace uso del operador de comparación `!=`.

14) Obtener todas las parejas de nombre de marcas que sean de la misma ciudad.\
Se necesita la tabla: MARCAS.
```sql
SELECT A.nombre, B.nombre
FROM MARCAS A, MARCAS
WHERE A.ciudad=B.ciudad
AND A.nombre<B.nombre;
```

`NOTA:` Cabe destacar que para este caso anterior, se necesitan dos tablas del mismo tipo, creando alias
de la manera que podemos observar anteriormente, además, podemos ver que se hace uso de la setencia
`A.nombre<B.nombre`, teniendo en cuenta que se hace uso de esto para evitar que las parejas de nombres de las
marcas no se repitan como por ejemplo: (audi, seat) y (seat, audi).

15) Obtener las parejas de modelos de coches cuyo nombre es el mismo y cuya marca es de Bilbao.\
Se necesitan las tablas: MARCAS, COCHES, MARCO.
```sql
SELECT A.nombre, B.nombre
FROM COCHES A, COCHES B, MARCAS, MARCO
WHERE A.nombre=B.nombre
AND A.modelo < B.modelo
AND MARCO.codcoche=A.codcoche
AND MARCO.cifm=MARCA.cifm
and MARCAS.ciudad='Bilbao';
```

16) Obtener todos los codcoche de los coches cuyo nombre empiece por c.\
Se necesitan las tablas: COCHES.
```sql
SELECT codcoche
FROM COCHES
WHERE nombre LIKE 'c%';
```

17) Obtener todos los codcoche de los coches cuyo nombre no contiene ninguna a.\
Se necesitan las tablas: COCHES.
```sql
SELECT codcoche
FROM COCHES
WHERE nombre NOT LIKE '%a%';
```

18) Obtener el número total de nombres de marcas de coches que son de Madrid.\
Se necesitan las tablas: MARCAS.
```sql
SELECT COUNT(DISTINCT nombre)
FROM MARCAS
WHERE ciudad='Madrid';
```

19) Obtener la media de la cantidad de coches que tienen todos los concesionarios.\
Se necesitan las tablas: DISTRIBUCION.
```sql
SELECT AVG(codcoche)
FROM DISTRIBUCION;
```

20) Obtener el dni cuya numeración sea la más alta de todos los clientes de Madrid.\
Se necesitan las tablas: CLIENTES.
```sql
SELECT MAX(dni)
FROM CLIENTES
WHERE ciudad='Madrid';
```