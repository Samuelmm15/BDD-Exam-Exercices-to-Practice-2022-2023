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
(`soluciones apuntes página 20`):

1. Personas que en un mismo día han comprado al menos 2 artículos distintos en una misma tienda.\
Se necesitan las tablas: TIENDAS
```sql
Para dom(V1) = VENTAS, dom(V2) = VENTAS
{t1 | ∃(V1)((V1[DNI] = t[DNI]) ^ ∃(V2)((V2[DNI] = V1[DNI]) ^ (V2[F] = V1[F]) ^ (V2[CT] = V1[CT]) ^ (V2[CA] ¬= V1[CA])))}
```

2. Artículos que sólo están disponibles en la tienda T1.\
Se necesitan las tablas: TIENDAS
```sql
Para dom(T1) = TIENDAS, dom(T2) = TIENDAS
{t1 | ∃(T1)((T1[CA] = t[CA]) ^ ¬∃(T2)((T2[CT] ¬= T1[CT]) ^ (T2[CA] = T1[CA])))}
```

3. Artículo más barato de la tienda T1.\
Se necesitan las tablas: TIENDAS
```sql
Para dom(T1) = TIENDAS, dom(T2) = TIENDAS, dom(T3) = TIENDAS
{t1 | ∃(T1)((T1[CA] = t[CA]) ^ (T1[CT] = 'T1') ^ ∀(T2)∃(T3)((T2[CT] = T1[CT]) ^ (T3[CT] = T2[CT]) ^ (T3[CA] = T1[CA]) 
    ^ (T3[PR] <= T2[PR])))}
```

Otra manera mucho más sencilla de hacer esta consulta anterior es:
```sql
Para dom(T1) = TIENDAS, dom(T2) = TIENDAS
{t1 | ∃(T1)((T1[CA] = t[CA]) ^ (T1[CT] = 'T1') ^ ¬∃(T2)((T2[CT] = T1[CT]) ^ (T2[CA] = T1[CA]) ^ (T2[PR] < T1[PR])))}
```

4. Personas que han comprado un mismo artículo en cada tienda.\
Se necesitan las tablas: VENTAS, TIENDAS
```sql
Para dom(V1) = VENTAS, dom(T1) = TIENDAS, dom(V2) = VENTAS
{t1 | ∃(V1)((V1[DNI] = t[DNI]) ^ ∀(T1)∃(V2)((V2[CA] = V1[CA]) ^ (T1[CA] = V2[CA]) ^ (V2[DNI] = V1[DNI])))}
```

5. Tiendas que en un mismo día han vendido al menos una unidad de cada uno de sus artículos disponibles.\
Se necesitan las tablas: VENTAS, TIENDAS
```sql
Para dom(V1) = VENTAS, dom(T1) = TIENDAS,  dom(V2) = VENTAS
{t1 | ∃(V1)((V1[CT] = t[CT]) ^ ∀(T1)((T1[CT] ¬= V1[CT]) v (∃(V2)((V2[CT] = V1[CT]) ^ (T1[CT] = V1[CT]) ^ (V2[F] = V1[F]) 
    ^ (V2[NU] >= 1) ^ (V1[CA] = T1[CA]))) ))}
```