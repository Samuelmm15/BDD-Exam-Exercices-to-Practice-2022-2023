# Exercice Algebra relacional Exam

Se tiene las siguientes relaciones de tablas:

Tabla de contenidos:
- AR: área del profesor.
- C: calificación del trabajo.
- CAT: categoría del profesor.
- CUR: curso de defensa del trabajo.
- DNI: dni del profesor.
- P: dni del presidente del tribunal.
- S: dni del presidente del tribunal.
- T: dni del secretario del tribunal.
- TR: tipo del trabajo.
- V: dni del vocal de tribunal.

PROFESOR(DNI, AR, CAT)\
CP: DNI

TRIBUNAL(TR, P, S, V, CUR, C)\
CP: TR, CUR
CA: P, S, V, TR

TRABAJO(TR, T)\
CP: TR

Realizar la implementación de las distintas consultas haciendo uso de álgebra relacional:

1) Profesores que en el curso 15-16 han sido presidentes en 2 o más tribunales.\
Se necesitan las tablas: TRIBUNAL, PROFESORES\
Significado: Todos los profesores que 
```sql
TR1 = TR2 = TRIBUNAL
p(P)(s((TR1.TR ¬= TR2.TR) ^ (CUR = '15-16') ^ (TR1.P = TR2.P))(TR1 x TR2))
```

2) Áreas en las que ninguno de sus profesores ha sido secretario en tribunales del curso 2015-16.\
Se necesitan las tablas: TRIBUNAL, PROFESOR\
Significado: áreas donde todos sus profesores no ha sido secretario de todos los tribunales.
```sql
-- Profesores del curso 2015-16 que han sido secretarios en tribunales
A = p(A, DNI)(PROFESOR) / p(T)(s((CUR = '2015-16'))(TRIBUNAL))
-- Profesores que pertenecen a un mismo área y han sido secretarios en tribunales
B = p(A, DNI)(PROFESOR) / p(A)(PROFESOR)
    p(A)(PROFESOR - B)
```

Según la corrección que existe sobre esto es de la siguiente manera esto anterior:
```sql
A = p(S)(s(CUR = '2015-16')(TRIBUNAL))
B = p(AR)(s(DNI=A.S)(PROFESOR x A))
    p(AR)(PROFESOR - B)
```

3) Tribunales que en el curso 2015-16 todos sus miembros pertenecían a un mismo área.\
Se necesitan las tablas: TRIBUNAL, PROFESORES
```sql
A = p(AR, DNI, TR)(s((CUR = '2015-16') ^ (TRIBUNAL.P = PROFESORES.DNI))(TRIBUNAL x PROFESORES))
B = p(AR, DNI, TR)(s((CUR = '2015-16') ^ (TRIBUNAL.S = PROFESORES.DNI))(TRIBUNAL x PROFESORES))
C = p(AR, DNI, TR)(s((CUR = '2015-16') ^ (TRIBUNAL.V = PROFESORES.DNI))(TRIBUNAL x PROFESORES))
    p(TR)(A * B * C)
```

4) Áreas que han participado en todos los tribunales de trabajos tipo TFG en el curso 2015-16.\
Se necesitan las tablas: PROFESORES, TRIBUNAL, TRABAJO
```sql
-- Tribunales de trabajos de tipo TFG del curso 2015-16
A = p(TR, P, S, V)(s((T = 'TFG') ^ (CUR = '2015-16'))(TRIBUNAL * TRABAJO))
-- Áreas que han participado en los trabajos de la consulta anterior
B = p(AR, TR)(s((PROFESOR.DNI = TRBUNAL.P) V (PROFESOR.DNI = TRIBUNAL.S) V (PROFESOR.DNI = TRIBUNAL
    .V))(PROFESORES X TRIBUNAL))
    p(AR)(B/A)
```
