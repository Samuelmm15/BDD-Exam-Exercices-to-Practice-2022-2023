# Exercice 4 Theme 2 CRD

Sean las relaciones siguientes: 

SOCIO( aficionado, videoclub )\
SIGNIFICADO: El aficionado es socio de videoclub. 

GUSTA( aficionado, película )\
SIGNIFICADO: Al aficionado le gusta la película. 

VIDEOTECA( videoclub, película )\
SIGNIFICADO: El videoclub dispone en su videoteca de la película. 

Escribir las consultas siguientes en cálculo relacional de dominios:


a) Videoclubs que disponen de alguna película que le guste al aficionado X.\
Se necesitan las tablas: GUSTA, VIDEOTECA\
Significado: Como no se está indicando en ningún lado que el aficionado tenga que ser socio, escogemos el aficionado de
la tabla GUSTA.
```sql
{< V > | ∃(A, P)((<V, P> ∈ VIDEOTECA) ^ (<A, P> ∈ GUSTA) ^ (A = 'X'))}
```

b) Aficionados que son socios al menos de un videoclub que dispone de alguna película de
su gusto.\
Se necesitan las tablas: SOCIOS, GUSTA, VIDEOTECA
{< A > | ∃(V, P)((<A, V> ∈ SOCIO) ^ (<A, P> ∈ GUSTA) ^ (<V, P> ∈ VIDEOTECA))}

c) Aficionados que son socios solamente de videoclubes que disponen de alguna película de
su gusto.\
Se necesitan las tablas: SOCIOS, GUSTA, VIDEOTECA\
Significado: Los socios solo están en videoclub que tienen películas que le gustan
```sql
{< A > | ∃(V)((<A, V> ∈ SOCIO) ^ ∀(P)((<A, P> ∈ GUSTA) ^ (<V, P> ∈ VIDEOTECA)))}
```

d) Aficionados que sólo son socios de videoclubs que no tienen películas de su gusto.\
Se necesitan las tablas: SOCIOS, GUSTA, VIDEOTECA\
Significado: Socios que están en videotecas que no tienen películas que les gustan.
```sql
{< A > | ∃(V)((<A,V> ∈ SOCIO) ^ ¬∃(P)((<A,P> ∈ GUSTA) ^ (<V, P> ∈ VIDEOTECA)))}
```

e) Aficionados que son socios de algún videoclub que tiene todas las películas de su gusto.\
Se necesitan las tablas: SOCIOS, GUSTA, VIDEOTECA\
Significado: Socios que están en videoclubs en los que tienen todas las películas que les gustan.
```sql
{< A > | ∃(V)((<A,V> ∈ SOCIO) ^ ∀(P)((<A,P> ∈ GUSTA) ^ (<V,P> ∈ VIDEOTECA)))}
```

f) Aficionados que son socios solamente de videoclubs que tienen todas las películas de su
gusto.\
Se necesitan las tablas: SOCIOS, GUSTA, VIDEOTECA
```sql
{< A > | ∀(V)¬∃(P)((<A,V> ∈ SOCIO) ^ (<A,P> ∉ GUSTA) ^ (<V,P> ∉ VIDEOTECA)))}
```