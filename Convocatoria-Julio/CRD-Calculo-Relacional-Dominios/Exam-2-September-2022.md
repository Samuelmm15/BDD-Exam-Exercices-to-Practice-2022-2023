# 2 September Exam Agencia de Vehículos de Alquiler

Relación de las distintas tablas usadas en la base de datos:

COCHES(M, CAT)
SIGNIFICADO: El vehículo con matrícula M es de la categoría CAT.

DISPONIBILIDAD(M, CO, PR)
SIGNIFICADO: El coche con matrícula M está disponible en la oficina CO a un precio de PR euros diarios.
CP: (M, CO) CA: (M)

ALQUILERES(DNI, CO, M, N, F)
SIGNIFICADO: El cliente con dni DNI, ha alquilado en la oficina CO el vehículo con matrícula M, en la fecha F, durante N
días.
CP: (M, F) CA: (M, CO)

Realizar la implementación de las siguientes consultas haciendo uso de Cálculo relacional de tuplas CRT
(`soluciones en la página 13`):

1. Clientes que han alquilado al menos 2 vehículos distintos en una misma oficina.\
Se necesitan las tablas: ALQUILERES
```sql
{<DNI> | ∃(CO, M, N, F)((<DNI, CO, M, N, F> ∈ ALQUILERES) ^ ∃(CO', M', F', N')((<DNI, CO', M', N', F'> ∈ ALQUILERES) ^ (M ¬= M') ^ (CO = CO'))}
```

En la solución de los apuntes la consulta la expresa de manera distintas a esta, ya que se hace uso de un no existe,
pero, es lo mismo que negarlo más tarde aquella condición que queremos que no sea verdadera.

2. Vehículos que sólo están disponibles en la oficina O1.\
Se necesitan las tablas: DISPONIBILIDAD
```sql
{<M> | ∃(CO, PR)((<M, CO, PR> ∈ DISPONIBILIDAD) ^ (CO = 'O1') ^ ¬∃(CO', PR')((<M, CO', PR'> ∈ DISPONIBILIDAD) ^ (CO ¬= CO)))}
```

3. Vehículo más económico en la oficina O1.
Se necesitan las tablas: DISPONIBILIDAD
```sql
{<M> | ∃(CO, PR)((<M, CO, PR> ∈ DISPONIBILIDAD) ^ (CO = 'O1') ^ ∀(M', CO', PR')((<M', CO', PR'> ∈ DISPONIBILIDAD) ^ (M' ¬= M) ^ (PR <= PR') ^ (CO' = CO)}
```

La consulta anterior es correcta, pero es `más óptima` y, permite que esta sea más lógica en cuanto al lenguaje natural,
por tanto, de manera corregida es:
```sql
{<M> | ∃(PR)((<M, 'O1', PR> ∈ DISPONIBILIDAD) ^ ¬∃(M', PR')((<M', 'O1', PR'> ∈ DISPONIBILIDAD) ^ (PR' < PR)))}
```

4. Clientes que han alquilado al menos un vehículo en cada oficina.\
Se necesitan las tablas: ALQUILERES
```sql
{<DNI> | ∃(CO, M, N, F)((<DNI, CO, M, N, F> ∈ ALQUILERES) ^ ∀(CO', M', N', F')∃(CO'', M'', N'', F'')((<DNI, CO', M', N', F'> ∈ ALQUILERES) ^ 
    (<DNI, CO'', M'', N'', F''> ∈ ALQUILERES) ^ (CO'' = CO) ^ (M'' = M) ^ (CO'' ¬=  CO)))}
```

5. Oficinas que en un mismo día han alquilado al menos un vehículo de cada una de las categorías de las que disponen.\
Se necesitan las tablas: ALQUILERES, COCHES
```sql
{<CO> | ∃(DNI, M, N, F)((<DNI, CO, M, N, F> ∈ ALQUILERES) ^ ∀(DNI', M', N', F', CAT')∃(M'', CAT'')((<DNI', M', N', F'> ∈ ALQUILERES) ^ (<M', CAT'> ∈ COCHES)
    ^ (<M'', CAT''> ∈ COCHES) ^ (M'' = M) ^ (M' ¬= M) ^ (F' = F) ^ (CAT' ¬= CAT))}
```