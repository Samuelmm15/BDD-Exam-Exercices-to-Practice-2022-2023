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
(`soluciones en la página 21`):

1. Clientes que han alquilado al menos 2 vehículos distintos en una misma oficina.\
Se necesitan las tablas: ALQUILERES
```sql
Para dom(A1) = ALQUILERES, dom(A2) = ALQUILERES
{t1 | ∃(A1)((A1[DNI] = t[DNI]) ^ ∃(A2)((A1[DNI] = A2[DNI]) ^ (A1[CO] = A2[CO]) ^ (A1[M] ¬= A2[M])))}
```

2. Vehículos que sólo están disponibles en la oficina O1.\
Se necesitan las tablas: DISPONIBILIDAD
```sql
Para dom(D1) = DISPONIBILIDAD, dom(D2) = DISPONIBILIDAD
{t1 | ∃(D1)((D1[M] = t[M]) ^ (D1[CO] = 'O1') ^ ¬∃(D2)((D1[M] = D2[M]) ^ (D1[CO] ¬= D2[CO])))}
```

3. Clientes que siempre alquilan vehículos de la categoría C1.\
Se necesitan las tablas: ALQUILERES, COCHES
```sql
Para dom(A) = ALQUILERES, dom(C) = COCHES
{t1 | ∀(A)((A[DNI] = t[DNI]) ^ ∃(C)((C[M] = A[M]) ^ (C[CAT] = 'C1')))}
```

4. Clientes que han alquilado al menos un vehículo en cada oficina.\
Se necesitan las tablas: ALQUILERES
```sql
Para dom(A1) = ALQUILERES, dom(A2) = ALQUILERES, dom(A3) = ALQUILERES
{t1 | ∃(A1)((A1[DNI] = t[DNI]) ^ ∀(A2)∃(A3)((A2[CO] = A3[CO]) ^ (A1[CO] = A3[CO]) ^ (A1[M] = A3[M]) ^ (A1[DNI] = A3[DNI])))}
```

5. Oficinas que en un mismo día han alquilado al menos un vehículo de cada una de las categorías de las que disponen.\
Se necesitan las tablas: ALQUILERES, COCHES, DISPONIBILIDAD
```sql
Para dom(A1) = ALQUILERES, dom(A2) = ALQUILERES, dom(C1) = COCHES, dom(C2) = COCHES, dom(D) = DISPONIBILIDAD
{t1 | ∃(A1)((A1[CO] = t[CO]) ^ ∀(A2, C1)∃(D, C2)((A2[M] = A1[M]) ^ (D[M] = A1[M]) ^ (D[CO] = A1[CO]) ^ (C1[M] = A1[M]) ^ (C2[M] = A1[M]) ^ (C2[CAT] = C1[CAT])))}
```