# Exercice 1 Exam Ventas

Se tienen las siguientes relaciones de la base de datos correspondiente:

ARTICULOS(CA, CAT)\
Significado: El artículo CA es de categoría CAT\
CP: CA

TIENDAS(CT, CA, PR)\
Significado: La tienda con código CT dispone de un artículo CA a un precio de PR euros.\
CP: CT, CA\
CA: CA

VENTAS(DNI, CT, CA NU, F)\
Significado: La persona con dni DNI ha comprado en la tienda CT, NU unidades del artículo CA, en la fecha F\
CP: DNI, CT, CA, F\
CA: CT, CA

Realizar la implementación de las distintas consultas a continuación, haciendo uso de álgebra relacional:

1) Tiendas con artículos de las categorías C1 o C2.
Se necesitan las tablas: TIENDAS, ARTICULOS
```sql
p(CT)(s((CA = 'C1') v (CA = 'C2'))(TIENDAS * ARTICULOS))
```

2) Personas que han comprado el artículo A1 y A2 en una misma tienda.\
Se necesitan las tablas: VENTAS
```sql
A = p(DNI, CT)(s((CA = 'A1'))(VENTAS)) -- Personas que compran A1 en una tienda
B = p(DNI, CT)(s((CA = 'A2'))(VENTAS)) -- Personas que compran A2 en una tienda
    A ∩ B -- Personas que compran A1 y A2 en la misma tienda y la misma persona
```

3) Tienda que tiene el artículo A1 con menor precio.\
Se necesitan las tablas: TIENDAS
```sql
T1 = T2 = TIENDAS
p(CT)(s((T1.CT ¬= T2.CT) ^ (T1.CA = 'A1') ^ (T1.CA = T2.CA) ^ (T1.PR <= T2.PR))( T1 x T2))
```

4) Personas que han comprado al menos un artículo en cada tienda.\
Se necesitan las tablas: VENTAS, TIENDAS\
Significado: Personas que han comprado al menos un artículo en todas(/) las tiendas que tienen dicho articulo.
```sql
A = p(DNI, CA, CT)(VENTAS)
B = p(CA, CT)(TIENDAS)
    p(DNI)(A / B)
```

5) Personas que han comprado en alguna tienda todos los articulos de alguna categoria.\
Se necesitan las tablas: VENTAS, ARTICULOS
```sql
ART1 = ART2 = ARTICULOS
    p(DNI)(p(DNI, CA)(VENTAS) / p(CA)(s((ART1.CA ¬= ART2.CA) ^ (ART1.CAT = ART2.CAT))(ART1 x ART2)))
```