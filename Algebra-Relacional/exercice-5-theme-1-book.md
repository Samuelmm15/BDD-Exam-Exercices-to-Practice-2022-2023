# Exercice 5 Álgebra relacional (Libro)

Sean las tablas siguientes:

PRO(NP, NOMP, CIUDADP)\
CLAVE: NP\
Significado: Cada fila representa un proveedor, cuyo identificador es NP, su nombre NOMP y habita en la ciudad CIUDADP.

ART(NA, DESA, COLOR, TALLA)\
CLAVE: NA\
Significado: Cada fila representa un artículo, cuyo identificador es NA y su descripción DESA.

FAB(NF, NOMF, CIUDADF)\
CLAVE: NF\
Significado: Cada fila representa una fábrica, cuyo identificador es NF, su nombre NOMF y está situada en CIUDADF.

PED(NP, NA, NF, CANTIDAD)\
CLAVE: NP, NA, NF\
Significado: Cada fila representa un pedido del artículo NA, el proveedor NP, para la fábrica NF.

Realizar la implementación de cada una de las consultas expresadas a continuación haciendo uso de álgebra relacional:

1) Hallar los identificadores de todas las fábricas.\
Se necesitan las tablas: FAB
```
p(NF)(FAB)
```

2) Hallar los nombres de las fábricas situadas en Madrid.\
Se necesitan las tablas: FAB
```
p(NOMF)(s((CIUDADF = 'Madrid'))(FAB))
```

3) Artículos tales que ningún otro tiene talla más pequeña.\
Se necesitan las tablas: ART
```
ART1 = ART2 = ART
p(NA)(s((ART1.talla < ART2.talla))(ART1 * ART2))
```

La solución real ante la consulta anterior es:
```
ART1 = ART2 = ART
p(NA)(ART) - p(ART1.NA)(s(ART1.TALLA > ART2.TALLA)(ART1 * ART2))
```

4) Proveedores que suministran a la fábrica F1.\
Se necesitan las tablas: PED\
Cabe destacar que suponemos que F1 se trata del identificador de la fábrica y no del nombre de esta.
```
p(NP)(s((NF = 'F1'))(PED))
```

5) Proveedores que suministran a la fábrica F1 el artículo A1.\
Se necesitan las tablas: PED\
Cabe destacar que suponemos que F1 y A1 hacen referencia a los identificadores de los elementos y no a los propios
nombres de estos.
```
p(NP)(s((NF = 'F1') ^ (NA = 'A1'))(PED))
```

6) Nombres de las fábricas a las que suministra el proveedor F1.\
Se necesitan las tablas: PED, FAB
```
p(NOMF)(s((NP = 'F1') ^ (PED.NF = FAB.NF))(FAB * PED))
```

Corregido de manera correcta:
```
p(NOMF)(s((NP = 'F1'))(FAB * PED))
```

`NOTA:` Cabe destacar que en álgebra relacional no se debe de establecer la sentencia `(PED.NF = FAB.NF)`, ya que,
al realizar la operación de yunción natural ya se está suponiendo esto, por tanto estaría mal si se pusiera, por
tanto, es mejor no ponerlo, porque si no está mal, eso solo se hace en cálculo relacional de tuplas.

7) Colores de los artículos suministrados por el proveedor P1.\
Se necesitan las tablas: PED, ART
```
p(COLOR)(s((PED.NP = 'P1'))(ART * PED))
```

8) Qué proveedores suministran a las fábricas F1 y F2.\
Se necesitan las tablas: PED, PRO
```
p(NP)(s((PRO.NP = PED.NP) ^ (PED.NF = 'F1') ^ (PED.NF = 'F2'))(PRO * PED))
```

Corregido de manera correcta por el solucionario:
```
(p(NP)(s(NF = 'F1')(PED)) ⋂ p(NP)(s(NF = 'F2')(PED)))
```

9) Proveedores que suministran artículos azules a la fábrica F1.\
Se necesitan las tablas: PED, ART
```
p(NP)(s((NF = 'F1') ^ (COLOR = 'Azul'))(PED * ART))
```

`NOTA:` Nótese que al hacer uso de la operación `ART * PED` ya tenemos como resultado una nueva tabla que contiene
los atributos de ambas tablas sumando los atributos comunes que tienen estas, por tanto, no hace falta hacer uso
de la notación de tipo `PED.NF = 'F1'`, ya que al obtener una nueva tabla como resultado, suponemos que los atributos
ya se encuentran dentro de esta, al igual ocurre con las consultas anteriores, deberemos de quitar todo esto
anterior y dichas manías de usarlo continuamente de esta manera.

10) Artículos suministrados a las Fábricas de Madrid.\
Se necesitan las tablas: PED, FAB
```
p(NA)(s((CIUDADF = 'Madrid'))(PED * FAB))
``