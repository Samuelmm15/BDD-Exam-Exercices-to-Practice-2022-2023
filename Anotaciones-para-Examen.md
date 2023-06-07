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

