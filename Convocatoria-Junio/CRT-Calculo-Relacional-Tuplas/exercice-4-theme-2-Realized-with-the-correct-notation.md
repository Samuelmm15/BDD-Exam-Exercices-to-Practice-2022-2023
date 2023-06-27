# Exercice 4 CRT

`NOTA:` La notación usada en el correspondiente documento es la que quiere el profesor en cuanto a cálculo relacional
de tuplas.

Sean las relaciones siguientes: 

SOCIO( aficionado, videoclub )\
SIGNIFICADO: El aficionado es socio de videoclub. 

GUSTA( aficionado, película )\
SIGNIFICADO: Al aficionado le gusta la película. 

VIDEOTECA( videoclub, película )\
SIGNIFICADO: El videoclub dispone en su videoteca de la película. 

Escribir las consultas siguientes en álgebra relacional:\
a) Videoclubs que disponen de alguna película que le guste al aficionado X.\
Se necesitan las tablas: GUSTA, VIDEOTECA
```
Para (dom(g) = GUSTA) y (dom(v) = VIDEOTECA)
{t | ∃g ∃v((t = v.videoclub) ^ (g.aficionado = X) ^ (v.pelicula = g.pelicula)} 
```

Cabe destacar que esta consulta anterior implementada en CRT está corregida mediante el solucionario del libro,
por tanto, está expresada de manera correcta haciendo uso de la notación correcta.

b) Aficionados que son socios al menos de un videoclub que dispone de alguna película de
su gusto.\
Se necesitan las siguientes tablas: SOCIO, GUSTA, VIDEOTECA
```
Para (dom(s) = SOCIO), (dom(g) = GUSTA) y (dom(v) = VIDEOTECA)
{t | ∃s ∃g ∃v((s.aficionado = g.aficionado) ^ (g.pelicula = v.pelicula) ^ (s.videoclub = v.videoclub) 
^ (s.aficionado = t)}
```

`NOTA`: Hay que tener en cuenta que en el solucionario del libro no realiza la operación de `(s.aficionado = t)`,
tener en cuenta que esto se puede deber a algún hecho de algún tipo.

c) Aficionados que son socios solamente de videoclubes que disponen de alguna película de
su gusto.\
Se necesitan las siguientes tablas: SOCIO, GUSTA, VIDEOTECA
```
Para (dom(s) = SOCIO), (dom(g) = GUSTA) y (dom(v) = VIDEOTECA)
{t | ∃g ∃v((g.pelicula = v.pelicula)) ^ ∀s((s.videoclub = v.videoclub) ^ (s.aficionado = g.aficionado) 
^ (s.aficionado = t))}
```

Solución propuesta por el solucionario:
```
{t | SOCIO(t) ^ ∀s((s.aficionado ¬= t) v ((s.aficionado = t) ^ (∃g ∃v ((s.videoclub = v.videoclub) ^
(v.pelicula = g.pelicula) ^ (g.aficionado = t)))))}
```

d) Aficionados que sólo son socios de videoclubs que no tienen películas de su gusto.\
Se necesitan las siguientes tablas: SOCIO, GUSTA, VIDEOTECA
```
Para (dom(s) = SOCIO), (dom(g) = GUSTA), (dom(v) = VIDEOTECA)
{t | SOCIO(t) ^ ∀s((s.aficionado = t) ^ ∃g ∃v((s.videoclub = v.videoclub) ^ 
(v.pelicula ¬= g.pelicula) ^ (g.aficionado = t)))}
```

e) Aficionados que son socios de algún videoclub que tiene todas las películas de su gusto.\
Se necesitan las siguientes tablas: SOCIO, GUSTA, VIDEOTECA
```
Para (dom(s) = SOCIO), (dom(g) = GUSTA), (dom(v) = VIDEOTECA)
{t | SOCIO(t) ^ ∀s((s.aficionado = t) ^ ∃v ∀g((s.videoclub = v.videoclub) ^ (v.pelicula = g.pelicula)
^ (g.aficionado = t)))}
```

f) Aficionados que son socios solamente de videoclubs que tienen todas las películas de su
gusto.\
Se necesitan las siguientes tablas: SOCIO, GUSTA, VIDEOTECA
```
Para (dom(s) = SOCIO), (dom(g) = GUSTA), (dom(v) = VIDEOTECA)
{t | SOCIO(t) ^ ∀s((s.aficionado = t) ^ ∀v ∀g((v.videoclub = s.videoclub) ^ (v.pelicula = g.pelicula)
^ (g.aficionado = t)))}
```
