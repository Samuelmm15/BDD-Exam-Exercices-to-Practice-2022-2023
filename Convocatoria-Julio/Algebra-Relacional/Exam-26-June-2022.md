# 26 June Exam Actividad Aeropuertos

Relación de las distintas tablas usadas en la base de datos:

UBICACIÓN (A, PR)\
SIGNIFICADO: El acropuerto A está ubicado en la província PR.\
CP: (A)

PISTAS (A, PI, L)\
SIGNIFICADO: La pista PI del aeropuerto A tiene una longitud de L metros.\
СР: (A, PI) СA: (A)

REGISTRO (A, PI, F, II, OP, T' )\
SIGNIFICADO: En la pista PI del aeropuerto A, el día F a la hora H se realizó una operación OP para un avión de tipo T.\
CP: (A, PI, P, H) CA: (A, PI)

Realizar la implementación de las distintas consultas haciendo uso de Álgebra relacional:

1. Aeropuertos en los que algún día aterriza al menos un avión de carga en alguna de sus pistas.\
Se necesitan las tablas: REGISTRO
```sql
p(A)(s((OP = 'aterrizaje') ^ (T = 'carga'))(REGISTRO))
```

2. Aeropuertos en los que todas sus pistas tienen una longitud superior a 3000 metros.\
Se necesitan las tablas: PISTAS
```sql
A = p(A, PI)(s(L < 3000)(PISTAS))
    p(A)(p(A, PI)(PISTAS) - A)
```

3. Aeropuertos en los que cada día aterriza al menos un avión de carga en alguna de sus pistas.\
Se necesitan las tablas: REGISTRO
```sql
-- R = A
-- A = F
-- T = PI
A = p(A)(REGISTRO) x p(PI, F)(REGISTRO)
B = A - p(A, PI, F)(REGISTRO)
C = P(A, F)(REGISTRO) - p(A, F)(B)
    p(A)(C)
```

4. Aeropuertos que algún día en cada una de sus pistas sólo operaron aviones de pasajeros.
Se necesitan las tablas: REGISTRO
```sql
A = p(A, PI, F)(s(T = 'pasajeros')(REGISTRO)) -- Pistas dónde operaron aviones de pasajeros
B = p(A, PI, F)(REGISTRO) - A -- Pistas dónde operaron más aviones a parte de la de pasajeros
C = p(A, PI, F)(REGISTRO) - B -- Pistas dónde solo operaron aviones de pasajeros
    p(A)(p(A, PI, F)(REGISTRO) / C)
```