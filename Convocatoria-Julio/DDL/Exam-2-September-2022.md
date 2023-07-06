# 2 September Exam Agencia de Vehículos de Alquiler

Relación de las distintas tablas usadas en la base de datos:

COCHES(M, CAT)
SIGNIFICADO: El vehículo con matrícula M es de la categoría CAT.

DISPONIBILIDAD(M, CO, PR)
SIGNIFICADO: El coche con matrícula M está disponible en la oficina CO a un precio de PR euros diarios.
CP: (M, CO) CA: (M)

ALQUILERES(DNI, CO, M, N, F)
SIGNIFICADO: El cliente con dni DNI, ha alquilado en la oficina CO el vehículo con matrícula M, en la fecha F, durante N
días.
CP: (M, F) CA: (M, CO)

Realizar la implementación de las siguientes consultas haciendo uso de SQL (DDL) (`soluciones en la página
14`):

1. Crea una vista que indique para cada cliente y cada vehículo alquilado, el coste total abonado en la fecha de 
alquiler.\
Se necesitan las tablas: ALQUILERES, DISPONIBILIDAD
```sql
CREATE VIEW TOTAL_COST AS (SELECT DNI, M, (N * PR)
                            FROM ALQUILERES NATURAL JOIN DISPONIBILIDAD
                            GROUP BY DNI, M, F);
```

2. Bonifica en un 10% el precio de alquiler de los vehículos de la categoría C1.\
Se necesitan las tablas: DISPONIBILIDAD, COCHES
```sql
UPDATE DISPONIBILIDAD
SET PR = PR * 0.9
WHERE M IN (SELECT M
            FROM COCHES
            WHERE (CAT = 'C1'));
```

3. Impón que a partir de ahora el número mínimo de días de alquiler de los vehículos de la categoría C1 sea de 2 días
en cualquier oficina.\
Se necesitan las tablas: ALQUILERES, COCHES
```sql
ALTER TABLE ALQUILERES
ADD CONSTRAINT NAME CHECK NOT EXISTS(SELECT *
                                     FROM ALQUILERES NATURAL JOIN COCHES
                                     WHERE (CAT = 'C1') AND (F >= SYSDATE()) AND (N < 2));
```

4. Evita que los precios de alquiler de un mismo vehículo puedan variar según la oficina.\
Se necesitan las tablas: DISPONIBILIDAD
```sql
CREATE ASSERTION PRECIOS_VEHICULOS CHECK NOT EXISTS(SELECT *
                                                    FROM DISPONIBILIDAD D1
                                                    WHERE NOT EXISTS(SELECT *
                                                                     FROM DISPONIBILIDAD D2
                                                                     WHERE (D1.CO != D2.CO) AND (D1.PR != D2.PR) AND (D1.M = D2.M)));
```

5. Impide que un mismo vehículo pueda volver a alquilarse mientras permace alquilado.
```sql
-- En este caso se necesita la implementación de un trigger para ello
```