# Exercice 4 CRT

Sean las relaciones siguientes: 
SOCIO( aficionado, videoclub )
SIGNIFICADO: El aficionado es socio de videoclub. 
GUSTA( aficionado, película )
SIGNIFICADO: Al aficionado le gusta la película. 
VIDEOTECA( videoclub, película )
SIGNIFICADO: El videoclub dispone en su videoteca de la película. 

Escribir las consultas siguientes en cálculo relacional de tuplas:

a) Videoclubs que disponen de alguna película que le guste al aficionado X.\
Implementado por copilot:
```
  {<videoclub> | ∃aficionado(X) ∧ (∃película(GUSTA(X, película)) ∧ VIDEOTECA(videoclub, película))}
```
Se necesitan las tablas: GUSTA, VIDEOCLUB
```
  x: GUSTA, y: VIDEOTECA
  {y.videoclub | GUSTA(x) ^ (x.aficionado = X) ^ VIDEOTECA(y) ^ (y.pelicula = x.pelicula)}
```

b) Aficionados que son socios al menos de un videoclub que dispone de alguna película de
su gusto.\
Implementado por copilot:
```
  {<aficionado> | ∃videoclub, película (∃aficionado(SOCIO(aficionado, videoclub)) ∧ GUSTA(aficionado, película) 
  ∧ VIDEOTECA(videoclub, película))}
```
Se necesitan las tablas: SOCIO, GUSTA, VIDEOTECA
```
  x: SOCIO, y: GUSTA, z: VIDEOTECA
  {x.aficionado | GUSTA(y) ^ VIDEOTECA(z) ^ (y.pelicula = z.pelicula) ^ SOCIO(x) ^ (x.aficionado = y.aficionado) ^ 
  (x.videoclub = z.videoclub)}
```

c) Aficionados que son socios solamente de videoclubes que disponen de alguna película de
su gusto.\
Implementado por copilot:
```
{<aficionado> | ∀videoclub, película ((SOCIO(aficionado, videoclub) → VIDEOTECA(videoclub, película)) ∧ 
(GUSTA(aficionado, película) → ∃videoclub, película VIDEOTECA(videoclub, película))))}
```
Se necesitan las tablas: SOCIO, GUSTA, VIDEOTECA
```
  x: SOCIO, y: GUSTA, z: VIDEOTECA
  {x.aficionado | SOCIO(x) ^ VIDEOTECA(z) ^ (x.videoclub = z.videoclub) ^ GUSTA(y) ^ ∀ y.pelicula ∃ z.videoclub 
  (y.pelicula = z.pelicula)}
```

d) Aficionados que sólo son socios de videoclubs que no tienen películas de su gusto.\
Implementado por copilot:
```
{<aficionado> | ∀videoclub, película ((SOCIO(aficionado, videoclub) → ¬∃videoclub, película (GUSTA(aficionado, película)
 ∧ VIDEOTECA(videoclub, película))))}
```
Se necesitan las tablas: SOCIO, GUSTA, VIDEOTECA
```
  x: SOCIO, y: GUSTA, z: VIDEOTECA
  {x.aficionado | SOCIO(x) ^ VIDEOTECA(z) ^ (x.videoclub = z.videoclub) ^ GUSTA(y) ^ ∀ y.pelicula ∃ z.videoclub 
  ((y.pelicula != z.pelicula))}
```

e) Aficionados que son socios de algún videoclub que tiene todas las películas de su gusto.\
Implementado por copilot:
```
{<aficionado> | ∃videoclub (∃aficionado(SOCIO(aficionado, videoclub)) ∧ (∀película (GUSTA(aficionado, película) 
→ VIDEOTECA(videoclub, película))))}
```
Se necesitan las tablas: SOCIO, GUSTA, VIDEOTECA
```
  x: SOCIO, y: GUSTA, z: VIDEOTECA
  {x.aficionado | SOCIO(x) ^ VIDEOTECA(z) ^ (x.videoclub = z.videoclub) ^ GUSTA(y) ^ ∀ y.pelicula ∃ z.videoclub
  ((y.pelicula = z.pelicula)}
```

f) Aficionados que son socios solamente de videoclubs que tienen todas las películas de su
gusto.\
Implementado por copilot:
```
{<aficionado> | ∀videoclub (∃aficionado(SOCIO(aficionado, videoclub)) → (∀película (GUSTA(aficionado, película) 
→ VIDEOTECA(videoclub, película))))}
```
Se necesitan las tablas: SOCIO, GUSTA, VIDEOTECA
```
  x: SOCIO, y: GUSTA, z: VIDEOTECA
  {x.aficionado | SOCIO(x) ^ VIDEOTECA(z) ^ (x.videoclub = z.videoclub) ^ GUSTA(y) ^ ∀ y.pelicula ¬∃ z.videoclub
  ((y.pelicula != z.pelicula)}
```
