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
```sql
p(NF)(FAB)
```

2) Hallar los nombres de las fábricas situadas en Madrid.\
Se necesitan las tablas: FAB
```sql
p(NOMF)(s((CIUDADF = 'Madrid'))(FAB))
```

3) Artículos tales que ningún otro tiene talla más pequeña.\
Se necesitan las tablas: ART
```sql
ART1 = ART2 = ART
p(NA)(s((ART1.talla < ART2.talla))(ART1 * ART2))
```

La solución real ante la consulta anterior es:
```sql
ART1 = ART2 = ART
p(NA)(ART) - p(ART1.NA)(s(ART1.TALLA > ART2.TALLA)(ART1 * ART2))
```

4) Proveedores que suministran a la fábrica F1.\
Se necesitan las tablas: PED\
Cabe destacar que suponemos que F1 se trata del identificador de la fábrica y no del nombre de esta.
```sql
p(NP)(s((NF = 'F1'))(PED))
```

5) Proveedores que suministran a la fábrica F1 el artículo A1.\
Se necesitan las tablas: PED\
Cabe destacar que suponemos que F1 y A1 hacen referencia a los identificadores de los elementos y no a los propios
nombres de estos.
```sql
p(NP)(s((NF = 'F1') ^ (NA = 'A1'))(PED))
```

6) Nombres de las fábricas a las que suministra el proveedor F1.\
Se necesitan las tablas: PED, FAB
```sql
p(NOMF)(s((NP = 'F1') ^ (PED.NF = FAB.NF))(FAB * PED))
```

Corregido de manera correcta:
```sql
p(NOMF)(s((NP = 'F1'))(FAB * PED))
```

`NOTA:` Cabe destacar que en álgebra relacional no se debe de establecer la sentencia `(PED.NF = FAB.NF)`, ya que,
al realizar la operación de yunción natural ya se está suponiendo esto, por tanto estaría mal si se pusiera, por
tanto, es mejor no ponerlo, porque si no está mal, eso solo se hace en cálculo relacional de tuplas.

7) Colores de los artículos suministrados por el proveedor P1.\
Se necesitan las tablas: PED, ART
```sql
p(COLOR)(s((PED.NP = 'P1'))(ART * PED))
```

8) Qué proveedores suministran a las fábricas F1 y F2.\
Se necesitan las tablas: PED, PRO
```sql
p(NP)(s((PRO.NP = PED.NP) ^ (PED.NF = 'F1') ^ (PED.NF = 'F2'))(PRO * PED))
```

Corregido de manera correcta por el solucionario:
```sql
(p(NP)(s(NF = 'F1')(PED)) ⋂ p(NP)(s(NF = 'F2')(PED)))
```

9) Proveedores que suministran artículos azules a la fábrica F1.\
Se necesitan las tablas: PED, ART
```sql
p(NP)(s((NF = 'F1') ^ (COLOR = 'Azul'))(PED * ART))
```

`NOTA:` Nótese que al hacer uso de la operación `ART * PED` ya tenemos como resultado una nueva tabla que contiene
los atributos de ambas tablas sumando los atributos comunes que tienen estas, por tanto, no hace falta hacer uso
de la notación de tipo `PED.NF = 'F1'`, ya que al obtener una nueva tabla como resultado, suponemos que los atributos
ya se encuentran dentro de esta, al igual ocurre con las consultas anteriores, deberemos de quitar todo esto
anterior y dichas manías de usarlo continuamente de esta manera.

10) Artículos suministrados a las Fábricas de Madrid.\
Se necesitan las tablas: PED, FAB
```sql
p(NA)(s((CIUDADF = 'Madrid'))(PED * FAB))
```

11) Proveedores que suministran algún artículo azul a las fábricas de Madrid o Lisboa.\
Se necesitan las tablas: ART, FAB, PED
```sql
p(NP)(s((COLOR = 'azul') ^ (CIUDADF = 'Madrid') V (CIUDADF = 'Lisboa'))(ART * FAB * PED))
```

12) Artículos suministrados por proveedores en cuya ciudad hay alguna fábrica.\
Se necesitan las tablas: PED, PRO, FAB
```sql
p(NA)(s((CIUDADP = CIUDADF))(PRO * PED * FAB))
```

Sentencia corregida por el solucionario del libro:
```sql
p(NA)(PED * p(NP)(s(CIUDADF = CIUDADP)(PRO * FAB)))
```

13) Artículos suministrados a las fábricas de Madrid por proveedores de Madrid.\
Se necesitan las tablas: PED, PRO, FAB
```sql
p(NP)(s((CIUDADF = 'MADRID') ^ (CIUDADP = 'MADRID'))(PRO * PED * FAB))
```

14) Fábricas abastecidas por al menos un proveedor de distinta ciudad.\
Se necesitan las tablas: PED, PRO, FAB
```sql
p(NF)(s((CIUDADF ¬= CIUDADP))(PRO * PED * FAB))
```

