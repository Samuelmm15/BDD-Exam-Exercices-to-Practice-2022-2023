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

Para no tener que subir tanto, cada vez que se quieran ver las tablas de la base de datos, se vuelven a introducir
en esta zona para poder ver todo de manera correcta:\
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

21) Obtener el DNI con numeración más baja de todos los clientes que han comprado un coche 'blanco'.\
Se necesitan las tablas: VENTAS
```sql
SELECT MIN(dni)
FROM VENTAS
WHERE color='Blanco';
```

22) Obtener el cifc de todos los concesionarios cuyo número de coches en sotck no es nulo.\
Se necesitan las tablas: CONCESIONARIOS, DISTRIBUCION.
```sql
SELECT cifc
FROM CONCESIONARIOS C, DISTRIBUCION D
WHERE cantidad IS NOT NULL;
```

No es necesaria la tabla de concesionarios, por tanto, la solución real es:
```sql
SELECT DISTINCT cifc
FROM CONCESIONARIOS
WHERE cantidad IS NOT NULL;
```

23) Obtener el cifm y el nombre de las marcas de coches cuya segunda letra del nombre de la ciudad de origen sea
una 'i'.\
Se necesitan las tablas: MARCAS
```sql
SELECT cifm, nombre
FROM MARCAS
WHERE nombre LIKE '_i%';
```

24) Obtener el dni de los clientes que han comprado algún coche a un concesionario de Madrid.\
Se necesitan las tablas: VENTAS, CONCESIONARIOS
```sql
SELECT DISTINCT dni
FROM VENTAS
WHERE cifc IN (SELECT cifc
               FROM CONCESIONARIOS
               WHERE ciudad='Madrid');
```

`NOTA:` Cabe destacar que en SQL cuando se solicita o hace referencia a álgun elemento, es decir, que la consulta
aparece la palabra algún, se suele implementar dicha consulta haciendo uso de subconsultas.

25) Obtener el color de los coches vendidos por el concesionario 'acar'.\
Se necesitan las tablas: VENTAS, CONCESIONARIOS
```sql
SELECT color
FROM VENTAS
WHERE cifc IN (SELECT cifc
               FROM CONCESIONARIOS
               WHERE nombre='acar');
```

26) Obtener el codcoche de los coches vendidos por algún cocesionario de Madrid.\
Se necesitan las tablas: VENTAS, CONCESIONARIOS
```sql
SELECT codcoche
FROM VENTAS
WHERE cifc IN (SELECT cifc
               FROM CONCESIONARIOS
               WHERE ciudad='Madrid');
```

27) Obtener el nombre y el modelo de los coches vendidos por algún concesionario de Barcelona.\
Se necesitan las tablas: VENTAS, CONCESIONARIOS, COCHES
```sql
SELECT nombre, modelo
FROM COCHES
WHERE codcoche IN (SELECT codcoche
                   FROM VENTAS
                   WHERE cifc IN (SELECT cifc
                                  FROM CONCESIONARIOS
                                  WHERE ciudad='Barcelona'));
```

28) Obtener todos los nombres de los clientes que hayan adquirido algún coche del cocesionario 'dcar'.\
Se necesitan las tablas: VENTAS, CONCESIONARIOS, CLIENTES
```sql
SELECT nombre
FROM CLIENTES
WHERE dni IN (SELECT dni
              FROM VENTAS
              WHERE cifc IN (SELECT cifc
                             FROM CONCESIONARIOS
                             WHERE nombre='dcar'));
```

29) Obtener el nombre y el apellido de los clientes que han adquirido un coche modelo 'gti' de color 'blanco'.\
Se necesitan las tablas: CLIENTES, VENTAS, COCHES
```sql
SELECT nombre, apellidos
FROM CLIENTES
WHERE dni IN (SELECT dni
              FROM VENTAS
              WHERE color='blanco' AND codcoche IN (SELECT codcoche
                                                    FROM COCHES
                                                    WHERE modelo='gti'));
```

30) Obtener el nombre y el apellido de los clientes que han adquirido un automóvil a un cocesionario que posea
actualmente coches en stock del modelo 'gti'.\
Se necesitan las tablas: CLIENTES, VENTAS, COCHES, DISTRIBUCION
```sql
SELECT nombre, apellidos
FROM CLIENTES
WHERE dni IN (SELECT dni
              FROM VENTAS
              WHERE cifc IN (SELECT cifc
                                 FROM DISTRIBUCION
                                 WHERE codcoche IN (SELECT codcoche
                                                    FROM COCHES
                                                    WHERE modelo='gti');
```

