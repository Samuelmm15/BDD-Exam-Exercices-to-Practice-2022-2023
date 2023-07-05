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

Realizar la implementación de las siguientes consultas haciendo uso de cálculo relacional de dominios CRD
(`página 23 de los apuntes los resultados`):

1. Personas que hayan estado contagiadas más de una vez.\
Se necesitan las tablas: CONTAGIOS
```sql
{<DNI> | ∃(FC, FA)((<DNI, FC, FA> ∈ CONTAGIOS) ^ ∃(FC', FA')(<DNI, FC', FA'> ∈ CONTAGIOS) ^ (FC ¬= FC'))}
```

2. Personas que siempre han recibido el mismo tipo de vacuna.\
Se necesitan las tablas: VACUNAS
```sql
{<DNI> | ∃(FV, V, C, MC)((<DNI, FV, V, C, MC> ∈ VACUNAS) ^ ¬∃(FV', V', C', MC')((<DNI, FV', V', C', MC'> ∈ VACUNAS) ^ (V ¬= V')))}
```

3. Centro médico donde se realizó la vacunación por primera vez.\
Se necesitan las tablas: VACUNAS
```sql
{<C> | ∃(FV, V, DNI, MC)((<DNI, FV, V, C, MC> ∈ VACUNAS) ^ ¬∃(DNI', FV', V', C', MC')((<DNI', FV', V', C', MC'> ∈ VACUNAS) ^ (FV` < FV)))}
```

4. Centro médico donde se hayan administrado todos los tipos de vacunas.\
Se necesitan las tablas: VACUNAS
```sql
{<C> | ∃(DNI, FV, V, MC)((<DNI, FV, V, C, MC> ∈ VACUNAS) ^ ∀(DNI', FV', V', C', MC')(<DNI', FV', V', C', MC'> ∉ VACUNAS) V ∃(DNI'', FV'')((<DNI'', FV'', V', C, MC> ∈ VACUNAS)))}
```