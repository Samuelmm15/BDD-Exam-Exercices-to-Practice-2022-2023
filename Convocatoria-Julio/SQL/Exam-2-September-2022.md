# 2 September Exam Agencia de Vehículos de Alquiler

Relacion de las distintas tablas usadas en la base de datos:

COCHES(M, CAT)
SIGNIFICADO: El vehículo con matrícula M es de la categoría CAT.

DISPONIBILIDAD(M, CO, PR)
SIGNIFICADO: El coche con matrícula M está disponible en la oficina CO a un precio de PR euros diarios.
CP: (M, CO) CA: (M)

ALQUILERES(DNI, CO, M, N, F)
SIGNIFICADO: El cliente con dni DNI, ha alquilado en la oficina CO el vehículo con matrícula M, en la fecha F, durante N
días.
CP: (M, F) CA: (M, CO)

Realizar la implementación de las siguientes consultas haciendo uso de SQL ():

1. Número de vehículos de la categoría C1 disponibles en la oficina O1.\
Se necesitan las tablas: DISPONIBILIDAD, COCHES
```sql
SELECT COUNT(*)
FROM DISPONIBILIDAD NATURAL JOIN COCHES
WHERE (CAT = 'C1') AND (CO = 'O1');
```

2. Importe medio total pagado por los clientes de la oficina O1.\
Se necesitan las tablas: DISPONIBILIDAD, ALQUILERES
```sql
SELECT AVG(PR)
FROM DISPONIBILIDAD NATURAL JOIN ALQUILERES
WHERE (CO = 'O1');
```

3. Clientes que siempre alquilan vehículos de la categoría C1.\
Se necesitan las tablas: ALQUILERES, COCHES
```sql
SELECT DNI
FROM ALQUILERES NATURAL JOIN COCHES
GROUP BY DNI, CAT
HAVING (CAT != 'C1');
```

4. Oficinas que en un mismo día han alquilado todos sus vehículos disponibles.\
Se necesitan las tablas: ALQUILERES, DISPONIBILIDAD
```sql
SELECT CO
FROM ALQUILERES A1
WHERE (COUNT(DISTINCT M) = (SELECT COUNT(*)
                  FROM DISPONIBILIDAD D1
                  WHERE (D1.CO = A1.CO)))
GROUP BY CO, F;
```

La consulta anterior es `casi correcta`, pero, se encuentra un error en el hecho de la agrupación, la consulta
corregida es:
```sql
SELECT CO
FROM ALQUILERES A1
GROUP BY CO, F
HAVING (COUNT(DISTINCT M) = (SELECT COUNT(*)
                             FROM DISPONIBILIDAD D1
                             WHERE (D1.CO = A1.CO)));
```

5. Oficinas tales que al menos el 40% de sus vehículos disponibles son de una misma categoría.\
Se necesitan las tablas: COCHES, DISPONIBILIDAD
```sql
SELECT CO
FROM DISPONIBILIDAD NATURAL JOIN COCHES AS DC
WHERE (COUNT(M) >= 0.40 * (SELECT COUNT(*)
                           FROM DISPONIBILIDAD AS D1
                           WHERE (D1.CO = DC.CO)))
GROUP BY CO, CAT;
```

La consulta anterior es `casi correcta`, pero, se encuentra un error en el hecho de la agrupación, la consulta
corregida es:
```sql
SELECT CO
FROM DISPONIBILIDAD NATURAL JOIN COCHES AS DC
GROUP BY CO, CAT
HAVING (COUNT(M) >= 0.40 * (SELECT COUNT(*)
                            FROM DISPONIBILIDAD AS D1
                            WHERE (D1.CO = DC.CO)));
```