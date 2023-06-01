# Exercice 4

4) Sean las relaciones siguientes: 
    SOCIO( aficionado, videoclub )
    SIGNIFICADO: El aficionado es socio de videoclub. 

    GUSTA( aficionado, película )
    SIGNIFICADO: Al aficionado le gusta la película. 

    VIDEOTECA( videoclub, película )
    SIGNIFICADO: El videoclub dispone en su videoteca de la película. 

    Escribir las consultas siguientes en álgebra relacional:
    a) Videoclubs que disponen de alguna película que le guste al aficionado X.
    Implementación de copilot:
    ```
        p(videoclub)(s((GUSTA.aficionado = X) ^ (GUSTA.pelicula = VIDEOTECA.pelicula))(GUSTA*VIDEOTECA))
    ```
    Se necesitan las tablas: GUSTA, VIDEOTECA.
    ```
        p(videoclub)(s(VIDEOTECA.pelicula = GUSTA.pelicula)(GUSTA*VIDEOTECA))
    ```
    b) Aficionados que son socios al menos de un videoclub que dispone de alguna película de
    su gusto.
    Implementación de copilot:
    ```
        p(aficionado)(s((SOCIO.aficionado = GUSTA.aficionado) ^ (SOCIO.videoclub = VIDEOTECA.videoclub) ^ 
        (GUSTA.pelicula = VIDEOTECA.pelicula))(SOCIO*GUSTA*VIDEOTECA))
    ```
    Se necesitan las tablas: SOCIO, GUSTA, VIDEOTECA.
    ```
        p(aficionado)(s((SOCIO.aficionado = GUSTA.aficionado) ^ (SOCIO.videoclub = VIDEOTECA.videoclub) ^ 
        (GUSTA.pelicula = VIDEOTECA.pelicula))(SOCIO*GUSTA*VIDEOTECA))
    ```
    c) Aficionados que son socios solamente de videoclubes que disponen de alguna película de
    su gusto.
    Implementación de copilot:
    ```
        V1 = GUSTA ∩ VIDEOTECA
        p(aficionado)(s((SOCIO.videoclub = V1.videoclub))(SOCIO*V1))
    ```
    Se necesitan las tablas: SOCIO, GUSTA, PELICULA
    ```
        V1 = GUSTA ∩ VIDEOTECA
        p(aficionado)(s((SOCIO.videoclub = V1.videoclub))(SOCIO*V1))
    ```
    d) Aficionados que sólo son socios de videoclubs que no tienen películas de su gusto.
    Implementación de copilot:
    ```
        V1 = VIDEOTECA - GUSTA
        p(aficionado)(s((SOCIO.videoclub = V1.videoclub))(SOCIO*V1))
    ```
    Se necesitan las tablas: SOCIOS, GUSTA, VIDEOTECA
    ```
        V1 = VIDEOTECA - GUSTA
        p(aficionado)(s((SOCIO.videoclub = V1.videoclub))(SOCIO*V1))
    ```
    e) Aficionados que son socios de algún videoclub que tiene todas las películas de su gusto. 
    Implementación de copilot:
    ```
        V1 = GUSTA - VIDEOTECA
        p(aficionado)(s((SOCIO.videoclub = V1.videoclub))(SOCIO*V1))
    ```
    Se necesitan las tablas: SOCIOS, GUSTA, VIDEOTECA
    ```
        V1 = GUSTA - VIDEOTECA
        p(aficionado)(s((SOCIO.videoclub = V1.videoclub))(SOCIO*V1))
    ```
    f) Aficionados que son socios solamente de videoclubs que tienen todas las películas de su
       gusto.
    Implementación de copilot:
    ```
           V1 = GUSTA / VIDEOTECA  /// Obtenemos todas las películas que gustan de un videoclub
            V2 = VIDEOTECA
            p(aficionado)(s((SOCIO.videoclub = V1.videoclub) ^ (SOCIO.videoclub != V2.videoclub))(SOCIO*V1*V2))
        ```
    ```
    Se necesitan las tablas: SOCIOS, GUSTA, VIDEOTECA
    ```
        V1 = GUSTA / VIDEOTECA  /// Obtenemos todas las películas que gustan de un videoclub
        V2 = VIDEOTECA
        p(aficionado)(s((SOCIO.videoclub = V1.videoclub) ^ (SOCIO.videoclub != V2.videoclub))(SOCIO*V1*V2))
    ```

