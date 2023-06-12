# Exercice 2 SQL EXAM

Se tienen la siguientes tablas que implementan la base de datos:

ARTÍCULOS(CA, CAT) \
Significado: El artículo CA es de categoría CAT. \
CP: CA

TIENDAS(CT, CA, PR) \
Significado: La tienda con código CT dispone del artículo CA a un precio PR euros. \
CP: (CT, CA) \
CA: CA

VENTAS(DNI, CT, CA, NU, F) \
Significado: La persona con dni DNI, ha comprado en la tienda CT, NU unidades \
del artículo CA, en la fecha F. \
CP: DNI, CT, CA, F \
CA: CT, CA

Realizar la implementación de las distintas consultas que se puedes observar a continuación, haciendo uso de SQL:

1. Número de productos de la tienda T1 con un precio inferior a 30€.\
Se necesitan las tablas: TIENDAS
```sql
SELECT COUNT(CA)
FROM TIENDAS
WHERE (CT='T1') AND (PR<30);
```

Cabe destacar que en la implementación de Pablo, este hace uso de `COUNT(*)`, pero, como se especifica el número
de productor o artículos de dicha tienda, podemos hacer uso también de manera directa del código del artículo, CA.

2. Tiendas que no disponen de artículos de categoría C1.\
Se necesitan las tablas: TIENDAS, ARTÍCULOS
```sql
SELECT CT
FROM TIENDAS
WHERE CA NOT IN (SELECT CA
                 FROM ARTICULOS
                 WHERE CAT='C1');
```

3. Personas que han comprado en una misma tienda más de 10 unidades de productos iguales o distintos.\
Se necesitan las tablas: VENTAS
```sql
SELECT DNI
FROM VENTAS
WHERE (NU>10) OR ((TOTAL>10) AND TOTAL = (SELECT SUM(NU)
                                        FROM VENTAS
                                        WEHRE DNI = VENTAS.DNI
                                        GROUP BY DNI));
```

Esta consulta anterior es correcta y se puede especificar de dicha manera, pero, otra manera de poder implementar dicha
consulta es de la siguiente manera:
```sql
SELECT DNI
FROM VENTAS
GROUP BY CT, DNI
HAVING SUM(NU) > 10;
```

4. Tiendas que en un mismo día han vendido artículos de todas las categorías.\
Se necesitan las tablas: VENTAS, ARTICULOS
```sql
SELECT CT
FROM VENTAS
GROUP BY F, CA
HAVING NOT EXISTS(SELECT *
                  FROM ARTICULOS A1
                  WHERE NOT EXISTS(SELECT *
                                   FROM ARTICULOS A2
                                   WHERE A1.CA=A2.CA AND A1.CA=VENTAS.CA AND A2.CAT==A1.CAT));
```

Otra manera de implementar la consulta anterior es:
```sql
SELECT CT
FROM VENTAS NATURAL JOIN ARTICULOS
GROUP BY F, CA
HAVING COUNT(CAT) = (SELECT COUNT(CA)
                   FROM ARTICULOS);
```

5. Tiendas que al menos el 40% de sus artículos cuestan menos de 30€.\
Se necesitan las tablas: TIENDAS
```sql
SELECT CT
FROM TIENDAS T1
GROUP BY CT, CA
HAVING (TOTAL = (0.40 * COUNT(CA))) AND TOTAL >= (SELECT COUNT(CA)
                                                  FROM TIENDAS T2
                                                  GROUP BY CT, CA
                                                  HAVING (T1.CT=T2.CT) AND (PR<30)); 
```

6. Elimina en la tabla VENTAS la clave primaria.
```sql
ALTER TABLE VENTAS
DROP PRIMARY KEY;
```

7. Incrementa en un 10% el precio de todos los artículos de la tienda T1 que pertenecen a la categoría C1.\
Se necesitan las tablas: TIENDAS, ARTICULOS
```sql
UPDATE TIENDAS
SET PR = PR + (0.1 * PR)
WHERE (CT = 'T1') AND CA IN (SELECT CA
                             FROM ARTICULOS
                             WHERE (CAT = 'C1'));
```

8. Crea una tabla que almacene todas las ventas anteriores al 1 de enero de 2020 en la base de datos.\
```sql
CREATE TABLE NUEVAS_VENTAS AS (SELECT *
                               FROM VENTAS
                               WHERE F <='01-JAN-2020');
```

9. Impide que los precios de un mismo artículo puedan variar según la tienda.
```sql
ALTER TABLE TIENDAS
ADD CONSTRAINT IGUAL_PRECIO CHECK NOT EXISTS(SELECT *
                                             FROM TIENDAS T1
                                             WHERE NOT EXISTS(SELECT *
                                                              FROM TIENDAS T2
                                                              WHERE T1.CT = T2.CT AND T1.CA = T2.CA AND T1.PR != T2.PR));
```

10. Impide que una tienda pueda vender de un mismo producto más de 100 unidades en un mismo día.
```sql
ALTER TABLE VENTAS
ADD CONSTRAINT LIMITE_UNIDADES CHECK (NOT EXISTS(SELECT *
                                                FROM VENTAS V1
                                                WHERE NOT EXISTS(SELECT *
                                                                 FROM VENTAS V2
                                                                 WHERE V1.CT = V2.CT AND V1.CA = V2.CT AND NU > 100
                                                                 GROUP BY F)));
```