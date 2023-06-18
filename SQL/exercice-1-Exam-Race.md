Se tienen las siguientes relaciones de la base de datos del ejercicio:

CDC --> Ciudad donde se realiza la carrera\
D --> Distancia en kilómetros de una carrera\
DNI --> DNI de la persona\
F --> Fecha de la carrera\
IC --> Identificador de carrera\
PR --> Precio en euros de la inscripción en una carrera\
S --> Sexo\
T --> Tiempo en minutos que una persona invirtió en una carrera (Nulo si no la acabó)

PERSONAS(DNI, S)\
CP(DNI)

CARRERAS(IC, CDC, F, D, PR)\
CP(IC)

PARTICIPANTES(DNI, IC, T)\
CP(DNI, IC)\
CA(DNI, IC)

Realizar la implementación de las distintas consultas en SQL:

1) Número de corredores que han terminado la competición l1.\
Se necesitan las tablas: PARTICIPANTES
```sql
SELECT COUNT(*)
FROM PARTICIPANTES
WHERE (IC = 'l1') AND (T IS NOT NULL);
```

2) Mostrar para cada carrera el mejor tiempo realizado.\
Se necesitan las tablas: PARTICIPANTES
```sql
SELECT MIN(T)
FROM PARTICIPANTES
GROUP BY IC;
```

3) Corredores que siempre que participan en una carrera la terminan.\
Se necesitan las tablas: PARTICIPANTES
```sql
SELECT DNI
FROM PARTICIPANTES P1
GROUP BY DNI
HAVING NOT EXISTS(SELECT *
                  FROM PARTICIPANTES P2
                  WHERE (P1.DNI = P2.DNI) AND (P1.IC = P2.IC) AND (P2.T IS NULL));
```

4) Ciudad en la que se ha organizado el mayor número de carreras.\
Se necesitan las tablas: CARRERAS
```sql
SELECT CDC
FROM CARRERAS
WHERE CDC IN (SELECT CDC
              FROM CARRERAS
              GROUP BY CDC
              HAVING COUNT(IC) = (SELECT MAX(TOTAL_RACE)
                                  FROM CARRERAS
                                  GROUP BY CDC
                                  WHERE TOTAL_RACE = COUNT(IC)));
```

5) Carreras tales que al menos el 30% de sus participantes son mujeres.\
Se necesitan las tablas: PARTICIPANTES, PERSONAS
```sql
SELECT IC
FROM PARTICIPANTES P1
GROUP BY IC
HAVING (TOTAL_MUJERES >= (O.30 * COUNT(*))) AND (TOTAL_MUJERES = (SELECT COUNT(*)
                                                                  FROM PARTICIPANTES NATURAL JOIN PERSONAS AS P2
                                                                  GROUP BY IC
                                                                  HAVING (P1.IC = P2.IC) AND (S = 'Mujer')));
```

7) Crea una vista que muestre para cada corredor y cada carrera en la que haya participado el tiempo medio por KM.\
Se necesitan las tablas: PARTICIPANTES, CARRERAS
```sql
CREATE VIEW TIEMPO_MEDIO
AS (SELECT DNI, IC, AVG(MEDIO_TIEMPO)
    FROM PARTICIPANTES P1
    WHERE MEDIO_TIEMPO = (SELECT TOTAL
                          FROM CARRERAS C1
                          WHERE (P1.IC = C1.IC) AND (TOTAL = P1.T * C1.D)));
```

Implementación de esto anterior según Felipe:
```sql
CREATE VIEW TIEMPO_MEDIO
AS (SELECT DNI, IC, (D / T)
    FROM PARTICIPANTES NATURAL JOIN CARREAS);   
```

8) Elimina las personas que no han participado en ninguna carrera.\
Se necesitan las tablas: PARTICIPANTES, PERSONAS
```sql
DELETE FROM PERSONAS
WHERE DNI NOT IN (SELECT DNI
                  FROM PARTICIPANTES);
```

9) Impide que los precios de inscripción en cualquier carrera sean superiores a 50 euros.\
Se necesitan las tablas: CARRERAS
```sql
ALTER TABLE CARRERAS
ADD CONSTRAINT LIMITE_INSCRIPCION CHECK NOT EXISTS(SELECT *
                                                   FROM CARRERAS
                                                   WHERE (PR > 50));
```

10) Fuerza que los precios de incripción en cualquier carrera dada sean distintos.\
Se necesitan las tablas: CARRERAS
```sql
ALTER TABLE CARRERAS
ADD CONSTRAINT DISTINTOS_PRECIOS CHECK NOT EXISTS(SELECT *
                                                  FROM CARRERAS C1
                                                  GROUP BY IC
                                                  HAVING NOT EXISTS(SELECT *
                                                                   FROM CARRERAS C2
                                                                   GROUP BY IC
                                                                   HAVING (C1.IC = C2.IC) AND (C1.PR = C2.PR)));
```