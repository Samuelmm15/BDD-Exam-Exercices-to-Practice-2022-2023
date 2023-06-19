# Exercice 2 Exam CRD Maquina

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

Realizar la implementación de las distintas consultas haciendo uso de cálculo relacional de dominios:

1) Personas que tiene cuenta en la máquina M1 pero no en la M2.\
Se necesitan las tablas: CUENTA
```sql
{<DNI> | ∃(CM, U, F)((<DNI, CM, U, F> ∈ CUENTA) ^ (CM = 'M1') ^ ∃(CM`, F`)((<DNI, CM`, U, F`> ∈ CUENTA) ^ (CM` ¬= 'M2'))}
```

2) Personas que sólo tiene cuenta en la máquina M1.\
Se necesitan las tablas: CUENTA
```sql
{<DNI> | ∃(CM, U, F)((<DNI, CM, U, F> ∈ CUENTA) ^ (CM = 'M1') ^ ¬∃(CM`, F`)((<DNI, CM`, U, F`> ∈ CUENTA) ^ (CM` ¬= 'M1')))}
```

3) Personas que tienen cuentas en todas las máquinas de 'Linux'.\
Se necesitan las tablas: CUENTA, MAQUINA
```sql
{<DNI> | ∃(CM, U, F, SO)((<DNI, CM, U, F> ∈ CUENTA) ^ ∀(CM`, T)((<CM`, T, SO> ∉ MAQUINA) v (<CM`, T, SO> ∈ MAQUINA) ^ (SO = 'Linux') ^ (CM` = CM))}
```

4) Personas tales que todas sus cuentas están abiertas en una misma máquina.\
Se necesitan las tablas: CUENTA
```sql
{<DNI> | ∀(F, F`)∃(CM, CM`, U)((<DNI, CM, U, F> ∈ CUENTA) ^ (<DNI, CM`, U, F``> ∈ CUENTA) ^ (CM = CM`)}
```

5) Personas que han abierto cuentas en todas las máquinas de un mismo tipo.\
Se necesitan las tablas: CUENTA, MAQUINA
```sql
{<DNI> | ∃(CM, U, F, T, SO)((<DNI, CM, U, F> ∈ CUENTA) ^ ∀(CM`, CM``)∃(T`)(((<CM`, T, SO> ∉ MAQUINA) ^ (<CM``, T`, SO> ∉ MAQUINA)) V ((<CM`, T, SO> ∈ MAQUINA) ^ (<CM``, T`, SO> ∈ MAQUINA) ^ (CM` ¬= CM``) ^ (T = T`) ^ (CM = CM`) V (CM = CM``)))}
```