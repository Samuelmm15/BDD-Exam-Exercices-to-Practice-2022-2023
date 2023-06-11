# Exercice 1 Theme 1 Exam Carreras

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

Realizar la implementación de las distintas consultas para dicha base de datos haciendo uso de álgebra relacional:

1. Personas que han corrido la carrera l1 pero no la l2.\
Se necesitan las tablas: CARRERAS, PARTICIPANTES
```sql
A = p(DNI)(s((IC = 'l1'))(PARTICIPANTES * CARRERAS))
B = p(DNI)(s((IC = 'l2'))(PARTICIPANTES * CARRERAS))
    p(DNI)(A - B)
```

2. Personas que han corrido una misma distancia en varias ciudades.\
Se necesitan las tablas: CARRERAS, PARTICIPANTES
```sql
A = p(DNI, D)(PARTICIPANTES * CARRERAS)
B = p(DNI, D)(PARTICIPANTES * CARRERAS)
    p(DNI)(A ∩ B)
```

Cabe destacar que en el caso anterior, como dice que se produce en varias ciudades y no en la misma ciudad, 
suponemos que al no determinar que sean la misma ciudad, se produce que la propia consulta determina que deben
de ser varias ciudades, y no tiene por qué ser la misma, si la distancia de ambas coincide.

3. Persona que ganó la carrera l1.\
Se necesitan las tablas: PARTICIPANTES\
Significado para poder entender esto anterior: Obtener el participante que menos tiempo empleó para un carrera de
TODOS los participantes.
```sql
A = p(DNI, T)(s(IC = 'l1')(PARTICIPANTES)) -- Todos los participantes de la carrera l1
B = p(DNI, T)(s((IC = 'l1') ^ (PARTICIPANTES.T > A.T))(PARTICIPANTES x A)) -- Obtenemos todos los participantes con el mayor tiempo
    p(DNI)(A - B) --Obtenemos el único que tiene menor tiempo que todos
```