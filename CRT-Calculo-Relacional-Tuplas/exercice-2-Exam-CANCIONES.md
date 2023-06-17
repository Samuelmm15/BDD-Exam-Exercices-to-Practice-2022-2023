# Exercice 2 EXAM CANCIONES

Se tiene la siguiente implementación de las distintas relaciones de la base de datos:

CANCIONES(CC, I, T, D)\
Significado: La canción con código CC tiene como título T, es interpretada por I y dura D segundos\
CLAVE PRIMARIA(CC)

LISTAS(CL, CC, U)\
Significado: la canción con códico CC es añadida por usuario U a la lista con código CL\
CLAVE PRIMARIA(CL, CC)\
CLAVE AJENA(CC)

ME_GUSTA(CC, U, F)\
Significado: el usuario U ha marcado la canción CC con me gusta en la fecha F\
CLAVE PRIMARIA(CC, U)\
CLAVE AJENA(CC)

Realizar la implementacion de las distintas consultas haciendo uso de calculo relacional de tuplas:

1) Canciones que han sido añadidas en dos listas por el mismo usuario.\
Se necesitan las tablas: LISTAS
```sql
Para (dom(l1) = LISTAS), (dom(l2) = LISTAS)
{t1 | ∃l1,l2((l1.CC = t) ^ (l1.CC = l2.CC) ^ (l1.CL ¬= l2.CL) ^ (l1.U = l2.U))}
```

2) Usuarios a los que no les gusta ninguna cancion de la lista l1.\
Se necesitan las tablas: LISTAS, ME_GUSTA
```sql
Para (dom(l) = LISTAS), (dom(m) = ME_GUSTA)
{t1 | ∃l((l.U = t) ^ (l.CL = 'l1') ^ ∀m((m.CC ¬= l.CC) ^ (l.U = m.U)))}
```

3) Canción más corta del intérprete 1.\
Se necesitan las tablas: CANCIONES
```sql
Para (dom(c1) = CANCIONES), (dom(c2) = CANCIONES)
{t1 | ∃c1((c1.CC = t) ^ (c1.I = 'I1') ^ ∀c2((c2.CC ¬= c1.CC) ^ (c1.D <= C2.D)))}
```

4) Usuarios que han marcado con 'me gusta' todas las canciones de alguna lista.\
Se necesitan las tablas: LISTAS, ME_GUSTA
```sql
Para (dom(l) = LISTA), (dom(m) = ME_GUSTA), (dom(m1) = ME_GUSTA)
{t1 | ∃l, l1((l.U = t) ^ ∀m((m.U = l.U) ^ (l1.U = l.U) ^ (l1.CL = l.CL) ^ (l.CC = m.CC)))}
```

5) Usuarios que el mismo día han marcado con `me gusta`, al menos una canción de cada intérprete.
Se necesitan las tablas: CANCIONES, ME_GUSTA
```sql
Para (dom(m1) = ME_GUSTA), (dom(m2) = ME_GUSTA), (dom(c1) = CANCIONES), (dom(c2) = CANCIONES)
{t1 | ∀m2 ∃m1(((m1.U = t) ^ (m1.F = m2.F) ^ ∀c2 ∃c1((m2.CC = c2.CC) ^ (c1.I ¬= c2.I))))}
```
