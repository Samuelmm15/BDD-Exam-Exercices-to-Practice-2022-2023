# Exercice 1 Exam Máquina Álgebra relacional

Se tienen las siguientes relaciones de la base de datos:

CM -> Código de la máquina.
DNI -> DNI del usuario.
F -> Fecha de apertura de la cuenta.
SO -> Sistema Operativo.
T -> Tipo de máquina.
U -> Usuario.

MAQUINA(CM, T, SO)\
CP: CM\
Significado: La máquina CM es de tipo T y tiene instalado el sistema operativo SO.

CUENTA(DNI, CM, U, F)\
CP: CM, U\
CA: CM\
Significado: La persona con dni DNI, ha abierto la cuenta U en la máquina CM, en la fecha F.

Realizar la implementación de las distintas consultas haciendo uso de álgebra relacional:

1) Personas que han abierto alguna cuenta en una máquina de tipo T1.\
Se necesitan las tablas: CUENTA
```sql
p(DNI)(s((T = 'T1'))(CUENTA))
```

2) Personas que sólo tienen cuentas en máquinas con Linux.\
Se necesitan las tablas: CUENTA, MÁQUINA
```sql
-- Máquinas que tienen no tienen Linux
A = p(CM)(s((SO ¬= 'Linux'))(MAQUINA))
-- Personas con cuentas en máquinas distintas de Linux
B = p(DNI, CM)(CUENTA * A)
    p(DNI)(p(DNI, CM)(CUENTA) - (B))
```

3) Persona que ha abierto la cuenta más reciente en la máquina M1.\
Se necesitan las tablas: CUENTA
```sql
C1 = C2 = CUENTA
p(DNI)(s((C1.F <= C2.F) ^ (C1.CM = 'M1') ^ (C2.CM = 'M1'))(C1 x C2))
```

4) Personas que en un mismo día han abierto cuenta en todas las Máquinas.\
Se necesitan las tablas: CUENTA, MAQUINA
```sql
C1 = C2 = CUENTA
-- Personas que han abierto cuenta el mismo día.
A = p(DNI, CM)(s((C1.F = C2.F) ^ (C1.DNI ¬= C2.DNI))(C1 x C2))
    -- Personas que han abierto cuenta en todas las máquinas el mismo día
    p(DNI)(p(DNI,CM)(A) / p(CM)(MAQUINA))
```

5) Personas que han abierto cuentas en todas las máquinas de un mismo tipo.\
Se necesitan las tablas: CUENTA, MAQUINA
```sql
M1 = M2 = MAQUINA
-- Máquinas del mismo tipo
A = p(CM)(s((M1.T = M2.T) ^ (M1.CM ¬= M2.CM))(M1 x M2))
-- Cuentas abiertas por personas en todas las máquinas de un mismo tipo
    p(DNI)(p(DNI, CM)(CUENTA) / p(CM)(A))
```