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

10) Artículos suministrados a las fábricas de Madrid.
Se necesitan las tablas: PED, FAB
```sql
Para (dom(pe) = PED), (dom(f) = FAB)
{t | ∃f ∃pe (pe.NA = t ^ pe.NF = f.NF ^ f.CIUDADF = 'Madrid')}
```

