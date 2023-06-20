# Exercice 1 Theme 3 Exam Ventas SQL

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
Se necesitan las tablas: TIENDAS, ARTICULOS
```sql
SELECT CT
FROM TIENDAS T
WHERE NOT EXISTS(SELECT *
                 FROM ARTICULOS A
                 WHERE (A.CA = T.CA) ^ (A.CAT = 'C1'));
```

3) Personas que han comprado en una misma tienda más de 10 unidades de productos iguales o distintos.\
Se necesitan las tablas: VENTAS
```sql
SELECT DNI
FROM VENTAS V1
WHERE (NU >= 10) OR ((TOTAL_UNITS >= 10) AND (TOTAL_UNITS = (SELECT SUM(NU)
                                                             FROM VENTAS V2
                                                             WHERE (V1.DNI = V2.DNI) AND (V1.CT = V2.CT))));
```

4) Tiendas que en un mismo día han vendido artículos de todas las categorías.\
Se necesitan las tablas: VENTAS, ARTICULOS
```sql
SELECT CT
FROM VENTAS NATURAL JOIN ARTICULOS
GROUP BY F
HAVING COUNT(DISTINCT CAT) = (SELECT COUNT(DISTINCT CAT)
                             FROM ARTICULOS); 
```

5) Tiendas que tales que al menos el 40% de sus articulos cuestan menos de 30€.\
Se necesitan las tablas: TIENDAS
```sql
SELECT CT
FROM TIENDAS T1
GROUP BY CT
HAVING (ARTICULOS_MENOS_30 >= (0.40 * COUNT(*)) AND (ARTICULOS_MENOS_30 = (SELECT COUNT(*)
                                                                          FROM TIENDAS T2
                                                                          GROUP BY CT
                                                                          HAVING (T1.CT = T2.CT) AND (T1.PR <= 30)))); 
```

6) Elimina en la tabla VENTAS la clave primaria.
```sql
ALTER TABLE VENTAS
DROP PRIMARY KEY;
```

7) Incrementa en un 10% el precio de todos los articulos de la tienda T1 que pertenecen a la categoria C1.\
Se necesitan las tablas: TIENDAS, ARTICULOS
```sql
UPDATE TIENDAS
SET PR = (PR + (0.10 * PR))
WHERE (CT = 'C1') AND (CT = 'T1');
```

8) Crea una tabla que almacene todas las ventas anteriores al 1 de enero de 2020 de la base de datos.
```sql
CREATE TABLE VENTAS_1_ENERO AS (SELECT *
       FROM VENTAS
       WHERE (F <= '01-JAN-2020'));
```

9) Impide que los precios de un mismo articulo puedan variar segun la tienda.
Se necesitan las tablas: TIENDAS
```sql
ALTER TABLE TIENDAS
ADD CONSTRAINT LIMITE_PRECIO CHECK NOT EXISTS(SELECT *
                                                FROM TIENDAS T1
                                                WHERE NOT EXISTS(SELECT *
                                                                 FROM TIENDAS T2
                                                                 WHERE (T1.CT != T2.CT) AND (T1.CA = T2.CA) AND (T1.PR != T2.PR)));
```

10) Impide que una tienda pueda vender de un mismo producto mas de 100 unidades en un mismo dia.
Se necesitan las tablas: VENTAS
```sql
ALTER TABLE VENTAS
ADD CONSTRAINT LIMITE_VENTA CHECK (NOT EXISTS(SELECT *
                                              FROM VENTAS
                                              GROUP BY CT, CA, F
                                              WHERE (SUM(NU) > 100)));
```