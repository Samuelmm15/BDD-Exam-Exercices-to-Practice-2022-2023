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

Realizar la implementación de las siguientes consultas haciendo uso de álgebra relacional (`página 19 de los apuntes
los resultados`):

1. Personas que han recibido alguna dosis de la vacuna en un centro médico ubicado en su municipio de residencia.\
Se necesitan las tablas: VACUNAS, PERSONAS
```sql
A = p(DNI, MP)(PERSONAS)
B = p(DNI, MC)(VACUNAS)
    p(DNI)(A * B)
```

2. Personas que han recibido dosis de distintas vacunas.\
Se necesitan las tablas: VACUNAS
```sql
A = p(DNI, V)(VACUNAS)
B = P(DNI, V)(VACUNAS)
    p(DNI)(s((A.V ¬= B.V))(A x B))
```

Esta consulta anterior también se puede implementar de la siguiente manera:
```sql
A = B = VACUNAS
p(A.DNI)(s((A.DNI = B.DNI) ^ (A.V ¬= B.V))(A x B))
```

3. Persona de mayor edad que ha estado contagiada.\
Se necesitan las tablas: PERSONAS, CONTAGIOS
```sql
-- ESTO NO ES COMPLETAMENTE CORRECTO
A = p(DNI, E)(PERSONA * CONTAGIOS)
B = p(DNI, E)(PERSONA * CONTAGIOS)
    p(DNI)(s(A.E >= B.E)(A * B))
```

Esta implementación anterior no sería correcta, esta implementación a continuación si que sería correcta:
```sql
-- IMPLEMENTACIÓN CORRECTA
A = p(DNI, E)(PERSONA * CONTAGIOS)
B = p(DNI, E)(PERSONA * CONTAGIOS)
C = p(DNI)(s((A.E < B.E))(A x B))
    p(DNI)(CONTAGIOS) - C
```

4. Personas que han recibido al menos una dosis de cada tipo de vacuna.\
Se necesitan las tablas: VACUNAS
```sql
p(DNI)(p(DNI, V)(VACUNAS) / p(V)(VACUNAS))
```

5. Personas que han recibido alguna dosis de vacunas en cada uno de los centros médicos del mismo municipio.\
Se necesitan las tablas: VACUNAS
```sql
p(DNI)(p(DNI, V, C, MC)(VACUNAS) / p(C, MC)(VACUNAS))
```
