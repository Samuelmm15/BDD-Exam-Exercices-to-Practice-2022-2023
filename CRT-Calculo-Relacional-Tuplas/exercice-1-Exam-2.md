# Exercice 1 CRT EXAM 2

Se tienen las siguientes tablas dentro de la base de datos:

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
CP(DNI, IC)
CA(DNI, IC)

Realizar la implementación de las distintas consultas para dicha base de datos haciendo uso de cálculo
relacional de tuplas:

1) Personas que han corrido al menos 2 carrearas celebradas en un mismo día.\
Se necesitan las tablas: PARTICIPANTES, CARRERAS
```sql
Para (dom(p1) = PARTICIPANTES), (dom(p2) = PARTICIPANTES), (dom(c1) = CARRERAS), (dom(c2) = CARRERAS)
{t1 | ∃p1, ∃p2 ∃c1 ∃c2 ((p1.DNI = t) ^ (p2.DNI = p1.DNI) ^ (p1.IC ¬= p2.IC) ^ (p1.IC = c1.IC) ^ (p2.IC = c2.IC)
    ^ (c1.F = c2.F))}
```

2) Personas que siempre corren la misma distancia.\
Se necesitan las tablas: PARTICIPANTES, CARRERAS
```sql
Para (dom(p1) = PERSONAS), (dom(p2) = PERSONAS), (dom(c1) = CARRERAS), (dom(c2) = CARRERAS)
{t1 | ∃p1 ∃p2 ∀c1 ∀c2 ((p.DNI = t) ^ (p1.DNI = p2.DNI) ^ (p1.IC = c1.IC) ^ (p2.IC = c2.IC) ^ (c1.D = c2.D))}
```

3) Personas que han ganado alguna carrera.\
Se necesitan las tablas: PARTICIPANTES
```sql
Para (dom(p1) = PARTICIPANTES), (dom(p2) = PARTICIPANTES)
{t1 | ∃p1 ∀p2((p1.DNI = t) ^ (p1.IC ^ p2.IC) ^ (p1.T >= p2.T))}
```