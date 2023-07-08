# 26 June Exam Actividad Aeropuertos

Relación de las distintas tablas usadas en la base de datos:

UBICACIÓN (A, PR)\
SIGNIFICADO: El acropuerto A está ubicado en la província PR.\
CP: (A)

PISTAS (A, PI, L)\
SIGNIFICADO: La pista PI del aeropuerto A tiene una longitud de L metros.\
СР: (A, PI) СA: (A)

REGISTRO (A, PI, F, II, OP, T' )\
SIGNIFICADO: En la pista PI del aeropuerto A, el día F a la hora H se realizó una operación OP para un avión de tipo T.\
CP: (A, PI, P, H) CA: (A, PI)

Realizar la implementación de las distintas consultas haciendo uso de SQL (`soluciones apuntes
de Pablo Notion`):

1. Provincias que tienen algún aeropuerto con exactamente 2 pistas de aterrizaje.\
Se necesitan las tablas: UBICACIÓN, PISTAS
```sql
SELECT PR
FROM UBICACIÓN NATURAL JOIN PISTAS
WHERE (COUT(PI) = 2);
```

2. Número medio de operaciones de aterrizaje diarias en el aeropuerto de los Rodeos.\
Se necesitan las tablas: REGISTROS
```sql
SELECT AVG(COUNT(*))
FROM REGISTROS
WHERE (OP = 'aterrizaje') AND (A = 'Los Rodeos')
GROUP BY F;
```

3. Aeropuertos con mayor número de operaciones realizadas en total el día '25-06-17'.
Se necesitan las tablas: REGISTRO
```sql
SELECT A
FROM REGISTROS AS R1
WHERE COUT(*) = (SELECT MAX(COUNT(*))
                  FROM REGISTROS AS R2
                  WHERE (F = '25-06-17'));
```

4. Aeropuertos tales que al menos el 70% de sus operaciones las realizan aviones de carga.\
Se necesitan las tablas: REGISTRO
```sql
SELECT A
FROM REGISTROS AS R1
WHERE (T = 'carga') AND (COUNT(*) >= 0.70 * (SELECT COUNT(*)
                                             FROM REGISTROS AS R2
                                             WHERE (R2.A = R1.A)));
```