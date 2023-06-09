# Anotaciones para Examen

## 1. Solicitud de parejas de datos.

De manera general, si se realiza la solicitud de una consulta que obtenga las parejas de datos, de manera normal,
si las parejas de datos se solicitan sobre la misma tabla, se debe de realizar la implementación de dos tablas
de la misma con distinto nombre y realizando aquellas operaciones que sean necesarias para poder obtener dichas parejas
de datos. Por ejemplo, si solicitan parejas de nombres de coches para la tabla COCHE, se hace uso de C1 = C2 = COCHE,
por tanto, se hace uso de dichas dos tablas, permitiendo obtener las parejas de datos que son solicitadas
en el propio ejercicio a implementar.

## 2. Números especificados dentro de t en CRT

El número que se especifica dentro de t en el CRT, hace referencia al número de columnas que tendrá como resultado
la tabla tras la realización de la consulta, es por ello que, en la mayoría de las situaciones se genera que se tiene
t1, mientras que en ciertos casos se tiene t2 cuando existe más de una columna como resultado de la consulta.

Para poder entender esto se tiene:
```sql
-- Para el caso de que se produzca una única columna como resultado.
{t1 | (p = t)}
```

```sql
-- Para el caso de que se produzca más de una columna, para este caso dos.
{t2 | (p = t1) ^ (q = t2)}
```

## 3. Sentencias que solicitan `TODO` o `TODOS` los elementos en álgebra relacional.

Cuando ocurren situaciones en las que en la consulta se solicita todo, en álgebra relacional considerar siempre
el uso del conciente, ya que, en muchas de las situaciones, por no decir en todas, se debe de hacer uso del
operador cociente para poder resolver la consulta e implementarla de manera correcta y óptima.

## 4. Setencias que solicitan `algún` elemento en SQL.

De manera general cuando se solicita en la sentencia que algún elemento se encuentre dentro de otro, o que en el
propio enunciado aparezca la palabra algún, se produce que es necesario el uso de subconsultas para la realización
de la implementación de dicha consulta en SQL.

## 5. Realización de subconsultas en SQL.

En ocasiones, cuando son necesarias la implementación de subconsultas para obtener un valor en concreto o
un conjunto de valores en concreto, no es necesario siempre en todas las ocasiones hacer uso de otros operadores
que nos permitan implementar dichas subconsultas, sino que, cuando se quiere obtener un valor en concreto de una
tabla en concreto por ejemplo, se puede hacer uso de `()` por ejemplo, e implementar dicha subconsulta para poder
obtener el valor necesario. Es por ello que para poder entender esto, se tiene el siguiente ejemplo para poder
comprender:
```sql
-- 32) Obtener el nombre y el apellido de los clientes cuyo número de dni es menor que el del cliente 'Juan Martín'.\
-- Se necesitan las tablas: CLIENTES
SELECT nombre, apellidos
FROM CLIENTES
WHERE dni<(SELECT dni
           FROM CLIENTES
           WHERE nombre='Juan' AND apellidos='Martín');
```

## 6. Uso de negación `¬` cuando se hace uso de un para todo `∀` en una consulta CRT.

De manera común, cuando en una consulta CRT se hace uso de un para todo en alguna parte intermedia de la consulta,
se suele hacer uso de la negación de un dato con la variable libre t, ya que, cuando se implementa el para todo, 
en ocasiones las tuplas buscadas como resultado no coinciden con algunas de las dadas con el para todo, por tanto,
es necesario poner dicha negación al principio del para todo, para eliminar y evitarnos que resulte que dichas
tuplas salgan como resultado de la consulta final. Un ejemplo para poder ver esto y comprenderlo de manera
correcta es:

```sql
-- 25) Fábricas que usan sólo artículos que pueden ser suministrados por el proveedor P1.\
-- Se necesitan las tablas: PED, FAB, ART
-- Para (dom(pe) = PED), (dom(f) = FAB), (dom(pe1) = PED)
{t1 | ∃f ((f.NF = t)) ^ ∀pe ((pe.NF ¬= t) v ((pe.NF = t) ^ ∃pe1 ((pe1.NA = pe.NA ^ pe1.NP = 'P1'))))}
```

## 7. Uso del operador `ALL` de SQL.

Cuando en una consulta se hace referencia a `todos`, se debe de hacer uso del operador `ALL` delante de una subconsulta
ya que, tiene en cuenta todas las tuplas que resultan de dicha subconsulta, por tanto, es muy útil para ello.

## 8. Uso del operador `ANY` de SQL.

Cuando en una consulta se hace referencia a `alguno o algún` elemento, se debe de hacer uso del operador `ANY` delante
de la subconsulta que tiene como resultado las tuplas que serán comprobadas mediante el operador ANY.

## 9. Uso de los paréntesis en SQL.

Hay que tener cuidado con el uso de paréntesis en SQL, ya que, si en ocasiones que son necesarias, no se ponen de
manera correcta, puede resultar que la consulta no se realice de manera correcta o no funcione como es debido y
nos dé un error. Un ejemplo para poder entender el correcto uso de los paréntesis es:

```sql
-- Obtener el nombre y el apellido de los clientes cuyo nombre empieza por a y cuyo dni es mayor que el de alguno de
-- los clientres que son de Madrid o menor que el de todos los de Valencia.\
-- Se necesitan las tablas: CLIENTES
SELECT nombre, apellidos
FROM CLIENTES
WHERE nombre LIKE 'a%' AND (dni > ANY (SELECT dni
                                       FROM CLIENTES
                                       WHERE ciudad='Madrid') OR dni < ALL (SELECT dni
                                                                            FROM CLIENTES
                                                                            WHERE ciudad='Valencia'));
```

## 10. Uso del operador de conjunto `MAX` para cadenas de caracteres.

Si deseamos hacer uso del operador MAX para cadenas de caracteres, se puede hacer uso, pero, esto nos devolverá
el último elemento de una lista ordenada alfabética, por tanto, si existe algún elemento con una Z al comienzo de esta,
nos devolverá dicho elemento.

## 11. Uso del operador `EXISTS` en SQL.

Cuando en el enunciado de una consulta nos solicitan o en alguna parte de la consulta se especifica que `ningún`
elemento se produzca o que ningún elemento aparezca o elementos similares, es necesario el empleo del operador
`NOT EXISTS`, ya que, de esta manera se pone la sentencia en la que se quiere comproabar que si existe dicha
solución y dicho operador nos obtiene aquellas tuplas que no cumplen con ello. Un ejemplo para poder entender esto
anterior es:

```sql
-- Obtener los nombres de los clientes que no han comprado ningún coche rojo a ningún concesionario de Madrid.
SELECT nombre
FROM CLIENTES C
WHERE NOT EXISTS(SELECT *
                 FROM VENTAS V
                 WHERE V.dni=C.dni AND color='rojo' AND cifc IN (SELECT cifc
                                                                 FROM CONCESIONARIOS CO
                                                                 WHERE ciudad='Madrid'));
```

El operador `EXISTS` también puede ser usado en aquellas consultas en las que se especifica o se hace uso de la 
palabra `solo se produzca una x situación`. Por ejemplo, para poder entender esto anterior:

```sql
-- Obtener el nombre de los clientes que sólo han comprado en el concesionario de cifc 0001.
SELECT nombre
FROM CLIENTES
WHERE dni IN (SELECT dni
              FROM VENTAS V1
              WHERE NOT EXISTS(SELECT *
                               FROM VENTAS V2
                               WHERE V1.dni=V2.dni AND V2.cifc!='0001'));
```