`NOTA:` siempre que se quiera hacer uso del símbolo de distinto, tanto en álgebra relacional como en cálculo
relacional no se debe de hacer uso de !=, sino, se debe de hacer uso de `¬=`, ya que se trata del uso 
de operadores lógicos, es por ello que se emplea este tipo de símbolo de distinto.

15) Fábricas que NO son abastecidas de artículos azules por proveedores de Madrid.\
Se necesitan las tablas: PED, FAB, ART, PRO
```sql
p(NF)(s((COLOR != 'azul') ^ (CIUDADP = 'Madrid'))(PRO * PED * FAB * ART))
```

Esto anterior se puede hacer de esa manera anterior, pero, la manera más óptima de hacer esto esta corregida
por el solucionario del libro:
```sql
p(NF)(FAB) - P(NF)(s((CIUDADP = 'Madrid') ^ (COLOR = 'azul'))(PED * PRO * ART))
```

16) Proveedores que suministran al menos un artículo suministrado por al menos otro proveedor que suministra al 
menos un artículo azul.\
Se necesitan las tablas: PRO, PED, ART
```sql
V1 = p(NP)(s((COLOR = 'azul'))(PED * ART)) -- Estos son los artículos de color azul suministrados por al menos un 
-- proveedor
V2 = p(NA)(PED * V1) -- Artículos suministrados por los proveedores de artículos azules
p(NP)(PED * V2)
-- Hay que tener en cuenta que para el caso anterior, no se tiene en cuenta el hecho de que puede ser el propio
-- proveedor el que suministre los artículos azules.
```

17) Fábricas que usan al menos un artículo suministrado por el proveedor P1 a ella misma o a otra.\
Se necesitan las tablas: PED
```sql
p(NF)(s((NP = 'P1'))(PED))
```

Setencia corregida mediante el solucionario:
```sql
p(NF)(PED * p(NA)(s(NP = 'P1')(PED)))
```

18) Parejas de ciudades tales que un proveedor de la primera abastece a una fábrica de la
segunda.\
Se necesitan las tablas: PRO, PED, FAB
```sql
p(CIUDADP, CIUDADF)(PED * PRO * FAB)
```

19) Obtener las tripletas de valores de <CIUDAD, NA, CIUDAD> tales que un proveedor de la primera ciudad
abastece un artículo NA a una fábrica de la segunda ciudad.\
Se necesitan las tablas: PRO, PED, FAB
```sql
p(CIUDADP, NA, CIUDADF)(PED * PRO * FAB)
```

21) Proveedores que suministran un mismo artículo, al menos, a todas las fábricas.\
Se necesitan las tablas: PED, FAB
```sql
p(NP)(PED) ∩ p(NF)(FAB)
```

Casi está bien, el concepto que tenía era el correcto, pero, corregido es:
```sql
p(NP)((p(NP, NA, NF)(PED) / p(NF)(FAB)))
```

22) Fábricas que tienen como único proveedor a P1.\
Se necesitan las tablas: PED
```sql
p(NF)(s((NP = 'P1'))(PED)) - p(NF)(s((NP ¬= P1))(PED))
```

23) Artículos que son suministrados a todas las fábricas de Madrid.\
Se necesitan las tablas: PED, FAB
```sql
(p(NA, NF)(PED)) / (p(NF)(s(CIUDAD = 'Madrid')(FAB)))
```

24) Fábricas que usan al menos, todos los artículos suministrados por el proveedor P1.\
Se necesitan las tablas: PED, FAB
```sql
(p(NF, NA)(PED)) / p(NA)(s((NP = 'P1'))(PED))
```

Por ejemplo, esta consulta vi el todos antes de nada, y considere el operador cociente y es necesario para dicho
caso, por tanto, es correcto el empleo del operador cociente cuando se introduzca el todos en la consulta.

25) Fábricas que usan sólo artículos que pueden ser suministrados por el proveedor P1.
Se necesitan las tablas: FAB, PED
```sql
p(NF)(FAB) - (p(NF)(PED * ((p(NA)(ART) - p(NA)(s(NP = 'P1')(PED))))))
```

26) Fábricas abastecidas por el proveedor P1 con todos los artículos que este suministra.
Se necesitan las tablas: PED
```sql
(p(NF, NA, NP)(PED)) / (p(NA, NP)(s(NP = 'P1')(PED)))
```

27) Fábricas que obtienen del proveedor P1, total o parcialmente, todos los artículos que usan.
Se necesitan las tablas: PED
```sql
(p(NF, NA, NP)(PED)) / (p(NA, NP)(s(NP = 'P1')(PED)))
```

Corregido por el solucionario:
```sql
p(NF)(FAB) - (p(NF)(p(NA, NF)(PED) - (p(NA, NF)(s(NP = 'P1')(PED)))))
```

28) Fábricas abastecidas por todos los proveedores que suministran algún artículo de color azul.
```sql
(p(NF, NP)(PED)) / p(NP)(s(COLOR = AZUL)(PED * ART))
```