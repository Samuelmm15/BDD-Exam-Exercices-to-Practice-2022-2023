# Anotaciones para Examen

## 1. Solicitud de parejas de datos.

De manera general, si se realiza la solicitud de una consulta que obtenga las parejas de datos, de manera normal,
si las parejas de datos se solicitan sobre la misma tabla, se debe de realizar la implementación de dos tablas
de la misma con distinto nombre y realizando aquellas operaciones que sean necesarias para poder obtener dichas parejas
de datos. Por ejemplo, si solicitan parejas de nombres de coches para la tabla COCHE, se hace uso de C1 = C2 = COCHE,
por tanto, se hace uso de dichas dos tablas, permitiendo obtener las parejas de datos que son solicitadas
en el propio ejercicio a implementar.

## 2. Números especificados dentro de t en CRT

El número que se especifica dentro de t en el CRT, hace referencia al número de columnas que tendrá como resultado
la tabla tras la realización de la consulta, es por ello que, en la mayoría de las situaciones se genera que se tiene
t1, mientras que en ciertos casos se tiene t2 cuando existe más de una columna como resultado de la consulta.

Para poder entender esto se tiene:
```sql
-- Para el caso de que se produzca una única columna como resultado.
{t1 | (p = t)}
```

```sql
-- Para el caso de que se produzca más de una columna, para este caso dos.
{t2 | (p = t1) ^ (q = t2)}
```