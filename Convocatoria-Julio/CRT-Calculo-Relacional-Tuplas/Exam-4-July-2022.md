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

Realizar la implementación de las siguientes consultas haciendo uso de cálculo relacional de tuplas CRT
(`página 22 de los apuntes los resultados`):

1. Personas que hayan estado contagiadas más de una vez.\
Se necesitan las tablas: CONTAGIOS
```sql
Para dom(C1) = CONTAGIOS, dom(C2) = CONTAGIOS
{t1 | ∃(C1, C2)((t = C1[DNI]) ^ (C1[DNI] = C2[DNI]) ^ (C1[FC] ¬= C2[FC]))}
```

2. Personas que siempre han recibido el mismo tipo de vacuna.\
Se necesitan las tablas: VACUNAS
```sql
Para dom(V1) = VACUNAS, dom(V2) = VACUNAS
{t1 | ∃(V1, V2)((V1[DNI] = t) ^ (V1[DNI] = V2[DNI]) ^ (V1[FV] ¬= V2[FV]) ^ (V1[V] = V2[V]))}
```

Esta consulta anterior es `incorrecta`, mejor realizar la implementación de la siguiente manera:
```sql
Para dom(V1) = VACUNAS, dom(V2) = VACUNAS
{t1 | ∃(V1)((V1[DNI] = t) ^ ¬∃(V2)((V1[DNI] = V2[DNI]) ^ (V1[V] ¬= V2[V])))}
```

3. Centro médico dónde se realizó la vacunación por primera vez.\
Significado: centro médico con la menor fecha de vacunación posible.\
Se necesitan las tablas: VACUNAS
```sql
Para dom(V1) = VACUNAS, dom(V2) = VACUNAS
{t1 | ∃(V1)((V1[C] = t) ^ ∀(V1)¬∃(V2)((V1[DNI] = V2[DNI]) ^ (V2[FV] < V1[FV])))}
```

La consulta anterior es `aproximadamente correcta` , pero, esta es la corrección:
```sql
Para dom(V1) = VACUNAS, dom(V2) = VACUNAS, dom(V3) = VACUNAS
{t1 | ∃(V1)((V1[C] = t) ^ ∀(V2)(∃(V3)((V1[C] = V2[C]) ^ (V1[C] = V3[C]) ^ (V2[FV] < V3[FV]))))}
```

4. Centro médico dónde se hayan administrado todos los tipos de vacunas.\
Se necesitan las tablas: VACUNAS
```sql
Para dom(V1) = VACUNAS, dom(V2) = VACUNAS, dom(V3) = VACUNAS
{t1 | ∃(V1)((V1[C] = t) ^ ∀(V2, V3)((V1[C] = V2[C]) ^ (V1[C] = V2[C]) ^ (V2[V] = V3[V])))}
```

La consulta anterior es `aproximadamente correcta`, pero, esta es la corrección:
```sql
Para dom(V1) = VACUNAS, dom(V2) = VACUNAS, dom(V3) = VACUNAS
{t1 | ∃(V1)((V1[C] = t) ^ ∀(V2)(∃(V3)(V1[C] = V2[C]) ^ (V1[C] = V2[C]) ^ (V2[V] = V3[V])))}
```

5. Centro médico dónde se hayan administrado todos los tipos de vacunas en una misma fecha.\
Se necesitan las tablas: VACUNAS
```sql
Para dom(V1) = VACUNAS, dom(V2) = VACUNAS, dom(V3) = VACUNAS
{t1 | ∃(V1)((V1[C] = t) ^ ∀(V2)(∃(V3)((V1[C] = V2[C]) ^ (V1[C] = V3[C]) ^ (V2[V] = V3[V]) ^ (V2[F] = V3[F]))))}
```

La consulta anterior es `aproximadamente correcta`, pero, esta es la corrección:
```sql
Para dom(V1) = VACUNAS, dom(V2) = VACUNAS, dom(V3) = VACUNAS
{t1 | ∃(V1)((V1[C] = t) ^ ∀(V2)((V1[F] ¬= V2[F]) v ∃(V3)((V1[C] = V2[C]) ^ (V1[C] = V3[C]) ^ (V2[V] = V3[V]) ^ (V1[F] = V3[F]))))}
```