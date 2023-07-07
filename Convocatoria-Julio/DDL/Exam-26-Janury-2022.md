# 26 Janury Exam Cadena de Tiendas

Relación de las distintas tablas usadas en la base de datos:

ARTÍCULOS (CA, CAT)\
SIGNIFICADO: El artículo CA es decategoría CAT.\
CLAVE PRIMARIA: (CA)

TIENDAS(CT, CA, PR)\
SIGNIFICADO: La tienda con código CT dispone del articulo CA a un precio de PR euros.\
CLAVE PRIMARIA: (CT, CA)\
CLAVE AJENA: (CA)

VENTAS(DNI, CT, CA, NU, F)\
SIGNIFICADO: La persona con n i DNI. ha comprado en la tienda CT, NU unidades del articulo CA, en la fecha F.\
CLAVE PRIMARIA: (DNI, CT, CA, F)\
CLAVE AJENA: (CT, CA)

Realizar la implementación de las distintas consultas haciendo uso de SQL-DDL  (`soluciones apuntes de Pablo
`):

1. Elimina en la tabla VENTAS la clave primaria.\
Se necesita la tabla: VENTAS
```sql
ALTER TABLE VENTAS
DROP PRIMARY KEY;
```

2. Incrementa en un 10% el precio de todos los artículos de la tienda T1 que pertenecen a la categoría C1.\
Se necesita la tabla: TIENDAS
```sql
UPDATE TIENDAS
SET PR = PR + (0.1 * PR)
WHERE (CT = 'T1') AND (CA IN (SELECT CA
                              FROM ARTÍCULOS
                              WHERE (CAT = 'C1')));
```

3. Crea una tabla que almacene todas las ventas anteriores al 1 de enero de 2020 de la base de datos.\
```sql
CREATE TABLE VENTAS_ANTERIORES AS (SELECT *
                                   FROM VENTAS
                                   WHERE (F < '1-01-22'));
```

4. Impide que los precios de un mismo artículo pueda variar según la tienda.\
Se necesita la tabla: TIENDAS
```sql
ALTER TABLE TIENDAS
ADD CONSTRAINT MISMO_PRECIO CHECK NOT EXISTS(SELECT *
                                             FROM TIENDAS T1
                                             WHERE NOT EXISTS(SELECT *
                                                              FROM TIENDAS T2
                                                              WHERE (T1.CT != T2.CT) AND (T1.CA = T2.CA) AND (T1.PR != T2.PR)));
```

5. Impide que una tienda pueda vender de un mismo producto más de 100 unidades en un mismo día.\
Se necesita la tabla: VENTAS
```sql
ALTER TABLE VENTAS
ADD CONSTRAINT LIMITE_PRODUCTO CHECK NOT EXISTS(SELECT *
                                                FROM VENTAS V1
                                                GROUP BY F
                                                HAVING NOT EXISTS(SELECT *
                                                                 FROM VENTAS V2
                                                                 GROUP BY F
                                                                 HAVING (V1.CT = V2.CT) AND (V1.CA = V2.CA) AND (SUM(V1.NU) > 100)));
```

Esta consulta anterior es `incorrecta`, ya que, se podría hacer de manera mucho más sencilla de la siguiente manera:
```sql
ALTER TABLE VENTAS
ADD CONSTRAINT LIMITE_PRODUCTO CHECK ( SUM(NU) <= 100 );
```