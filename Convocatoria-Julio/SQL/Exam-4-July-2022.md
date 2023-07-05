# 4 July Exam Servicio Regional de Salud

Relación de las distintas tablas usadas en la base de datos:

PERSONAS (DNI, E, S, MP)\
SIGNIFICADO: La persona con DNI y edad E tiene sexo S y reside en el municipio MP.\
CLAVE PRIMARIA: (DNI)

VACUNAS (DNI, FV, V, C, MC)\
SIGNIFICADO: La persona con DNI ha recibido una dosis de la vacuna V en la fecha FV en el centro médico C\
ubicado en el municipio MC.\
CLAVE PRIMARIA: (DNI, FV) \
CLAVE AJENA: (DNI)

CONTAGIOS (DNI, FC, FA)\
SIGNIFICADO: La persona con DNIestuvo contagiada entre las fechas FC y FA.\
CLAVE PRIMARIA: (DNI, FC)

Realizar la implementación de las siguientes consultas haciendo uso de SQL
(`página 25 de los apuntes los resultados`):

1. Número de personas que han superado un contagio.\
Se necesitan las tablas: CONTAGIOS
```sql
SELECT COUNT(DISTINCT DNI)
FROM CONTAGIOS
WHERE FA IS NOT NULL;
```

2. Para cada municipio mostrar el número de personas residentes que no han recibido ninguna dosis de vacuna.\
Se necesitan las tablas: PERSONAS, VACUNAS
```sql
SELECT MP, COUNT(DISTINCT DNI)
FROM PERSONAS
WHERE DNI NOT IN (SELECT *
                  FROM VACUNAS);
```

Esta consulta anterior es `casi correcta`, la consulta corregida es:
```sql
SELECT MP, COUNT(DISTINCT DNI)
FROM PERSONAS
WHERE DNI NOT IN (SELECT *
                  FROM VACUNAS)
                  GROUP BY MP;
```

3. Personas que se han contagiado por primera vez después de haber recibido alguna dosis de la vacuna.\
Se necesitan las tablas: VACUNAS, CONTAGIOS
```sql
SELECT DNI
FROM VACUNAS V1
WHERE DNI IN (SELECT *
              FROM CONTAGIOS C1
              WHERE (C1.DNI = V1.DNI) AND (V1.FV < C1.FC));
```

La consulta anterior la estaba haciendo mucho más complicada de lo que era, la corrección es:
```sql
SELECT DNI
FROM VACUNAS NATURAL JOIN CONTAGIOS
WHERE FC > FV;
```

4. Centro médico dónde se ha administrado la mayor cantidad de dosis de vacunas.\
Se necesitan las tablas: VACUNAS
```sql
SELECT C
FROM VACUNAS
GROUP BY C
HAVING MAX(COUN(V));
```

Para poder obtener la mayor cantidad de dosis de vacunas de un centro médico se hace uso de la siguiente solución:
```sql
SELECT C, COUNT(*) AS CANTIDAD_DOSIS
FROM VACUNAS
GROUP BY C
ORDER BY CANTIDAD_DOSIS DESC 
LIMIT 1;
```

5. Centro médico donde al menos el 30% de las personas vacunadas han sido hombres.\
Se necesitan las tablas: PERSONAS, VACUNAS
```sql
SELECT C
FROM VACUNAS NATURAL JOIN PERSONAS
WHERE (COUNT(S = 'HOMBRE') > (0.30 * COUNT(*))); 
```

La consulta anterior de manera corregida es:
```sql
SELECT C
FROM VACUNAS NATURAL JOIN PERSONAS
WHERE S = 'M'
GROUP BY C
HAVING COUNT(DISTINCT DNI) >= 0.3 * (SELECT COUNT(DISTINCT DNI)
                                     FROM VACUNAS);
```