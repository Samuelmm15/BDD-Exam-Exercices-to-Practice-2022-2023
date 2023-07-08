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

Realizar la implementación de las distintas consultas haciendo uso de cálculo relacional de tuplas CRT  
(`soluciones apuntes página 22`):

1. Personas que un mismo día han comprado al menos 2 artículos distintos en una misma tienda.\
Se necesitan las tablas: VENTAS
```sql
{<DNI> | ∃(CT, CA, NU, F)((<DNI, CT, CA, NU, F> ∈ VENTAS) ^ ∃(CT', CA', NU', F')((<DNI, CT', CA', NU', F'> ∈ VENTAS) 
    ^ (CT' = CT) ^ (CA' ¬= CA) ^ (F' = F)))}
```

2. Artículos que sólo están disponibles en la tienda T1.\
Se necesitan las tablas: TIENDAS
```sql
{<CA> | ∃(CT, PR)((<CT, CA, PR> ∈ TIENDAS) ^ (CT = 'T1') ^ ¬∃(CT', PR')((<CT', CA, PR'> ∈ TIENDAS) ^ (CT' ¬= CT)))}
```

3. Artículo más barato de la tienda T1.\
Se necesitan las tablas: TIENDA
```sql
{<CA> | ∃(PR)((<'T1', CA, PR> ∈ TIENDAS) ^ ¬∃(PR')((<'T1', CA, PR'> ∈ TIENDAS) ^ (PR' < PR))}
```

4. Personas que han comprado un mismo artículo en cada tienda.\
Se necesitan las tablas: VENTAS, TIENDAS
```sql
{<DNI> | ∃(CT, CA, NU, F)((<DNI, CT, CA, NU, F> ∈ VENTAS) ^ ∀(CT', CA', PR')((<CT', CA', PR'> ∈ TIENDAS) ^ 
    ∃(CT'', CA'', NU'', F'')((<DNI, CT'', CA'', NU'', F''> ∈ VENTAS) ^ (CA' = CA) ^ (CA'' = CA) ^ (CT'' = CT')}
```

5. Tiendas que en un mismo día han vendido al menos una unidad de cada uno de sus artículos disponibles.\
Se necesitan las tablas: VENTAS, TIENDAS
```sql
{<CT> | ∃(DNI, CA, NU, F)((<DNI, CT, CA, NU, F> ∈ VENTAS) ^ ∀(CT', CA', PR')((<CT', CA', PR'> ∉ TIENDAS)
    v (∃(DNI'', CT'', CA'', NU'', F'')((<DNI'', CT'', CA'', NU'', F''> ∈ VENTAS) ^ (CT'' = CT) ^ (CT' = CT) ^ (F'' = F)
        ^ (NU >= 1) ^ (CA' = CA''))))}
```