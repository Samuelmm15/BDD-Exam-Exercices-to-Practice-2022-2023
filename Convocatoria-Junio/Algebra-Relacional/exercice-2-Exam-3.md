# Exercice 1 Exam 3 Álgebra relacional

Se tienen la siguientes tablas que implementan la base de datos:

ARTÍCULOS(CA, CAT) \
Significado: El artículo CA es de categoría CAT. \
CP: CA

TIENDAS(CT, CA, PR) \
Significado: La tienda con código CT dispone del artículo CA a un precio PR euros. \
CP: (CT, CA) \
CA: CA

VENTAS(DNI, CT, CA, NU, F) \
Significado: La persona con dni DNI, ha comprado en la tienda CT, NU unidades \
del artículo CA, en la fecha F. \
CP: DNI, CT, CA, F \
CA: CT, CA

Realizar la implementación de las distintas consultas en álgebra relacional:

1. Tiendas con artículos de las categorías C1 o C2.\
Se necesitan las tablas: ARTICULOS, TIENDAS
```sql
p(CT)(s((CAT = 'C1') v (CAT = 'C2'))(TIENDAS * ARTICULOS))
```

2. Personas que han comprado el artículo A1 y A2 en una misma tienda.\
Se necesitan las tablas: VENTA
```sql
A = p(DNI, CT)(s(CA = 'A1')(VENTAS))
B = p(DNI, CT)(s(CA = 'A2')(VENTAS))
    p(DNI)(A ∩ B)
```

3. Tienda que tiene el artículo A1 con el menor precio.\
Se necesitan las tablas: TIENDAS
```sql
-- Tiendas que tiene el artículo A1.
A = p(CT, PR)(s(CA = 'A1')(TIENDAS))
p(CT)(s(PR <= A.PR)(TIENDAS x A))
```

4. Personas que han comprado al menos un artículo en cada tienda.\
Se necesitan las tablas: TIENDAS, VENTAS\
Significado: Personas que han comprado un artículo en TODAS las tiendas.
```sql
-- Obtenemos todas las tiendas que exsiten
p(DNI)((p(DNI, CT)(VENTAS)) / (p(CT)(TIENDAS)))
```

5. Personas que han comprado en alguna tienda todos los artículos de alguna categoría.\
Se necesitan las tablas: VENTAS, TIENDAS, ARTICULOS
```sql
    -- Todos los artículos de alguna categoría
    A = p(CT, CA)(TIENDAS) / p(CA)(ARTICULOS)
    p(DNI)(p(DNI, CT, CA)(VENTAS) / A)
```