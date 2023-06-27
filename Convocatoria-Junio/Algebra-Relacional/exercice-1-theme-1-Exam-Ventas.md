# Exercice 1 Theme 1 Exam Ventas Álgebra Relacional

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

1) Tiendas con artículos de las categorías C1 o C2.\
Se necesitan las tablas: TIENDAS, ARTICULOS
```sql
p(CT)(s((CAT = 'C1') v (CAT = 'C2'))(TIENDAS * ARTICULOS))
```

2) Personas que han comprado el artículo A1 y A2 en una misma tienda.\
Se necesitan las tablas: VENTAS
```sql
V1 = V2 = VENTAS
p(DNI)(s((V1.CA = 'A1') ^ (V2.CA = 'A2') ^ (V1.CT = V2.CT) ^ (V1.DNI = V2.DNI))(V1 x V2))
```

3) Tiendas que tienen el artículo A1 con menor precio.\
Se necesitan las tablas: TIENDAS
```sql
T1 = T2 = TIENDAS
p(CT)(s((T1.CA = 'A1') ^ (T2.CA = T1.CA) ^ (T1.PR <= T2.PR))(T1 x T2))
```

4) Personas que han comprado al menos un artículo en cada tienda.\
Se necesitan las tablas: TIENDAS, VENTAS\
Significado: Personas que compran un arículo en todas las tiendas.
```sql
A = p(CT, CA)(TIENDAS)
B = p(DNI, CT, CA)(VENTAS)
    p(DNI)(B / A)
```

5) Personas que han comprado en alguna tienda todos los artículos de una categoría.\
Se necesitan las tablas: VENTAS, TIENDA, ARTICULOS\
Significado: Personas que compran en tiendas todos los articulos donde la categoría es la misma.
```sql
p(DNI)(p(DNI, CT, CA)(VENTAS) / p(CT, CA)(p(CT, CA, CAT)(TIENDAS * ARTICULOS) * p(CT, CA, CAT)(TIENDAS * ARTICULOS)))
```