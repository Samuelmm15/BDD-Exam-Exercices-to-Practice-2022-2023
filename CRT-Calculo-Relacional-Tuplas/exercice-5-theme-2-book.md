# Exercice 5 CRT (book)

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

Realizar la implementación de cada una de las consultas expresadas a continuación haciendo uso de 
cálculo relacional de tuplas:

1) Hallar los identificadores de todas las Fábricas:\
Se necesita la tabla: FAB
```sql
Para (dom(f) = FAB)
{t | ∃f (f.NF = t)}
```

2) Hallar los nombres de las fábricas situadas en Madrid.\
Se necesitan las tablas: FAB
```sql
Para (dom(f) = FAB)
{t | ∃f ((f.CIUDADF = 'Madrid') ^ (f.NF = t))}
```

3) Artículos tales que ningún otro tiene una talla más pequeña.\
Se necesita la tabla: ART
```sql
Para (dom(a) = ART), (dom(a1) = ART)
{t | ∃a ((a.NA = t) ^ ∀a1 (a.TALLA <= a1.TALLA))}
```

4) Proveedores que suministran a la fábrica F1.\
Se necesitan las tablas: PED
```sql
Para (dom(pe) = PED)
{t | ∃pe ((pe.NP = t) ^ (pe.NF = 'F1'))}
```

5) Proveedores que suministran a la fábrica F1 el artículo A1.\
Se necesitan las tablas: PED
```sql
Para (dom(pe) = PED)
{t | ∃pe (pe.NP = t ^ pe.NF = 'F1' ^ pe.NA = 'A1')}
```

`NOTA:` Tener cuidado con como se ponen los paréntesis para dichas consultas, ya que se pueden poner
de la manera en la que se ponen de la manera anterior y no es necesario tanto paréntesis como en las primeras
consultas implementadas en la parte de arriba del documento.

6) Nombre de las fábricas a las que suministra el proveedor P1.\
Se necesitan las tablas: PED
```sql
Para (dom(pe) = PED)
{t | ∃pe (pe.NF = t ^ pe.NP = 'P1')}
```

7) Colores de los artículos suminstrados por el proveedor P1.\
Se necesitan las tablas: PED, ART
```sql
Para (dom(pe) = PED), (dom(a) = ART)
{t | ∃a (a.COLOR = t) ^ ∃pe (pe.NA = a.NA ^ pe.NP = 'P1')}
```

Todo esto anterior puesto de mejor manera según el solucionario:
```sql
{t | ∃a ∃pe (a.COLOR = t ^ pe.NA = a.NA ^ pe.NP = 'P1')}
```

8) Qué proveedores suministran a las fábricas F1 y F2\
Se necesitan las tablas: PED
```sql
Para (dom(pe) = PED)
{t | (∃pe (pe.NP = t ^ pe.NF = 'F1')) ^ (∃pe (pe.NP = t ^ pe.NF = 'F2'))}
```

9) Proveedores que suministran artículos azules a la fábrica F1.\
Se necesitan las tablas: PED, ART
```sql
Para (dom(pe) = PED), (dom(a) = ART)
{t | ∃a ∃pe (pe.NP = t ^ pe.NA = a.NA ^ a.COLOR = 'azul' ^ pe.NF = 'F1')}
```

10) Artículos suministrados a las fábricas de Madrid.\
Se necesitan las tablas: PED, FAB
```sql
Para (dom(pe) = PED), (dom(f) = FAB)
{t | ∃f ∃pe (pe.NA = t ^ pe.NF = f.NF ^ f.CIUDADF = 'Madrid')}
```

11) Proveedores que suministran algún artículo azul a las fábricas de Madrid o Lisboa.\
Se necesitan las tablas: PRO, PED, ART, FAB
```sql
Para (dom(pe) = PED), (dom(a) = ART), (dom(f) = FAB)
{t | ∃f ∃pe ∃a ((pe.NP = t) ^ (a.NA = pe.NA) ^
    (a.COLOR = 'azul') ^ (pe.NF = f.NF) ^ (f.CIUDAD = 'Madrid' v f.CIUDAD = 'Lisboa'))}
```

12) Artículos suministrados por proveedores en cuya ciudad hay alguna fábrica.\
Se necesitan las tablas: PRO, FAB, PED
```sql
Para (dom(pr) = PRO), (dom(pe) = PED), (dom(f) = FAB)
{t | ∃pe ∃pro ∃f ((pe.NA = t) ^ (pe.NP = pro.NP) ^ (pe.NF = f.NF) ^ (pro.CIUDAD = f.CIUDAD))}
```

Corregido por el solucionario:
```sql
Para (dom(pr) = PRO), (dom(pe) = PED), (dom(f) = FAB)
{t | ∃pe ∃pro ∃f ((pe.NA = t) ^ (pe.NP = pro.NP) ^ (pro.CIUDAD = f.CIUDAD))}
-- Debido a que te dice que alguna fábrica, no se tiene que producir que (pe.NF = f.NF)
```

