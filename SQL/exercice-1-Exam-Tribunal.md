# Exercice 1 Exam SQL Tribunal

Se tiene la siguiente implementación de las distintas relaciones de una base de datos:

CAT -> Categoría del profesor
CUR -> Curso de defensa del trabajo
DNI -> DNI del profesor
P -> DNI del presidente del tribunal
S -> DNI del presidente del tribunal
T -> DNI del secretario del tribunal
TR -> Tipo del trabajo
V -> DNI del vocal del tribunal

PROFESOR (DNI, AR, CAT)\
Clave primaria: DNI

TRIBUNAL (TR, P, S, V, CUR, C)\
Clave primaria: TR, CUR\ 
Clave ajena: P, S, V, TR

TRABAJO (TR, T)\
Clave primaria TR

Realizar la implementación de las distintas consultas de la base de datos haciendo uso de SQL para ello:

1) Profesores que a lo largo del tiempo han ejercido en los tribunales todos los roles (presidente, secretario y 
vocal).\
Se necesitan las tablas: PROFESOR, TRIBUNAL
```sql
SELECT DNI
FROM PROFESOR
WHERE DNI IN (SELECT P
              FROM TRIBUNAL) AND DNI IN (SELECT S
                                         FROM TRIBUNAL) AND DNI IN (SELECT V
                                                                    FROM TRIBUNAL);
```

2) Número medio de tribunales en los que participa el área de LSI por curso académico.\
Se necesitan las tablas: PROFESOR, TRIBUNAL
```sql
SELECT AVG(NUMERO_TRIBUNALES)
FROM PROFESOR P1
WHERE (AR = 'LSI') AND (NUMERO_TRIBUNALES = (SELECT COUNT(*)
                                             FROM TRIBUNAL T
                                             WHERE (T.P = P1.DNI) OR (T.S = P1.DNI) OR (T.V = P1.DNI)
                                             GROUP BY CUR));
```

Esta consulta anterior es incorrecta, por tanto, de manera corregida se tiene que:
```sql
SELECT AVG(numero_tribunales)
FROM (
         SELECT P1.DNI, (SELECT COUNT(*)
                         FROM TRIBUNAL T
                         WHERE (T.P = P1.DNI OR T.S = P1.DNI OR T.V = P1.DNI)
                         GROUP BY T.CUR) AS numero_tribunales
         FROM PROFESOR P1
         WHERE P1.AR = 'LSI'
     ) AS subquery;
```

3) Profesores que han participado en todos los tribunales de trabajos de tipo TFG del curso 2015-16.\
Se necesitan las tablas: TRABAJO, TRIBUNAL, PROFESOR
```sql
SELECT DNI
FROM PROFESOR P
WHERE NOT EXISTS(SELECT *
                 FROM TRIBUNAL T
                 WHERE (CUR = '2015-16') AND ((T.P != P.DNI) OR (T.S != P.DNI) OR (T.V != P.DNI)) AND TR IN (SELECT TR
                                                                                                             FROM TRABAJO
                                                                                                             WHERE (T = 'TFG')));
```

4) Áreas tales que al menos el 50% de sus profesores han participado en algún tribunal en el curso 2015-16.\
Se necesitan las tablas: PROFESOR, TRIBUNAL
```sql
SELECT AR
FROM PROFESOR P 
WHERE (TOTAL_PROFESORES >= (0.5 * COUNT(*))) AND (TOTAL_PROFESORES = (SELECT COUNT(*)
                                                                      FROM TRIBUNAL T
                                                                      WHERE ((P.DNI = T.P) OR (P.DNI = T.S) OR (P.DNI = T.V)) AND (CUR = '2015-16')));
```

El error de la consulta anterior se encuentra en que el 50% es de los profesores del área, no de todos los profesores
que existen. Por tanto, de manera corregida es:
```sql
SELECT AR
FROM PROFESOR P
WHERE (TOTAL_PROFESORES >= (0.5 * (SELECT COUNT(*)
                                   FROM PROFESOR P1
                                   WHERE (P.AR = P1.AR)))) AND (TOTAL_PROFESORES = (SELECT COUNT(*)
                                                                                    FROM TRIBUNAL T
                                                                                    WHERE ((P.DNI = T.P) OR (P.DNI = T.S) OR (P.DNI = T.V)) AND (CUR = '2015-16')));
```

5) Crea una vista que muestre los trabajos de tipo TFM con una calificación de 9 o superior en el curso 2015-16.\
Se necesitan las tablas: TRIBUNAL, TRABAJO
```sql
CREATE VIEW TRABAJO_TFM
AS (SELECT TR
    FROM TRABAJO
    WHERE (T = 'TFM') AND TR IN (SELECT TR
                                 FROM TRIBUNAL
                                 WHERE (CUR = '2015-16') AND (C >= 9)));
```

6) Elimina de la base de datos los tribunales en los que haya algún miembro del área ATC.\
Se necesitan las tablas: TRIBUNAL, PROFESOR
```sql
DELETE FROM TRIBUNAL
WHERE (P IN (SELECT DNI
             FROM PROFESOR
             WHERE (AR = 'ATC'))) OR (S IN (SELECT DNI
                                            FROM PROFESOR
                                            WHERE (AR = 'ATC'))) OR (V IN (SELECT DNI
                                                                           FROM PROFESOR
                                                                           WHERE (AR = 'ATC')));
```

7) Impón la restricción a la base de datos de que los miembros de cada tribunal tienen que ser profesores distintos.\
Se necesitan las tablas: TRIBUNAL, PROFESOR
```sql
ALTER TABLE TRIBUNAL AS T
ADD CONSTRAINT DISTINTOS_PROFESORES CHECK (NOT EXISTS(SELECT *
                                                       FROM PROFESOR AS P
                                                       WHERE (T.P = P.DNI) AND (T.S = P.DNI) AND (T.V = P.DNI)));
```

Esta consulta anterior es incorrecta, ya que no se pueden usar alias para los ALTER TABLE, por tanto, se debe de
implementar esto anterior de la siguiente manera:
```sql
ALTER TABLE TRIBUNAL
ADD CONSTRAINT DISTINTOS_PROFESORES CHECK ( (P != S) AND (P != V) AND (S != V) );
```