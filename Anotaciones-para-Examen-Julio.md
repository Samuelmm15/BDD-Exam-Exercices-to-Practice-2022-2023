# Anotaciones para examen

`NOTA:` Enlace de otros apuntes que pueden ser útiles: [NOTION-NOTES](https://fantasy-dance-bb2.notion.site/Ejercicios-de-repaso-EXAMEN-d5126f682ba84532bf6729bfd884a705#5fbec5f80a4f4af0ade652cd7065a5d9)
También se pueden tener en cuenta y observar otros apuntes que se encuentran dentro del directorio: [Directorio-Apuntes](./Apuntes-BDD-Completos)

## 1. Modo de funcionamiento de las consultas solicitadas por Santiso.

Para que una consulta sea correcta y se pueda dar como válida, las consultas en SQL por ejemplo deben de ser lo más
naturales posible, ya que, lo más normal es que una consulta implmentada y desarrollada en SQL sea escrita de la manera
más natural, debido a que esto nos permite leerlo de una mejor manera y soluciona el hecho de que aquellas consultas más
complejas no adquieran conplejidad inecesaria.

Otra de las técnicas más comunes que permiten realizar e implementar las consultas de cualquier tipo de lenguaje de
administración de bases de datos de manera correcta, es hacer uso e implementar las consultas de la manera más sencilla
posible toda consulta por complicada que parezca, es más sencilla de lo que aparenta en el enunciado.

## 2. `CUIDADO` con las proyecciones del álgebra relacional.

Hay que tener cuidado con las proyecciones realizadas en álgebra relacional, ya que, si en las vistas
que han sido usadas o que serán usadas para poder completar y realizar las consultas de manera correcta, si en alguna
de estas vistas ya se realiza la proyección de aquellos atributos que queremos sacar como resultado de la consulta, 
no hace falta de nuevo proyectarlos en el resultado final de dicha consulta. Por ejemplo:

```sql
A = p(DNI)(Nombre_Tabla)
B = p(DNI)(Nombre_Tabla)
    p(DNI)(A - B)
```

Como podemos observar en la consulta anterior, no debemos de hacer uso de `p(DNI)` en la consulta resultante, ya que
dicho atributo ya ha sido proyectado de manera previa en las vistas anteriores.

## 3. Cláusula `WHERE` y `HAVING` del SQL.

Cabe destacar que cada una de estas cláusulas tiene un funcionamiento distinto, ya que, la primera de ellas, el `WHERE`
se usa para establecer las condiciones de las tuplas de la propia tabla que se está usando, en cambio, el operador 
`HAVING` se encarga de establecer las condiciones y aquellos aspectos que tienen que ver con el establecimiento de las
condiciones de los grupos que se han realizado de manera previa mediante el operador `GROUP BY`.

## 4. Técnica de SQL para determinar que algún elemento sea único.

En SQL para poder determinar que algo es único, se hace uso de la siguiente sentencia para realizar dicha
operación: `COUNT(DISTINCT P) = 1`, con esta sentencia lo que se realiza, es que se comprueba que de aquellas tuplas
que se encuentran dentro del atributo P (para este caso), la suma de todos aquellos elementos distintos es 1, si no es 
uno indica que dicho elemento por tanto, no es único.

## 5. Creación de restricciones en SQL.

Para la creación y el establecimiento de restricciones en SQL se hace uso de la sentencia `CREATE ASSERTION`
, es decir, cuando en el enunciado de la consulta se nos indica `Inpón la restricción de que ocurra o que no ocurra algo`
, se debe de hacer uso de la sentencia especificada anteriormente, la cual nos permite establecer dicha restricción.

## 6. Uso del operador EXISTS en SQL.

El uso de dicho operador está muy limitado, ya que, cuando se emplea dicho operador dentro de las consultas normales implementadas
en SQL se produce que no se cumple con la condición de que las consultas sean lo más natural posible, por tanto, de
manera normal y común, se suele hacer uso de estos operadores de SQL dentro de las consultas de tipo `ALTER TABLE`,
dentro de los `CHECK` de dichas consultas.

## 7. Posibles peticiones solicitadas en SQL.

### Creación de una Vista.

```sql
CREATE VIEW VISTA_NOMBRE 
    AS (SUBCONSULTA);
```

### Modificación de la estructura de una tabla.

Esta consulta a continuación se usa para añadir columnas a la estructura de una tabla.
```sql
ALTER TABLE TABLA_NOMBRE 
    ADD (NUEVA_COLUMNA VARCHAR2(([NUM.LONG.MAX.STRING])(IS/NOT)NULL));
```

Esta consulta a continuación se usa para añadir una restricción a una tabla usada.
```sql
ALTER TABLE NOMBRE_TABLA
ADD CONSTRAINT NOMBRE_CONSTANTE CHECK (NOT EXISTS() / SUBCONSULTA)
```

### Modificación de las filas de una tabla.

```sql
UPDATE NOMBRE_TABLA
SET VALUES
WHERE (CONDITION);
```

### Insertar nuevas filas en SQL

```sql
INSERT INTO NOMBRE_TABLA(C1, C2, ...)
VALUES (V1, V2, ...);
```

### Creación de una asserción.

Cabe destabar que esto hace referencia a la creación de una restricción de la misma manera que el `ALTER TABLE`, pero,
con la diferencia de que para este caso realiza e implementa una restricción para la base de datos entera, es decir, más
de 1 tabla usada para establecer dicha restricción, el primer caso es solo para el establecimiento para una única tabla
como condición.

```sql
CREATE ASSERTION NOMBRE_ASSERTION CHECK NOT EXISTS(SUBCONSULTA);
```

### Creación de un `trigger`

Un `trigger` es como por así decirlo un pequeño script en SQL que nos permite que antes o después
de que ocurra una cierta acción en la base de datos, se ejecuta dicha consulta, es decir, cuando se produce 
algún cambio o cierto aspecto que se va a quedar comprobando de manera continua, pues, permite ejecutar aquello que
nosotros queramos en el momento en el que se ha producido la situación que se ha establecido como límite. Su estructura
general es:

```sql
CREATE TRIGGER NOMBRE_TRIGGER
    {BEFORE | AFTER}{DELETE | INSERT | UPDATE} ON NOMBRE_TABKA
        REFERENCING {NEW | OLD} AS NOMBRE_FILA
        FOR EACH ROW
        WHEN (NOMBRE_FILA = ...)
        BEGIN
        
        END;
```

### Borrado de un Trigger

```sql
DROP TRIGGER NOMBRE_TRIGGER;
```

## 8. Función de fecha y hora actuales en SQL.

Para poder establecer y determinar la fecha y hora actuales en SQL se hace uso de la función que se encarga de esto
directamente, esta función es `SYSDATE()`.

## 9. Números especificados dentro de t en CRT

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

## 10. Sentencias que solicitan `TODO` o `TODOS` los elementos en álgebra relacional.

Cuando ocurren situaciones en las que en la consulta se solicita todo, en álgebra relacional considerar siempre
el uso del conciente, ya que, en muchas de las situaciones, por no decir en todas, se debe de hacer uso del
operador cociente para poder resolver la consulta e implementarla de manera correcta y óptima.

## 11. Setencias que solicitan `algún` elemento en SQL.

De manera general cuando se solicita en la sentencia que algún elemento se encuentre dentro de otro, o que en el
propio enunciado aparezca la palabra algún, se produce que es necesario el uso de subconsultas para la realización
de la implementación de dicha consulta en SQL.

## 12. Eliminación de la clave primaria de una tabla especifica en SQL.

Para poder eliminar la clave primaria de una tabla que ha sido especificada en cocreto se hace uso del siguiente
código en general:
```sql
ALTER TABLE NOMBRE_TABLA
DROP PRIMARY KEY;
```

## 13. Cuidado con el operador `Cociente` del álgebra relacional.

Cuando se hace uso del operador cociente del álgebra relacional, hay que tener cuidado con este, ya que, si en ocasiones
tenemos que agrupar todas las tuplas teniendo en cuenta distintos grupos que vienen marcados por una Clave primaria
que permite identificar cada grupo de tuplas de manera independiente y única. Por tanto, para poder entender el
funcionamiento de dicho operador y su potencia, es necesario observar el siguiente ejemplo:
```sql
A = p(CC, I)(CANCIONES)
B = p(U, CC, I)(CANCIONES * LISTAS)
    p(U)(B / A)
```

En el caso anterior, se produce que al no tener identificada la vista B de manera única haciendo uso de la clave
primaria CL (código de la lista), para cada uno de los grupos que se van a formar en la vista B, se produce,
que puede ocurrir que personas que tienen canciones del mismo intérprete en otras listas, se produce que han sido
añadidas en otras listas por el mismo usuario, pero, al ser listas distintas y tener todas las canciones de dicho
intérprete en listas distintas, se puede reconocer como que todas las canciones de un mismo artista han sido introducidas
en las distintas listas de dicho usuario, pero, lo que queremos nosotros es que estén todas en una misma lista, por
tanto, cada una de las tuplas de la vista B deben de estar identificadas de manera única, haciendo uso de la clave
primaria `CL`. Es por ello que la consulta, anterior de manera corregida y correcta es:
```sql
A = p(CC, I)(CANCIONES)
B = p(CL, U, CC, I)(CANCIONES * LISTAS)
    p(U)(B / A)
```