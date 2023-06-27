# Exercice SQL Exam Ventas

Se tienen las siguientes relaciones de la base de datos correspondiente:

ARTICULOS(CA, CAT)\
Significado: El artículo CA es de categoría CAT\
CP: CA

TIENDAS(CT, CA, PR)\
Significado: La tienda con código CT dispone de un artículo CA a un precio de PR euros.\
CP: CT, CA\
CA: CA

VENTAS(DNI, CT, CA NU, F)\
Significado: La persona con dni DNI ha comprado en la tienda CT, NU unidades del artículo CA, en la fecha F\
CP: DNI, CT, CA, F\
CA: CT, CA

Realizar la implementación de las distintas consultas a continuación, haciendo uso de SQL:

1) Número de productos de la tienda T1 con un precio inferior a 30€.\
Se necesitan las tablas: TIENDAS
```sql
SELECT COUNT(*)
FROM TIENDAS
WHERE (CT = 'T1') AND (PR <= 30);
```

2) Tiendas que no disponen de artículos de la categoría C1.\
Se necesitan las tablas: TIENDAS, ARTÍCULOS
```sql
SELECT CT
FROM TIENDAS T
GROUP BY CT
HAVING NOT EXISTS(SELECT *
                  FROM ARTICULOS A
                  WHERE (A.CA = T.CA) AND (CAT = 'C1'));
```

Otra manera de poner la consulta anterior es:
```sql
SELECT CT
FROM TIENDAS
WHERE CA NOT IN (SELECT CA
                 FROM ARTICULOS
                 WHERE (CAT = 'C1'));
```

3) Personas que han comprado en una misma tienda más de 10 unidades de productos iguales o distintos.\
Se necesitan las tablas: VENTAS
```sql
SELECT DNI
FROM VENTAS V1
GROUP BY DNI, CT
HAVING (NU >= 10) OR ((TOTAL_PRODUCTS >= 10) AND TOTAL_PRODUCTS = (SELECT SUM(NU)
                                                                   FROM VENTAS V2
                                                                   GROUP BY CT
                                                                   HAVING (V1.CT = V2.CT)));
```

Otra manera de hacer esto anterior es:
```sql
SELECT DNI
FROM VENTAS
GROUP BY DNI, CT
HAVING (NU >= 10) OR (SUM(NU) >= 10);
```

4) Tiendas que en un mismo día han vendido artículos de todas las categorías.\
Se necesitan las tablas: VENTAS, ARTICULOS
```sql
SELECT CT
FROM VENTAS V1
GROUP BY CT, F
HAVING (COUNT(DISTINCT CA) = (SELECT COUNT(CAT)
                    FROM ARTICULOS
                    GROUP BY CAT)) AND NOT EXISTS(SELECT *
                                                  FROM ARTICULOS A1
                                                  WHERE NOT EXISTS(SELECT *
                                                                   FROM ARTICULOS A2
                                                                   WHERE (A1.CA = A2.CA) AND (A1.CA = V1.CA) 
                                                                     AND (A1.CAT = A2.CAT)));
```

5) Tiendas tales que al menos el 40% de sus artículos cuestan menos de 30€.\
Se necesitan las tablas: TIENDAS
```sql
SELECT CT
FROM TIENDAS T1
GROUP BY CT
HAVING (TOTAL_ARTICLES >= (0.40 * COUNT(*))) AND (TOTAL_ARTICLES = (SELECT COUNT(DISTINCT CA)
                                                                              FROM TIENDAS T2
                                                                              WHERE (T1.CT = T2.CT) AND (PR <= 30)));
```

6) Elimina en la tabla ventas la clave primaria.\
```sql
ALTER TABLE VENTAS
DROP PRIMARY KEY;
```

7) Incrementa en un 10% el precio de todos los artículos de la tienda T1 que pertenecen a la categoría C1.\
Se necesitan las tablas: TIENDAS, ARTICULOS
```sql
UPDATE TIENDAS
SET PR = (PR + (0.1 * PR))
WHERE (CT = 'T1') AND CA IN (SELECT CA
                             FROM ARTICULOS
                             WHERE (CAT = 'C1'));
```

8) Crea una tabla que almacene todas las ventas anteriores al 1 de enero de 2020 de la base de datos.\
Se necesitan las tablas: VENTAS
```sql
CREATE TABLE PREVIOUS_VENTS AS (SELECT *
                                FROM VENTAS
                                WHERE (F <= '01-JAN-2020'));
```

9) Impide que los precios de un mismo artículo puedan variar según la tienda.\
Se necesitan las tablas: TIENDAS
```sql
ALTER TABLE TIENDAS
ADD CONSTRAINT LIMIT_PRICE CHECK NOT EXISTS(SELECT *
                                            FROM TIENDA T1
                                            WHERE NOT EXISTS(SELECT *
                                                             FROM TIENDAS T2
                                                             WHERE (T1.CT != T2.CT) AND (T1.CA = T2.CA) 
                                                               AND (T1.PR != T2.PR)))
```

10) Impide que una tienda pueda vender de un mismo producto más de 100 unidades en un mismo día.\
Se necesitan las tablas: VENTAS
```sql
ALTER TABLE VENTAS
ADD CONSTRAINT LIMIT_NUM_VENTS CHECK (NOT EXISTS(SELECT *
                                                  FROM VENTAS
                                                  GROUP BY F
                                                  HAVING (SUM(NU) >= 100)));
```
