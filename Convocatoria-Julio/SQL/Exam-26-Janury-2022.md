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

Realizar la implementación de las distintas consultas haciendo uso de SQL (`soluciones apuntes de Pablo
`):

1. Número de productos de la tienda T1 con un precio inferior a 30€.\
Se necesitan las tablas: TIENDAS
```sql
SELECT COUNT(*)
FROM TIENDAS
WHERE (CT = 'T1') AND (PR < 30);
```

2. Tiendas que no disponen de artículos de la categoría C1.
Se necesitan las tablas: TIENDAS, ARTÍCULOS
```sql
SELECT CT
FROM TIENDAS NATURAL JOIN ARTÍCULOS AS TA
WHERE CA NOT IN (SELECT CA
                 FROM ARTÍCULOS
                 WHERE (CAT = 'C1'));
```

3. Personas que han comprado en una misma tienda más de 10 unidades de productos iguales o distintos.\
Se necesitan las tablas: VENTAS
```sql
SELECT DNI
FROM VENTAS
GROUP BY DNI, CT
HAVING (SUM(NU) >= 10);
```

4. Tiendas que en un mismo día han vendido artículos de todas las categorías.\
Se necesitan las tablas: VENTAS, ARTÍCULOS
```sql
SELECT CT
FROM VENTAS NATURAL JOIN ARTÍCULOS
GROUP BY CT, F, CAT
HAVING COUNT(DISTINCT CAT) = (SELECT COUNT(DISTINCT CAT)
                              FROM ARTÍCULOS);
```

`NOTA:` Tener cuidado con el `DISTINCT`, ya que, nos puede ser útil para poder resolver e implementar aquellas
consultas que queremos comparar categorías y elementos que son distintos, por tanto, esto nos puede ser útil de
manera considerable, tener en cuenta esto es muy importante.

5. Tiendas que al menos el 40% de sus artículos cuestan menos de 30 euros.\
Se necesitan las tablas: TIENDAS
```sql
SELECT CT
FROM TIENDAS T1
WHERE (PR < 30)
GROUP BY CT, CA
HAVING (COUNT(*) >= 0.40 * (SELECT COUNT(CA)
                            FROM TIENDAS T2
                            WHERE (T1.CT = T2.CT)));
```