31) Obtener el nombre y el apellido de los clientes que han adquirido un automóvil a un cocesionario de 'Madrid' que
posea actualmente coches en stock del modelo 'gti'.\
Se necesitan las tablas: CLIENTES, COCHES, VENTAS, DISTRIBUCION
```sql
SELECT nombre, apellidos
FROM CLIENTES
WHERE dni IN (SELECT dni
              FROM VENTAS
              WHERE cifc IN (SELECT cifc
                             FROM CONCESIONARIOS
                             WHERE ciudad='Madrid'
                             ) AND cifc IN (SELECT cifc
                                                 FROM DISTRIBUCION
                                                 WHERE codcoche IN (SELECT codcoche
                                                                    FROM COCHES
                                                                    WHERE modelo='gti')));
```

32) Obtener el nombre y el apellido de los clientes cuyo número de dni es menor que el del cliente 'Juan Martín'.\
Se necesitan las tablas: CLIENTES
```sql
SELECT nombre, apellidos
FROM CLIENTES
WHERE dni<(SELECT dni
           FROM CLIENTES
           WHERE nombre='Juan' AND apellidos='Martín');
```

`NOTA:` Tener en cuenta que para poder realizar subconsultas no es necesario el empleo de otros operadores que
nos permitan hacer subconsultas, si no que en el caso que queramos, se pueden realizar las subconsultas de manera
directa.

33) Obtener el nombre y el apellido de los clientes cuyo dni es menor que el de los clientes que son de 'Barcelona'.\
Se necesitan las tablas: CLIENTES
```sql
SELECT nombre, apellido
FROM CLIENTES
WHERE dni<(SELECT dni
           FROM CLIENTES
           WHERE ciudad='Barcelona');
```

34) Obtener el nombre y el apellido de los clientes cuyo nombre empieza por 'a' y cuyo número del dni
es mayor que el de todos los clientes que son de Madrid.\
Se necesitan las tablas: CLIENTES
```sql
SELECT nombre, apellidos
FROM CLIENTES
WHERE nombre LIKE 'a%' AND dni > ALL (SELECT dni
                                  FROM CLIENTES
                                  WHERE ciudad='Madrid');
```

`NOTA:` En sql cuando se necesita o cuando la consulta solicitada necesita o hace referencia a `todos`, se hace uso
del operador ALL del SQL.

35) Obtener el nombre y el apellido de los clientes cuyo nombre empieza por a y cuyo número de dni es mayor que el de
alguno de los clientes que son de Madrid.\
Se necesitan las tablas: CLIENTES
```sql
SELECT nombre, apellidos
FROM CLIENTES
WHERE nombre LIKE 'a%' AND dni > ANY (SELECT dni
                                      FROM CLIENTES
                                      WHERE ciudad='Madrid');
```

`NOTA:` En SQL cuando se refiere en el enunciado de la consulta como alguno, se debe de hacer uso del operador `ANY`
delante de la subconsulta.

36) Obtener el nombre y el apellido de los clientes cuyo nombre empieza por a y cuyo dni es mayor que el de alguno de
los clientres que son de Madrid o menor que el de todos los de Valencia.\
Se necesitan las tablas: CLIENTES
```sql
SELECT nombre, apellidos
FROM CLIENTES
WHERE nombre LIKE 'a%' AND (dni > ANY (SELECT dni
                                      FROM CLIENTES
                                      WHERE ciudad='Madrid') OR dni < ALL (SELECT dni
                                                                           FROM CLIENTES
                                                                           WHERE ciudad='Valencia'));
```

`NOTA:` En SQL hay que tener cuidado con los paréntesis, ya que si no se ponen de manera correcta, se puede
producir que la consulta no se realice ni se implemente de manera correcta.

37) Obtener el nombre y el apellido de los clientes que han comprado como mínimo un coche blanco y un coche rojo.\
Se necesitan las tablas: CLIENTES, VENTA
```sql
SELECT nombre, apellidos
FROM CLIENTES
WHERE dni IN (SELECT dni
              FROM VENTAS
              WHERE color='blanco') AND dni IN (SELECT dni
                                                FROM VENTAS
                                                WHERE color='rojo');
```

