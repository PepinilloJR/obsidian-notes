
Primero veremos las consultas sumarias sin agrupación, que resultaran en una sola fila con tantas columnas como consultas sumarias hay, y luego veremos las consultas sumarias agrupadas, que explicaremos en su momento.

### Consultas sumarias

Las consultas sumarias permiten resumir datos de una BD, mediante funciones que aceptan una columna como argumento, y producen un dato resumen ***en una sola fila***.

Funciones de COLUMNA estándar:
-> SUM (columna): calcula total de la columna
-> AVG (columna): calcula promedio
-> COUNT (columna): cuenta cantidad de valores
-> MAX (columna): obtiene el máximo valor
-> MIN (columna): obtiene el mínimo valor

Todas ignoran los valores NULL, ***una columna NULL no aporta ni resulta en error para la consulta sumaria***.

***SI TODA LA COLUMNA TIENE NULL: 
-> SUM,AVG,MIN y MAX*** devuelven ***NULL***. 
***-> COUNT*** devuelve 0.

Por ejemplo:

Dada la tabla:

| id  | fecha      | monto  |
| --- | ---------- | ------ |
| 1   | 2022-01-05 | 150.00 |
| 2   | 2022-01-10 | 320.00 |
| 3   | 2022-02-01 | 210.00 |
| 4   | 2022-01-25 | 180.00 |
| 5   | 2021-01-10 | 100.00 |

Conseguir la sumatoria del monto de todas las facturas de enero 2022

```
SELECT SUM ( F.monto ) as SUMATORIA
FROM Facturas F
WHERE MONTH(F.fecha)=01 AND YEAR(F.fecha)=2022
```

Resultado:

(el id no es técnicamente parte de la tabla)

| id  | SUMATORIA |
| --- | --------- |
| 1   | 650.0     |
Mostrar la cantidad total de facturas desde el año 2019

```
SELECT COUNT ( * ) as CANTIDAD
FROM Facturas F
WHERE YEAR(F.fecha)>=2019
```

Resultado:

| id  | CANTIDAD |
| --- | -------- |
| 1   | 5        |
Mostrar el Monto Promedio Facturado en 2022

```
SELECT AVG(F.monto) as PROMEDIO2022
FROM Facturas F
WHERE YEAR(F.fecha) = 2022;
```

Resultado:

| id  | PROMEDIO2022 |
| --- | ------------ |
| 1   | 400.0        |

### Consultas agrupadas 

Ya vimos esto en las consultas simples.

El agrupamiento (GROUP BY) genera grupos de filas que verifican un mismo valor para una columna, a cada tabla-grupo se le aplica la operacion de agregación, y se forma una tabla compuesta por una fila por cada valor de agrupación, junto a los valores de agregación resultantes.

Por ejemplo:

Tenemos la tabla facturas:

![[Pasted image 20250521195749.png]]

Si nosotros queremos una tabla que nos indique para cada cliente el total de todas sus facturas, necesitamos usar agrupación.

realizando la siguiente consulta:

```
SELECT F.idcliente , SUM (F.total) AS comprado
FROM Facturas F
WHERE YEAR(F.fecha)=2018
GROUP BY F.idcliente
```

Primero obtendremos la tabla agrupada por el id del cliente.

![[Pasted image 20250521200226.png]]

Finalmente, para cada grupo se genera una fila con el SUM nombrado comprado, nosotros podemos entonces seleccionar este, y podemos seleccionar tambien la columna del IdCliente, siempre y cuando este forme parte del GROUP BY, es decir, que sea una columna de agrupacion.

El resultado final sera entonces:

![[Pasted image 20250521200520.png]]

### Consultas agrupadas con HAVING

HAVING nos sirve para aplicar condiciones de búsqueda a la tabla agrupada final, generalmente lo usamos para aplicar condiciones sobre los valores agregados.

Esto es porque en WHERE se aplica antes del GROUP BY
entonces seria imposible filtrar los valores agregados, porque todavía no existen.

Por ejemplo:

Teniendo la tabla:

![[Pasted image 20250521201219.png]]

Queremos obtener Idcliente y total comprado por cliente, durante mayo de 2018 y cuando el total supera los $5000.

La siguiente consulta resuelve esto:

```
SELECT F.idcliente , SUM (F.total)
FROM Facturas F
WHERE MONTH(F.fecha)=5 AND YEAR(F.fecha)=2018
GROUP BY F.idcliente
HAVING SUM(F.total) > 5000
```

Esto resulta en:

![[Pasted image 20250521201314.png]]
(la primera tabla es el resultado de aplicar el where y luego considerar a cada una como un grupo)

### Consideraciones sobre HAVING y GROUP BY

-> El motor devuelve una fila por cada grupo conformado (GROUP BY).
-> WHERE incluye condiciones para las filas mientras que HAVING incluye condiciones para el resultado de funciones sumarias.
-> HAVING comúnmente va con GROUP BY, puede estar solo y el grupo es uno.
-> El resultado de una consulta agrupada sale ordenada por atributos del GROUP BY, si aplicamos un ORDER BY.
-> NUNCA va una función sumaria (SUM, COUNT, MIN, MAX, AVG) en WHERE. Las funciones sumarias SIEMPRE van en HAVING.
-> Las columnas del SELECT sin función de grupo (es decir, columnas que no son resultado de aplicar una función sumaria) DEBEN estar en la cláusula GROUP BY, o resultara en error.