13) Artículos suministrados a las fábricas de Madrid por proveedores de Madrid.\
Se necesitan las tablas: PED, PRO, FAB
```sql
Para (dom(pr) = PRO), (dom(pe) = PED), (dom(f) = FAB)
{t | ∃pe ∃pro ∃f ((pe.NA = t) ^ (pe.NP = pr.NP) ^ (pe.NF = f.NF) ^ (pr.ciudad = 'Madrid')
    ^ (f.CIUDAD = 'Madrid'))}
```

14) Fábricas suministradas por al menos un proveedor de distinta ciudad.
Se necesitan las tablas: PRO, PED, FAB
```sql
Para (dom(pr) = PRO), (dom(pe) = PED), (dom(f) = FAB)
{t | ∃pe ∃pro ∃f ((pe.NF = t) ^ (pe.NF = f.NF) ^ (pe.NP = pr.NP) ^ (f.CIUDAD ¬= pr.CIUDAD))}
```

15) Fábricas que nos son suministradas de artículos azules por proveedores de Madrid.
Se necesitan las tablas: PED, PRO, ART
```sql
Para (dom(pr) = PRO), (dom(pe) = PED), (dom(a) = ART)
{t | ∃f (f.NF = t) ^ ¬(∃pe ∃pro ∃a ((pe.NF = t) ^ (pe.NA = a.NA) ^ (pe.NP = pr.NP)
    ^ (a.COLOR = 'azul') ^ (pr.CIUDAD = 'Madrid')))}
```

16) Proveedores que suministran al menos un artículos suministrado por al menos otro proveedor que suministra al
menos un artículo azul.\
Se necesitan las tablas: PED, ART, PRO
```sql
Para (dom(pr) = PRO), (dom(pe) = PED), (dom(a) = ART), (dom(pe1) = PED)
{t | ∃pe (pe.NP = t) ^ ∃pe1 ∃a ((pe1.NP ¬= t) ^ (pe1.NA = pe.NA) ^ (pe1.NA = a.NA) ^ (a.COLOR = 'azul'))}
```

17) Fábricas que usan al menos un artículo suministrado por el proveedor P1 a la misma fábrica u a otra.\
Se necesitan las tablas: PED
```sql
Para (dom(pe) = PED), dom(pe1) = PED
{t | ∃pe ((pe.NF = t)) ^ ∃pe1 ((pe1.NP = 'P1') ^ (pe.NA = pe1.NA) ^ ((pe1.NF = pe.NF) v (pe1.NF ¬= pe.NF)))}
```

18) Parejas de ciudades tales que un proveedor de la primera abastece a una fábrica de la segunda.\
Se necesitan las tablas: PED, PRO
```sql
Para (dom(pe) = PED), (dom(pr) = PRO), (dom(pr1) = PRO), (dom(pe1) = PED)
{t2 | ∃pr ∃pr1 ∃pe ∃pe1 ((pr.CIUDAD = t1) ^ (pr1.CIUDAD = t2) ^ (pe.NP = pr1.NP) ^ (pe1.NP = pr.NP))}
```

Consulta resuelta mediante el solucionario:
```sql
Para (dom(pe) = PED), (dom(f) = FAB), (dom(pr) = PRO)
{t2 | ∃pe ∃pr ∃f ((pr.CIUDAD = t1) ^ (f.CIUDAD = t2) ^ (pr.NP = pe.NP) ^ (pe.NF = f.NF))}
```

`NOTA:` Cabe destacar que cuando se tiene como resultado una pareja de elementos en cálculo relacional de tuplas
se hace uso de los números 1 y 2 para indicar que solo existe un único resultado, es decir una única columna en
la mayoría de casos, pero en casos como en el anterior, se produce que existen dos consultas o una pareja, por eso,
se indica el número 2 para dicho caso.

19) Obtener las tripletas de valores <ciudad, na , ciudad> tales que un proveedor de la primera ciudad abastece un 
artículo NA a una fábrica de la segunda ciudad.\
Se necesitan las tablas: FAB, PED, PRO
```sql
Para (dom(pe) = PED), (dom(f) = FAB), (dom(pr) = PRO)
{t3 | ∃pe ∃pr ∃f ((pr.CIUDAD = t1) ^ (pe.NA = t2) ^ (f.CIUDAD = t3) ^ (pr.NP = pe.NP) ^ (pe.NF = f.NF))}
```

20) Repetir la pregunta anterior, pero sin obtener las tripletas en que ambas ciudad son la misma.
Se necesitan las tablas: FAB, PED, PRO
```sql
Para (dom(pe) = PED), (dom(f) = FAB), (dom(pr) = PRO)
{t3 | ∃pe ∃pr ∃f ((pr.CIUDAD = t1) ^ (pe.NA = t2) ^ (f.CIUDAD = t3) ^ (pr.NP = pe.NP) ^ (pe.NF = f.NF)
    ^ (t1 ¬= t3))}
```