`NOTA:` Hay que tener en cuenta que debido a que no se puede tener color= a los distintos colores dentro de la 
misma consulta, ya que se generaría un error, debido a que no se pueden tener dos colores distintos dentro de la
misma tupla, por tanto, se realizan dos subconsultas en las que en las dos se realiza la búsqueda de los dos
colores distitnos para el mismo dni.

38) Obtener el dni de los clientes cuya ciudad sea la última de la lista alfabética de las ciudades donde hay 
concesionarios.\
Se necesitan las tablas: CONCESIONARIOS, CLIENTES
```sql
SELECT dni
FROM CLIENTES
WHERE ciudad = (SELECT MAX(ciudad)
                FROM CONCESIONARIOS);
```

Para no tener que subir tanto, cada vez que se quieran ver las tablas de la base de datos, se vuelven a introducir
en esta zona para poder ver todo de manera correcta:\
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

39) Obtener la media de los automóviles que cada concesionario tiene actualemente en stock.\
Se necesitan las tablas: DISTRIBUCION
```sql
SELECT cifc, AVG(cantidad)
FROM DISTRIBUCION
GROUP BY cifc;
```

`NOTA:` Haciendo uso del `GROUP BY` se nos permite poder obtener la media de cada uno de los distitnos
concesionarios, ya que al agruparlos teniendo en cuenta sus correspondientes claves primarias, se produce que,
se seleccionan los distintos concesionarios que existen en dicha tabla.

40) Obtener el cifc del concesionario que no sea de Madrid cuya media de vehículos en stock sea la más alta
de todas las medias.\
Se necesitan las tablas: CONCESIONARIOS, DISTRIBUCION
```sql
SELECT cifc
FROM DISTRIBUCION
WHERE cifc IN (SELECT cifc
               FROM CONCESIONARIOS
               WHERE ciudad!='Madrid') AND AVG(cantidad) > ALL (SELECT AVG(cantidad)
                                                                FROM DISTRIBUCION);
```

Consulta anterior corregida mediante el solucionario del libro:
```sql
SELECT cifc
FROM CONCESIONARIO
WHERE ciudad!='Madrid' AND cifc IN (SELECT cifc
                                    FROM DISTRIBUCION
                                    GROUP BY cifc
                                    HAVING AVG(cantidad) >= (SELECT AVG(cantidad)
                                                            FROM DISTRIBUCION
                                                            GROUP BY cifc));
```

41) Obtener el codcoche de los coches vendidas por algún concesionario de 'Madrid', pero, usando el operador
`EXISTS` en el resultado.\
Se necesitan las tablas: VENTAS, CONCESIONARIO
```sql
SELECT codcoche
FROM VENTAS
WHERE EXISTS(SELECT *
             FROM CONCESIONARIO
             WHERE ciudad='Madrid' AND CONCESIONARIO.cifc = VENTAS.cifc);
```

42) Utilizando EXISTS, obtener el dni de los clientes que hayan adquirido por lo menos alguno de los coches que
han sido vendidos por el concesionario cuyo cifc es 0001.\
Se necesitan las tablas: VENTAS
```sql
SELECT DISTINCT dni
FROM VENTAS
WHERE EXISTS(SELECT *
             FROM VENTAS V
             WHERE cifc='0001' AND V.codcoche = VENTAS.codcoche);
```

43) Obtener el dni de los clientes que sólo hayan comprado coches al cocesionario 0001.\
Se necesitan las tablas: VENTAS
```sql
SELECT dni
FROM VENTAS V1
WHERE NOT EXISTS(SELECT *
                 FROM VENTAS V2
                 WHERE cifc!=0001 AND V1.dni = V2.dni);
```

44) Obtener los nombres de los clientes que no han comprado ningún coche rojo a ningún concesionario de Madrid.\
Se necesitan las tablas: CLIENTES, VENTA, CONCESIONARIOS
```sql
SELECT nombre
FROM CLIENTES C
WHERE NOT EXISTS(SELECT dni
                 FROM VENTAS V
                 WHERE V.dni=C.dni AND color='rojo' AND NOT EXISTS(SELECT cifc
                                                                   FROM CONCESIONARIOS CO
                                                                   WHERE CO.cifc=V.cifc AND ciudad='Madrid'));
```

