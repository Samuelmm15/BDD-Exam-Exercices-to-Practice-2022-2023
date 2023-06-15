# Ejercicio 2 Hoja de problemas SQL

Considérese el esquema relacional siguiente: 

VIVE( nombre-persona, calle, ciudad-persona )\
CP: (nombre-persona)

TRABAJA( nombre-persona, nombre-compañía, salario )\
CP: (nombre-persona, nombre-compañía) 

LOCALIZACIÓN( nombre-compañía, ciudad-compañía )\
CP: (nombre-compañía)

SUPERVISA( nombre-persona, nombre-supervisor, nombre-compañía )\
CP: (nombre-persona, nombre-compañía)\
CA: (nombre-persona, nombre-compañía), (nombre-supervisor, nombre-compañía)

Constrúyase una expresión en SQL para cada una de las siguientes consultas:

1) Hallar el nombre de todas las personas que trabajan para la compañía C1.\
Se necesitan las tablas: TRABAJA
```sql
SELECT NP
FROM TRABAJA
WHERE NC='C1';
```

2) Hallar el nombre de todas las personas que viven en la misma ciudad en que se halla la compañía para
la que trabajan.
Se neceistan las tablas: TRABAJA, LOCALIZACION, VIVE
```sql
SELECT NP
FROM VIVE NATURAL JOIN TRABAJA VT1
WHERE CIUDADP IN (SELECT CIUDADC
                  FROM LOCALIZACION
                  WHERE NC=VT1.NC);
```

3) Hallar todas las personas que viven en la misma ciudad que su supervisor.\
Se necesitan las tablas: VIVE, SUPERVISA
```sql
SELECT NP
FROM SUPERVISA S1
WHERE (CIUDADP = (SELECT CIUDADP
                 FROM VIVE
                 WHERE S1.NP = VIVE.NP)) AND (CIUDADS = (SELECT CIUDADP
                                                       FROM VIVE
                                                       WHERE S1.NS = VIVE.NP)) AND (CIUDADP=CIUDADS);
```

4) Hallar todas las personas que no trabajan para la compañía C1.
Se necesitan las tablas: TRABAJA
```sql
SELECT NP
FROM TRABAJA
WHERE (NC != 'C1');
```

5) Hallar todas las personas que ganan más que cualquier empleado de la compañía C1.\
Se necesitan las tablas: TRABAJA
```sql
SELECT NP
FROM TRABAJA
WHERE SALARIO >= ALL (SELECT MAX(SALARIO)
                      FROM TRABAJA
                      WHERE (NC='C1'));
```

La consulta anterior estaba realizada de manera incorrecta, ya que, me falto poner el MAX, ya que esto nos
permite obtener el máximo de los empleados de la compañía 1, por tanto, se produce la comparación
de manera correcta con el empleado que gana más de la compañía 1.

6) Hallar todas las personas que ganan más que el salario promedio de las personas que trabajan en la
misma compañía.\
Se necesitan las tablas: TRABAJA
```sql
SELECT NP
FROM TRABAJA T1
WHERE SALARIO >= ALL (SELECT AVG(SALARIO)
                      FROM TRABAJO T2
                      WHERE T1.NC = T2.NC);
```

7) Hallar la compañía que tiene más empleados.
Se necesitan las tablas: TRABAJA
```sql
SELECT NC
FROM TRABAJA T1
GROUP BY NC
HAVING COUNT(*) = (SELECT MAX(EMP_COUNT)
                   FROM (SELECT COUNT(*) AS EMP_COUNT
                         FROM TRABAJA
                         GROUP BY NC));
```

Esta consulta en un comienzo estaba implementada de manera incorrecta, pero, ahora está correctamente implementada.
Realiza la operación que debe de realizar.

8) Hallar las compañías que pagan más, en promedio,que el sueldo medio de la compañía C1.
Se necesitan las tablas: TRABAJA
```sql
SELECT NC
FROM TRABAJA
GROUP BY NC
HAVING SALARIO >= (SELECT AVG(SALARIO)
                   FROM TRABAJA
                   GROUP BY NC
                   HAVING NC='C1');
```


