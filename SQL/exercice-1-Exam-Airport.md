# Exercice 1 SQL Exam Airport

Se tienen las siguientes relaciones en cuanto a la base de datos:

UBICACION(A, PR)\
CP: A

PISTAS(A, PI, L)\
CP: A, PI

REGISTRO(A, PI, F, H, OP, T)\
CP: A, PI

Realizar la implementación de las siguientes consultas a continuación como se pidan en cada una de ellas, ya
puede ser en SQL, en CRT, CRD o en álgebra relacional:

1) Aeropuertos en los que en algún día aterriza al menos un avión de carga de alguna de sus pistas.\
Se necesitan las tablas: REGISTRO\
Realizamos la implementación en SQL ya que no se indica nada.
```sql
SELECT A
FROM REGISTRO
GROUP BY F
HAVING (OP = 'aterrizaje') AND (T = 'Carga');
```

2) Ahora implementa la consulta anterior en álgebra relacional.
Se necesitan las tablas: REGISTRO
```sql
p(A)(s((OP = 'Aterrizaje') ^ (T = 'Carga'))(REGISTRO));
```

3) Aeropuertos en los que en un mismo día aterriza al menos un avión de carga en alguna de sus pistas de más 
de 3000 metros.\
Se necesitan las tablas: REGISTRO, PISTAS
```sql
SELECT A
FROM REGISTROS
GROUP BY F
HAVING (OP = 'Aterrizaje') AND (T = 'Carga') AND PI IN (SELECT PI
                                                        FROM PISTAS
                                                        WHERE (L >= 3000));
```

4) Aeropuertos en los que en algún día aterriza al menos un avión de carga en alguna de sus pistas de más de 3000 
metros. Realizar la implementación de la consulta en álgebra relacional.\
Se necesitan las tablas: REGISTRO, PISTAS
```sql
p(A)(s((OP = 'aterrizaje') ^ (T = 'carga') ^ (L >= 3000))(REGISTRO * PISTAS))
```

5) Aeropuertos en los que en algún día aterriza al menos un avión de carga en alguna de sus pistas de más de 3000
metros. Realizar la implementación de la consulta en CRT.\
Se necesitan las tablas: REGISTRO, PISTAS
```sql
Para (dom(r) = REGISTRO), (dom(p) = PISTAS)
{t1 | ∃r,p ((r.A = t) ^ (r.A = p.A) ^ (r.PI = p.PI) ^ (r.OP = 'aterrizaje') ^ (r.T = 'carga') ^ (p.L >= 3000))}
```

6) Aeropuertos en los que en algún día aterriza al menos un avión de carga en alguna de sus pistas de más de 3000
metros. Realizar la implementación de la consulta en CRD.\
Se necesitan las tablas: REGISTRO, PISTAS
```sql
{<A> | ∃(PI, L, F, H, OP, T)((<A, PI, F, H, OP, T> ∈ REGISTRO) ^ (<A, PI, L> ∈ PISTAS) ^ (OP = 'aterrizaje')
    ^ (T = 'carga') ^ (L >= 3000))}
```

7) Provincias que tienen algún aeropuerto con exactamente dos pistas de aterrizaje.\
Se necesitan las tablas: UBICACION, PISTAS
```sql
SELECT PR
FROM UBICACION
WHERE A IN (SELECT A
            FROM PISTAS
            GROUP BY A
            HAVING (COUNT(PI) = 2));
```

8) Implementación de la consulta anterior, haciendo uso de álgebra relacional.\
Se necesitan las tablas: UBICACION, PISTAS
```sql
P1 = P2 = PISTAS
A = p(A)(s((P1.A = P2.A) ^ (P1.PI ¬= P2.PI))(P1 x P2)) -- Aeropuertos con exactamente dos pistas
    p(PR)(A * UBICACION)
```

9) Aeropuertos en los que todas sus pistas tienen una longitud superior a 3000 metros.\
Se necesitan las tablas: PISTAS
```sql
SELECT A
FROM PISTAS P1
GROUP BY A, PI
HAVING COUNT(PI) = (SELECT COUNT(PI)
                    FROM PISTAS P2
                    WHERE (P1.A = P2.A) AND (P2.PI >= 3000));
```

10) Realizar la consulta anterior en álgebra relacional.\
Se necesitan las tablas: PISTAS
```sql
-- Pistas con longitud superior a 3000 metros
A = p(A, PI)(s((L >= 3000))(PISTAS))
    p(A)(p(A, PI)(PISTAS) / p(A,PI)(A))
```

11) Realizar la consulta anterior en CRT.\
Se necesitan las tablas: PISTAS
```sql
Para (dom(p1) = PISTAS), (dom(p2) = PISTAS)
{t1 | ∃p1 ((p1.A = t) ^ ∀p2((p2.A ¬= p1.A) v ((p2.PI = p1.PI) ^ (p2.L >= 3000))))}
```

12) Realizar la consulta anterior en CRD.\
Se necesitan las tablas: PISTAS.
```sql
{<A> | ∃(PI, L)((<A, PI, L> ∈ PISTAS) ^ ∀(PI`)((<A, PI`, L> ∈ PISTAS) ^ (PI = PI`) ^ (L >= 3000)))}
```

13) Aeropuertos en los que cada día aterriza un avión de carga en una pista.\
Se necesitan las tablas: REGISTRO
```sql
SELECT A
FROM REGISTRO
GROUP BY F
HAVING (OP = 'aterrizaje') AND (T = 'carga');
```

14) Realizar la consulta anterior en álgebra relacional. Cabe destacar que se refiere a todos los días.\
Se necesitan las tablas: REGISTRO
```sql
A = p(A, F)(s((Op = 'aterrizaje') ^ (T = 'carga'))(REGISTRO))
B = p(F)(REGISTRO)
    p(A)(A / B)
```

15. Aeropuertos en los que cada día aterriza un avión de carga en la misma Pista. Implementar la consulta en
álgebra relacional.\
Se necesitan las tablas: REGISTRO
```sql
R1 = R2 = REGISTRO
A = p(R1.A, R1.F)(s((R1.A = R2.A) ^ (R1.P = R2.P) ^ (R1.OP = 'aterrizzaje') ^ (R1.T = 'carga'))(R1 x R2))
B = p(F)(REGISTRO)
    p(A)(A / B)
```
