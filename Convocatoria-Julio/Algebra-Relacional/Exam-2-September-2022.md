# 2 Septembre Exam Agencia de Vehículos de Alquiler

Relacion de las distintas tablas usadas en la base de datos:

COCHES(M, CAT)
SIGNIFICADO: El vehículo con matrícula M es de la categoría CAT.

DISPONIBILIDAD(M, CO, PR)
SIGNIFICADO: El coche con matrícula M está disponible en la oficina CO a un precio de PR euros diarios.
CP: (M, CO) CA: (M)

ALQUILERES(DNI, CO, M, N, F)
SIGNIFICADO: El cliente con dni DNI, ha alquilado en la oficina CO el vehículo con matrícula M, en la fecha F, durante N
días.
CP: (M, F) CA: (M, CO)

Realizar la implementación de las siguientes consultas haciendo uso de álgebra relacional ():

1. Vehículos que están disponibles simultáneamente en al menos 2 oficinas.\
Se necesitan las tablas: DISPONIBILIDAD
```sql
A = B = DISPONIBILIDAD
p(A.M)(s((A.M = B.M) ^ (A.CO ¬= B.CO))(A x B))
```

2. Clientes que han alquilado los vehículos M1 y M2 en una misma oficina.\
Se necesitan las tablas: ALQUILERES
```sql
A = p(DNI, CO)(s((M = 'M1'))(ALQUILERES))
B = p(DNI, CO)(s((M = 'M2'))(ALQUILERES))
    p(DNI)(A * B)
```

3. Oficina que alquila el vehículo M1 con menor precio.\
Se necesita la tabla: DISPONIBILIDAD
```sql
A = p(CO, PR)(s((M = 'M1'))(DISPONIBILIDAD))    -- Oficinas que alquilan el vehículo M1
B = p(CO, PR)(s((M = 'M1'))(DISPONIBILIDAD))
C = p(CO, PR)(s(A.PR > B.PR)(A x B))    -- Todos los vehículos de mayor valor
    p(CO)(p(CO, PR)(DISPONIBLIDAD) - C) -- Vehículos con menor precio en una determinada oficina
```

La consulta anterior es casi correcta, pero, se encuentra un error en que no tuvimos en cuenta que las oficinas
deben de ser distintas a su vez, por tanto la consulta corregida es:
```sql
A = p(CO, PR)(s((M = 'M1'))(DISPONIBILIDAD))   
B = p(CO, PR)(s((M = 'M1'))(DISPONIBILIDAD))
C = p(CO, PR)(s((A.PR > B.PR) ^ (A.CO ¬= B.CO))(A x B))    
    p(CO)(p(CO, PR)(DISPONIBLIDAD) - C) 
```

4. Clientes que han alquilado al menos un vehículo de cada categoría.\
Se necesitan lsa tablas: ALQUILERES, COCHES
```sql
A = p(DNI, M, CAT)(ALQUILERES * COCHES) -- Vehículos junto con sus correspendientes categorías
B = p(CAT)(COCHES)
    p(DNI)(A / B)
```

5. Clientes que han alquilado en alguna oficina todos los vehículos de alguna categoría.\
Se necesitan las tablas: COCHES, ALQUILERES
```sql
A = p(DNI, M, CO, CAT)(ALQUILERES * COCHES) -- Coches junto con sus correspondientes categorías
B = p(CAT)(COCHES) -- Todas las categorías
    p(DNI)(A / B)
```

Esta consulta implementada anteriormente es incorrecta, la consulta corregida de manera correcta es:
```sql
p(DNI)(p(DNI, CAT)(ALQUILERES * COCHES) - p(DNI, CAT)(p(DNI)(ALQUILERES) x p(DNI, M, CAT)(ALQUILERES * COCHES) - p(DNI, M, CAT)(ALQUILERES * COCHES)))
```