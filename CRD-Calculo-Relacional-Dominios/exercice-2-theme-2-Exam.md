# Exercice 2 Theme 2 CRD Exam

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

Realizar la implementacion de las distintas consultas haciendo uso de calculo relacional de dominios:

1) Canciones que han sido añadidas en al menos 2 listas por el mismo usuario.\
Se necesitan las tablas: LISTAS
```sql
{<cc> | ∃(cl, cl`, u)((<cl, cc, u> ∈ LISTAS) ^ (<cl`, cc, u> ∈ LISTAS) ^ (cl ¬= cl`))}
```

2) Usuarios a los que no les gusta ninguna cancion de la lista L1.\
Se necesitan las tablas: ME_GUSTA, LISTAS
```sql
{<u> | ∃(cc, cc`, cl, f)((<cl, cc, u> ∈ LISTAS) ^ (<cc`, u, f> ∈ ME_GUSTA) ^ (cc ¬= cc`) ^ (cl = 'L1'))}
```

3) Cancion mas corta del interprete 1.
Se necesitan las tablas: CANCIONES
```sql
{<cc> | ∃(i, i`, t, t`, d, d`)((<cc, i, t, d> ∈ CANCIONES) ^ ∀(cc`)(<cc`, i`, t`, d`> ∈ CANCIONES) ^ (cc ¬= cc`) 
    ^ (i = i`) ^ (i = 'l1') ^ (d <= d`))}
```

4) Usuarios que han marcado con 'me gusta' todas las canciones de alguna lista.\
Se necesitan las tablas: LISTA, ME_GUSTA
```sql
{<u> | ∃(cl, f)(∀(cc)(<cl, cc, u> ∈ LISTAS) ^ (<cc, u, f> ∈ ME_GUSTA))}
```

5) Usuarios que el mismo dia han marcado con 'me gusta' al menos una cancion de cada interprete.\
Se necesitan las tablas: ME_GUSTA, CANCIONES
```sql
{<u> | ∃(cc, f, f`, t, d)((<cc, u, f> ∈ ME_GUSTA) ^ (<cc, u, f`> ∈ ME_GUSTA) ^ (f = f`)
    ^ ∀(i)((<cc, i, t, d> ∈ CANCIONES))}
```