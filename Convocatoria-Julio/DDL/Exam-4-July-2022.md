# 4 July Exam Servicio Regional de Salud

Relación de las distintas tablas usadas en la base de datos:

PERSONAS (DNI, E, S, MP)\
SIGNIFICADO: La persona con DNI y edad E tiene sexo S y reside en el municipio MP.\
CLAVE PRIMARIA: (DNI)

VACUNAS (DNI, FV, V, C, MC)\
SIGNIFICADO: La persona con DNI ha recibido una dosis de la vacuna V en la fecha FV en el centro médico C\
ubicado en el municipio MC.\
CLAVE PRIMARIA: (DNI, FV) \
CLAVE AJENA: (DNI)

CONTAGIOS (DNI, FC, FA)\
SIGNIFICADO: La persona con DNIestuvo contagiada entre las fechas FC y FA.\
CLAVE PRIMARIA: (DNI, FC)

Realizar la implementación de las siguientes consultas haciendo uso de SQL (DDL)
(`página 25 de los apuntes los resultados`):

1. Crea una vista que muestre para cada centro médico y tipo de vacuna el número de dosis administradas.\
Se necesitan las tablas: VACUNAS
```sql
CREATE VIEW NUEVA_VISTA AS (SELECT C, V, COUNT(*)
                            FROM VACUNAS
                            GROUP BY C, V);
```

2. Modifica la tabla CONTAGIOS para añadir un nuevo atributo G que indique la gravedad (BAJA, MEDIA, ALTA).\
Se necesita la tabla: CONTAGIOS
```sql
ALTER TABLE CONTAGIOS ADD(
  G VARCHAR(5) NOT NULL  
);

ALTER TABLE CONTAGIOS
ADD CONSTRAINT NAMES CHECK ( G IN ('BAJA', 'MEDIA', 'ALTA') );
```

3. Actualiza la gravedad a ALTA cuando la duración de un contagio sea superior a 15 días.
Se necesita la tabla: CONTAGIOS
```sql
UPDATE CONTAGIOS
SET G = 'ALTA'
WHERE ((SYSDATE() - FC) >= 15) AND FA IS NULL OR (FA IS NOT NULL AND ((FA-FC) >= 15));
```

4. Fuerza a todas las personas contagiadas tengan una fecha de alta.
Se necesita la tabla: CONTAGIOS
```sql
UPDATE CONTAGIOS
SET FA = SYSDATE()
WHERE FA IS NULL;
```

5. Impide que una persona pueda recibir dos dosis de una vacuna en un plazo inferior a 30 días.
Se necesita la tabla: VACUNAS
```sql
-- Es necesario la implementación de un trigger para esto

```