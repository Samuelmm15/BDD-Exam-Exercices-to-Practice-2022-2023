# Exercice 1 Theme 3 SQL

Sean los esquemas de relación siguientes: 

PERSONA(DNI, NOMBRE, DOMICILIO )\
CLAVE: (DNI).

PROP_COCHE(MAT, DNI, MARCA, MODELO)\
CLAVE: (MAT)

ACCIDENTE(MAT, FA, DNI_CONDUCTOR, I )\
CLAVE: (MAT, FA) Responder en SQL:

a) Hallar el número de usuarios que tuvieron un accidente en 1983.\
Se necesitan las tablas: 
```sql
SELECT COUNT(MAT)
FROM ACCIDENTE
WHERE FA='1983';
```

`NOTA:` Cabe destacar que para este caso, hemos supuesto que las fechas de los accidentes están en años y,
que además, se trata del atributo de la tabla denominado como FA.

b) Hallar el número de accidentes en que estuvieron implicados los vehículos de "Juan Pérez" durante 1990.\
Se necesitan las tablas: ACCIDENTE, PROP_COCHE, PERSONA
```sql
SELECT COUNT(MAT)
FROM ACCIDENTE
WHERE DNI_CONDUCTOR IN (SELECT DNI
                        FROM PERSONA
                        WHERE NOMBRE='Juan Pérez') AND FA='1990';
```

c) Propietario con más coches.\
Se necesitan las tablas: PROP_COCHE
```sql
SELECT DNI
FROM PROP_COCHE P1
WHERE DNI IN (SELECT DNI
              FROM PROP_COCHE P2
              GROUP BY MAX(COUNT(MAT))
              HAVING P2.DNI=P1.DNI);
```

El intento de esto anterior es bueno, pero no es sintácticamente correcto en SQL, por tanto, la consulta
anterior corregida de manera correcta según ChatGPT es:
```sql
SELECT DNI
FROM PROP_COCHE P1
WHERE (SELECT COUNT(MAT)
       FROM PROP_COCHE P2
       WHERE P2.DNI=P1.DNI
       GROUP BY P2.DNI) > (SELECT COUNT(MAT)
                           FROM PROP_COCHE P3
                           WHERE P3.DNI!=P1.DNI);
```

d) Conductor implicado en el accidente más reciente.\
Se necesitan las tablas: ACCIDENTE
```sql
SELECT DNI_CONDUCTOR
FROM ACCIDENTE
WHERE I=TRUE AND FA IN (SELECT MAX(FA)
                        FROM ACCIDENTE);
```

e) Número medio de accidentes por año.\
Se necesitan las tablas: ACCIDENTE
```sql
SELECT AVG(COUNT(MAT))
FROM ACCIDENTES
GROUP BY YEAR(FA);
```

f) Marca y modelo de coche más común.\
Se necesitan las tablas: PROP_COCHE
```sql
SELECT MARCA, MODELO
FROM PROP_COCHE P1
WHERE NOT EXISTS(SELECT MAX(COUNT(MARCA))
                 FROM PROP_COCHE P2
                 WHERE P2.MAT=P1.MAT) AND NOT EXISTS(SELECT MAX(COUNT(MODELO))
                                                     FROM PROP_COCHE P3
                                                     WHERE P3.MAT=P1.MAT);
```

g) Marca y modelo de coche involucrado en más accidentes.
Se necesitan las tablas: ACCIDENTES, PROP_COCHE
```sql
SELECT MARCA, MODELO
FROM PROP_COCHE P1
WHERE NOT EXISTS(SELECT MAX(COUNT(MAT))
                 FROM ACCIDENTES A1
                 WHERE I=TRUE AND NOT EXISTS(SELECT MAT
                                             FROM ACCIDENTES A2
                                             WHERE I=TRUE AND A2.MAT=A1.MAT AND A2.MAT=P1.MAT));
```