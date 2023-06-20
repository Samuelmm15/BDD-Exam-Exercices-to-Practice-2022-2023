# Exercice 1 Exam Electric Boogaloo

Sean las siguientes tablas que se encuentran dentro de la base de datos:

CANCIONES(CC, I, T, D)\
Significado: La canción con código CC tiene como título T, es interpretada por I y dura D segundos\
CLAVE PRIMARIA(CC)

LISTAS(CL, CC, U)\
Significado: La canción con códico CC es añadida por usuario U a la lista con código CL\
CLAVE PRIMARIA(CL, CC)\
CLAVE AJENA(CC)

ME_GUSTA(CC, U, F)\
Significado: El usuario U ha marcado la canción CC con me gusta en la fecha F\
CLAVE PRIMARIA(CC, U)\
CLAVE AJENA(CC)

Realizar las siguientes consultas sobre la base de datos haciendo uso de álgebra relacional:

1. Canciones del intérprete l1 que están en la lista L1.\
Se necesitan las tablas: CANCIONES, LISTAS
```sql
p(T)(s((I = 'l1') ^ (CL = 'L1'))(LISTAS * CANCIONES))
```

2. Canciones en L1 marcadas por el usuario U1.
Se necesitan las tablas: ME_GUSTA, LISTAS
```sql
p(T)(s((CL = 'L1') ^ (U = 'U1'))(LISTAS * ME_GUSTA * CANCIONES))
```

3. Usuarios que han añadido las canciones C1 y C2 a una misma lista.
Se necesitan las tablas: LISTAS
```sql
A = p(U, CL)(s((CC = 'C1'))(LISTAS))
B = p(U,CL)(s((CC = 'C2'))(LISTAS))
    p(U)(A ∩ B)
```

4. Usuarios que han marcado con 'me gusta' TODAS las canciones del intérprete l1 que están en la lista L1.\
Se necesitan las tablas: CANCIONES, ME_GUSTA, LISTA
```sql
p(U)((p(U, CC)(s((CL = 'L1'))(LISTAS * ME_GUSTA))) / (p(CC)(s((I = 'l1'))(CANCIONES))))
```

5. Usuarios que han añadido en alguna lista todas las canciones de algún intérprete.
Se necesitan las tablas: LISTAS, CANCIONES
```sql
A = p(CC, I)(CANCIONES)
B = p(CL, U, CC, I)(CANCIONES * LISTAS)
    p(U)(B / A)
```