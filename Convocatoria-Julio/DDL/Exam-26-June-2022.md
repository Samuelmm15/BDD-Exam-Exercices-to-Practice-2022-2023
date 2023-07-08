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

Realizar la implementación de las distintas consultas haciendo uso de SQL-DDL (`soluciones apuntes
de Pablo Notion`):

3. Incrementa un 10% la longitud de las pistas de aquellos aeropuertos con una única pista.\
Se necesitan las tablas: PISTAS
```sql
UPDATE PISTAS
SET L = L + (0.1 * L)
WHERE (COUNT(PI) = 1)
```