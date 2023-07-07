# 26 Janury Exam Cadena de Tiendas

Relación de las distintas tablas usadas en la base de datos:

ARTÍCULOS (CA, CAT)\
SIGNIFICADO: El artículo CA es decategoría CAT.\
CLAVE PRIMARIA: (CA)

TIENDAS(CT, CA, PR)\
SIGNIFICADO: La tienda con código CT dispone del articulo CA a un precio de PR euros.\
CLAVE PRIMARIA: (CT, CA)\
CLAVE AJENA: (CA)

VENTAS(DNI, CT, CA, NU, F)\
SIGNIFICADO: La persona con n i DNI. ha comprado en la tienda CT, NU unidades del articulo CA, en la fecha F.\
CLAVE PRIMARIA: (DNI, CT, CA, F)\
CLAVE AJENA: (CT, CA)

Realizar la implementación de las distintas consultas haciendo uso de álgebra relacional (`soluciones en la página 18
`):

1. Tiendas con artículos de las categorías C1 o C2.\
Se necesitan las tablas: TIENDAS, ARTÍCULOS
```sql
A = p(CT, CA)(s(CAT = 'C1')(TIENDAS * ARTÍCULOS))
B = p(CT, CA)(s(CAT = 'C2')(TIENDAS * ARTÍCULOS))
    p(CT)(A U B)
```

2. Personas que han comprado el artículo A1 y A2 en una misma tienda.\
Se necesitan las tablas: VENTAS
```sql
A = p(DNI, CT)(s(CA = 'A1')(VENTAS))
B = p(DNI, CT)(s(CA = 'A2')(VENTAS))
    p(DNI)(A * B)
```

3. Tienda que tiene el artículo A1 con menor precio.\
Se necesitan las tablas: TIENDAS
```sql
A = p(CT, PR)(s(CA = 'A1')(TIENDAS))
B = P(CT, PR)(S(CA = 'A1')(TIENDAS))
C = p(A.CT, A.PR)(s(A.PR > B.PR)(A x B))
    p(CT)(p(CT, PR)(TIENDAS) - C)
```

4. Personas que han comprado al menos un artículo en cada tienda.\
Se necesitan las tablas: VENTAS, TIENDAS
```sql
p(DNI)(p(DNI, CT, CA)(VENTAS) / p(CT, CA)(TIENDAS))
```

En la solución de los apuntes, no hace uso del código del artículo, pero, yo si que lo veo necesario, `posible error`.

5. Personas que han comprado en alguna tienda todos los artículos de alguna categoría.\
Se necesitan las tablas: VENTAS, ARTÍCULOS
```sql
-- R = DNI
-- A = CT
-- A' = CA
-- T = CAT
p(DNI)(p(DNI, CT, CA)(VENTAS) - p(DNI, CT, CA)((p(DNI, CA)(VENTAS) x p(CA, CAT)(ARTÍCULOS)) - p(DNI, CT, CA)(VENTAS)))
```

Esta consilta anterior es incorrecta, la solución para esto es la siguiente:
```sql
-- R = DNI
-- A = CAT
-- T = CA
A = p(DNI)(VENTAS) x p(CA, CAT)(ARTÍCULOS)
B = A - p(DNI, CA, CAT)(VENTAS * ARTÍCULOS)
C = p(DNI, CAT)(VENTAS * ARTÍCULOS) - p(DNI, CAT)(B)
    p(DNI)(C)
```