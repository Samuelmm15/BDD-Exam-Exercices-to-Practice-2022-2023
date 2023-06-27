# Exercice 1 Exam Electric Boogaloo

Sean las siguientes tablas que se encuentran dentro de la base de datos:

CANCIONES(CC, I, T, D)\
Significado: La canción con código CC tiene como título T, es interpretada por I y dura D segundos\
CLAVE PRIMARIA(CC)

LISTAS(CL, CC, U)\
Significado: La canción con códico CC es añadida por usuario U a la lista con código CL\
CLAVE PRIMARIA(CL, CC)\
CLAVE AJENA(CC)

ME_GUSTA(CC, U, F)\
Significado: El usuario U ha marcado la canción CC con me gusta en la fecha F\
CLAVE PRIMARIA(CC, U)\
CLAVE AJENA(CC)

Realizar las siguientes consultas sobre la base de datos haciendo uso de SQL:

1. Número de canciones marcadas hoy con me "gusta" por el usuario 1.\
Se necesitan las tablas: ME_GUSTA
```sql
SELECT COUNT(*)
FROM ME_GUSTA
WHERE U=1 AND F=SYSDATE;
```

2. Usuarios a los que no les gusta ninguna canción del intérprete l1.\
Se necesitan las tablas: CANCIONES, ME_GUSTA
```sql
SELECT U
FROM ME_GUSTA M
WHERE NOT EXISTS(SELECT *
                 FROM CANCIONES C
                 WHERE C.CC=M.CC AND I=l1);
```

3. Lista con mayor número de canciones.\
Se necesitan las tablas: LISTAS
```sql
SELECT CL
FROM LISTAS
GROUP BY CL
HAVING COUNT(CC) = (SELECT MAX(AUX)
                    FROM LISTAS
                    WHERE AUX = (SELECT COUNT(CC)
                                 FROM LISTAS
                                 GROUP BY CL));
```

4. Usuarios que en un mismo día han marcado con 'me gusta' solo canciones de un mismo intérprete.\
Se necesitan las tablas: ME_GUSTA, CANCIONES
```sql
SELECT U
FROM ME_GUSTA NATURAL JOIN CANCIONES AS NJ
GROUP BY F
HAVING I = ALL (SELECT I
              FROM CANCIONES C
              WHERE NJ.I=C.NJ);
```

5. Crea una vista que indique para cada lista su duración total.\
Se necesitan las tablas: LISTAS
```sql
CREATE VIEW DURACION_CANCIONES
AS (SELECT CL, SUM(D)
    FROM LISTA NATURAL JOIN CANCIONES
    GROUP BY CL;
```

6. Añade a la lista L1, por el usuario U1, todas las canciones del intérprete l1.
Se necesitan las tablas: LISTAS, CANCIONES
```sql
INSERT INTO LISTAS
VALUES (SELECT 'L1', CC, 'U1'
        FROM LISTAS NATURAL JOIN CANCIONES
        WHERE I='l1');
```

7. Refleja en la base de datos que el usuario U1 ha dejado de gustarle la canción C1.\
Como pone que refleje en la base de datos, supongo que se refiere a eliminar la tupla de la tabla ME_GUSTA.
```sql
DELETE FROM ME_GUSTA
WHERE (U='U1') AND (CC='C1');
```

8. Limita el tamaño de las Listas a un máximo de 25 canciones.
```sql
ALTER TABLE LISTAS
ADD CONSTRAINT LIMITE_25_CANCIONES CHECK 25 >= ALL (SELECT COUNT(DISTINCT CC) 
																										FROM LISTAS
																										GROUP BY CL);
```

9. Impide que la duración total de cualquier lista sea superior a 3 horas.
```sql
ALTER TABLE LISTAS
ADD CONSTRAINT LIMITE_3_HORAS CHECK 3600 * 3 >= ALL (SELECT SUM(D)
                                                     FROM LISTAS NATURAL JOIN CANCIONES
                                                     GORUP BY CL);
```