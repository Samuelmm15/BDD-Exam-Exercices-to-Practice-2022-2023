# Exercice 3 Theme 3 SQL Exam

CANCIONES(CC, I, T, D)\
Significado: La canción con código CC tiene como título T, es interpretada por I y dura D segundos\
CLAVE PRIMARIA(CC)

LISTAS(CL, CC, U)\
Significado: la canción con códico CC es añadida por usuario U a la lista con código CL\
CLAVE PRIMARIA(CL, CC)\
CLAVE AJENA(CC)

ME_GUSTA(CC, U, F)\
Significado: el usuario U ha marcado la canción CC con me gusta en la fecha F\
CLAVE PRIMARIA(CC, U)\
CLAVE AJENA(CC)

Realizar la implementacion de las distintas consultas haciendo uso de SQL:

1) Numero de canciones marcadas hoy con 'me gusta' por el usuario 1:\
Se necesitan las tablas: ME_GUSTA
```sql
SELECT COUNT(*)
FROM ME_GUSTA
WHERE (U = 'USUARIO 1') AND (F = SYSDATE);
```

2) Usuarios a los que no les gusta ninguna cancion del interprete l1.\
Se necesitan las tablas: ME_GUSTA, CANCIONES
```sql
SELECT U
FROM ME_GUSTA M
WHERE NOT EXISTS(SELECT *
                 FROM CANCIONES C
                 WHERE (M.CC = C.CC) AND (I = 'l1'));
```

3) Lista con mayor numero de canciones.\
Se necesitan las tablas: LISTAS
```sql
SELECT CL
FROM LISTAS
GROUP BY CL
HAVING COUNT(*) >= (SELECT MAX(COUNTER)
                    FROM LISTAS L1
                    WHERE COUNTER = (SELECT COUNT(CC)
                                        FROM LISTAS L2
                                        GROUP BY CL
                                        HAVING (L1.CL = L2.CL)));
```

4) Usuarios que en un mismo dia han marcado con 'me gusta' solo canciones de un mismo interprete.\
Se necesitan las tablas: ME_GUSTA, CANCIONES
```sql
SELECT U
FROM ME_GUSTA M
GROUP BY F
HAVING NOT EXISTS(SELECT *
                  FROM CANCIONES C
                  GROUP BY I
                  WHERE (M.CC != C.CC))
```

5) Crea una vista que indique para cada lista su duracion total.
```sql
CREATE VIEW DURACION_CANCIONES
AS (SELECT CL, SUM(D)
    FROM LISTAS NATURAL JOIN CANCIONES
    GROUP BY CL);
```

6) Añade a la lista L1, por el usuario U1, todas las canciones del interprete l1.
```sql
INSERT INTO LISTAS
VALUES (SELECT 'L1', CC, 'U1'
        FROM LISTA NATURAL JOIN CANCIONES
        WHERE (I = 'l1'));   
```

7) Refleja en la base de datos que al usuario U1 ha dejado de gustarle la cancion C1.
```sql
DELETE FROM ME_GUTA
WHERE (U = 'U1') AND (CC = 'C1');
```

8) Limita el tamaño de las listas a un máximo de 25 canciones.
```sql
ALTER TABLE LISTAS
ADD CONSTRAINT LIMITE_25_CANCIONES CHECK ( 25 >= ALL (SELECT COUNT(DISTINCT CC)
                                                      FROM LISTAS
                                                      GROUP BY CL))
```

9) Limita que la duracion total de cualquier lista sea superior a 3 horas.
```sql
ALTER TABLE LISTAS
ADD CONSTRAINT LIMITE_3_HORAS CHECK ( 3600 * 3 >= (SELECT SUM(D)
                                            FROM LISTAS NATURAL JOIN CANCIONES
                                            GROUP BY CL))
```

`NOTA:` cabe destacar que se hace uso de 3600 * 3, ya que la duracion de las canciones se encuentra 
medida en tiempo de segundos o minutos, por tanto si se quiere una duracion de 3 horas, se debera de realizar
la conversion de manera previa de las horas a minutos o segundo a como se encuentra representada la duracion
de las canciones.