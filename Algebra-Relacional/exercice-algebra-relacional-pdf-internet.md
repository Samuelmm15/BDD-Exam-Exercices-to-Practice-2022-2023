# Exercice PDF Internet 

Sean las siguientes tablas la base de datos usada:

CLIENTES(No Cliente, Nombre, Dirección, Teléfono, Población)\
Clave: numero-cliente\
Significado: Tabla que almacena todos los clientes de la empresa.

PRODUCTO(Cod Producto, Descripción, Precio)\
Clave: cod-producto\
Significado: Tabla que almacena todos los productos que ofrece la empresa.

VENTA(Cod Producto, No Cliente, Cantidad, Id Venta)\
Clave: id-venta\
Significado: Tabla que almacena la venta de productos hacia los determinados clientes de la empresa.

Implementación de las siguientes consultas haciendo uso de álgebra relacional:

1. Muestra el nombre de los clientes de Palencia.\
Se necesita la tabla: CLIENTES
```sql
p(nombre)(s(direccion='Palencia')(CLIENTES))
```

2. Indicar el código y descripción de los productos cuyo código coincida con su descripción.\
Se necesita la tabla: PRODUCTO
```sql
p(cod-producto, descripcion)(s(cod-producto=descripcion)(PRODUCTOS))
```

3. Obtener el nombre de los clientes junto con el identificador de venta y la cantidad vendida, de aquellos
productos de los que se vendieron más de 500 unidades.\
Se necesita la tabla: VENTA, CLIENTES
```sql
p(nombre, id-venta, cantidad)(s(cantidad > 500)(VENTA * CLIENTES))
```

4. Clientes que no han comprado nada.\
Se necesitan las tablas: VENTA, CLIENTES
```sql
p(nombre, no-cliente)(CLIENTES) - p(no-cliente)(VENTA)
```

Otra posibilidad de esto anterior, que nos permite mostrar únicamente el nombre de los clientes como resultado es:
```sql
p(nombre)(CLIENTES) - p(nombre)(CLIENTES * VENTA)
```

5. Nombre de los clientes que han comprado todos los productos de la empresa.\
Se necesitan las tablas: CLIENTES, VENTA, PRODUCTOS
```sql
(p(nombre, no-cliente)(CLIENTES)) / (p(no-cliente)(VENTA * PRODUCTOS))
```

`NOTA:` Mientras se realizaba la consulta, destacamos la aparición de la palabra `todos` en la descripción de la
consulta, por tanto, se ha optado por el uso de el operador de cociente, lo que nos permite comprobar y establecer
que el uso de dicho operador para este caso es correcto.

Otra alternativa a esto según las soluciones para que solo aparezca la columna de nombre es:
```sql
p(nombre)((p(nombre, no-cliente, cod-producto)(CLIENTE * VENTA)) / p(cod-producto)(PRODUCTOS))
```