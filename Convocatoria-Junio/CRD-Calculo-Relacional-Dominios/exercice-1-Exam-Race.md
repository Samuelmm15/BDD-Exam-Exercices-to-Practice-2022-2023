# Exercice 1 CRD Exam Race

Se tienen las siguientes relaciones de la base de datos del ejercicio:

CDC --> Ciudad donde se realiza la carrera\
D --> Distancia en kilómetros de una carrera\
DNI --> DNI de la persona\
F --> Fecha de la carrera\
IC --> Identificador de carrera\
PR --> Precio en euros de la inscripción en una carrera\
S --> Sexo\
T --> Tiempo en minutos que una persona invirtió en una carrera (Nulo si no la acabó)

PERSONAS(DNI, S)\
CP(DNI)

CARRERAS(IC, CDC, F, D, PR)\
CP(IC)

PARTICIPANTES(DNI, IC, T)\
CP(DNI, IC)\
CA(DNI, IC)

Realizar la implementación de las distintas consultas en CRD:

1) Personas que han corrido en al menos 2 carreras celebradas en un mismo día.\
Se necesitan las tablas: PARTICIPANTES, CARRERAS
```sql
{<DNI> | ∃(IC, IC`, T, T`, CDC, CDC`, F, D, D`, PR, PR`)((<DNI, IC, T> ∈ PARTICIPANTES) ^ 
    (<DNI, IC`, T`> ∈ PARTICIPANTES) ^ (<IC, CDC, F, D, PR> ∈ CARRERAS) ^ (<IC`, CDC`, F, D`, PR`> ∈ CARRERAS))}
```

2) Personas que siempre corren la misma distancia.\
Se necesitan las tablas: PARTICIPANTES, CARRERAS
```sql
{<DNI> | ∃(IC, T, CDC, F, D, PR)((<DNI, IC, T> ∈ PARTICIPANTES) ^ ∀(IC`, IC``)((<DNI, IC`, T> ∉ PARTICIPANTES)
    v ((<DNI, IC`, T> ∈ PARTICIPANTES) ^ (<IC`, CDC, F, D, PR> ∈ CARRERAS) ^ (<IC``, CDC, F, D, PR> ∈ CARRERAS)
        ^ (IC` = IC``))}
```

3) Personas que han ganado alguna carrera.\
Se necesitan las tablas: PARTICIPANTES
```sql
{<DNI> | ∃(IC, T)((<DNI, IC, T> ∈ PARTICIPANTES) ^ ∀(DNI`)∃(T`)((<DNI`, IC, T`> ∉ PARTICIPANTES) v 
    ((<DNI`, IC, T`> ∈ PARTICIPANTES) ^ (T <= T`))))}
```

4) Personas que han ganado todas las carreras en las que han participado.\
Se necesitan las tablas: PARTICIPANTES
```sql
{<DNI> | ∃(IC, T)((<DNI, IC, T> ∈ PARTICIPANTES) ^ ∀(DNI`, IC)∃(T`)((<DNI`, IC, T`> ∉ PARTICIPANTES) v 
    ((<DNI`, IC, T`> ∈ PARTICIPANTES) ^ (T <= T`))))}
```

5) Personas que han ganado todas las carreras en las que han participado celebradas en una misma ciudad.\
Se necesitan las tablas: PARTICIPANTES, CARRERAS
```sql
{<DNI> | ∃(IC, T, CDC, F, D, PR)((<DNI, IC, T> ∈ PARTICIPANTES) ^ ∀(DNI`, IC)∃(T`)((<DNI`, IC, T`> ∉ PARTICIPANTES) v 
    ((<DNI`, IC, T`> ∈ PARTICIPANTES) ^ (<IC, CDC, F, D, PR> ∈ CARRERAS) ^ ∃(CDC`)((<IC, CDC`, F, D, PR> ∈ CARRERAS) 
        ^ (T <= T`))))}
```