Esta consulta un podo modificada en el solucionario del libro:
```sql
SELECT nombre
FROM CLIENTES C
WHERE NOT EXISTS(SELECT *
                 FROM VENTAS V
                 WHERE V.dni=C.dni AND color='rojo' AND cifc IN (SELECT cifc
                                                                 FROM CONCESIONARIOS CO
                                                                 WHERE ciudad='Madrid'));
```

45) Obtener el nombre de los clientes que sólo han comprado en el concesionario de cifc 0001.\
Se necesitan las tablas: CLIENTES, VENTAS
```sql
SELECT nombre
FROM CLIENTES
WHERE dni IN (SELECT dni
              FROM VENTAS V1
              WHERE NOT EXISTS(SELECT *
                               FROM VENTAS V2
                               WHERE V1.dni=V2.dni AND V2.cifc!='0001'));
```

46) Obtener el codcoche de aquellos automóviles que han sido comprados por todos los clientes de 'Madrid'.\
Se necesitan las tablas: VENTAS, CLIENTES
```sql
SELECT codcoche
FROM VENTAS
WHERE ALL dni IN (SELECT dni
                  FROM CLIENTES
                  WHERE ciudad='Madrid');
```

Esto anterior corregido mediante el solucionario del libro:
```sql
SELECT DISTINCT codcoche
FROM VENTAS V1
WHERE NOT EXISTS(SELECT *
                 FROM CLIENTES
                 WHERE ciudad='Madrid' AND NOT EXISTS(SELECT *
                                                      FROM VENTAS V2
                                                      WHERE V2.dni=CLIENTES.dni AND V2.codcoche=V1.codcoche));
```

47) Obtener el codcoche de aquellos automóviles de color rojo y de modelo gti que han sido comprados por todos 
los clientes cuyo apellido comienza por g.\
Se necesitan las tablas: VENTAS, COCHES, CLIENTES
```sql
SELECT codcoche
FROM VENTAS V1
WHERE color='rojo' AND codcoche IN (SELECT codcoche
                                    FROM COCHES
                                    WHERE modelo='gti') AND NOT EXISTS(SELECT *
                                                                       FROM CLIENTES
                                                                       WHERE apellido LIKE 'g%' AND NOT EXISTS(SELECT *
                                                                                                               FROM VENTAS V2
                                                                                                               WHERE V2.dni=CLIENTE.dni AND V2.codcoche=V1.codcoche));
```

48) Obtener el dni de los clientes que han adquirido por lo menos los mismo automóviles que el cliente 'Luis García'.\
Se necesitan las tablas: CLIENTES, VENTAS
```sql
SELECT DISTINCT dni
FROM VENTAS V1
WHERE NOT EXISTS(SELECT *
                 FROM CLIENTES
                 WHERE nombre='Luis' AND apellido='García' AND NOT EXISTS(SELECT *
                                                                          FROM VENTAS V2
                                                                          WHERE V2.dni=CLIENTES.dni AND V2.codcoche=V1.codcoche));
```

Consulta corregida mediante el solucionario del libro:
```sql
SELECT DISTINCT dni
FROM VENTAS V1
WHERE NOT EXISTS(SELECT codcoche
                 FROM VENTAS V2
                 WHERE dni IN (SELECT DNI
                               FROM CLIENTES
                               WHERE nombre='Luis' AND apellido='García')) AND NOT EXISTS(SELECT *
                                                                                          FROM VENTAS V3
                                                                                          WHERE V3.codcoche=V2.codcoche
                                                                                          AND V3.dni=V1.dni);
```

48) Obtener el cifc de los concesionarios que han vendido el mismo coches a todos los clientes.\
Se necesitan las tablas: VENTAS, CLIENTES
```sql
SELECT cifc
FROM VENTAS V1
WHERE NOT EXISTS(SELECT codcoche
                 FROM VENTAS V2
                 WHERE NOT EXISTS(SELECT dni
                                  FROM CLIENTES
                                  WHERE NOT EXISTS(SELECT *
                                                   FROM VENTAS V3
                                                   WHERE V3.cifc=V1.cifc
                                                   AND V3.codcoche=V2.codcoche
                                                   AND V3.dni=CLIENTES.dni)));
```