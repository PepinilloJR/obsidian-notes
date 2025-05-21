Es por lo general útil realizar consultas de todo tipo en una base de datos, la herramienta principal que nos da SQL para ello es SELECT

hay dos cosas importantes que debemos considerar cuando usemos SELECT, una es entender que hace y las diferentes posibilidades de filtrado que nos da, la segunda, es entender el orden en que realiza operaciones implicadas en el SELECT

# Consultas simples con SELECT

Sintaxis general:

```

SELECT {lista de columnas}
FROM tablas
WHERE condiciones-filas
GROUP BY {lista de columnas}
HAVING condiciones-grupo
ORDER BY {lista de columnas} ASC|DESC


```

El orden de ejecución sera el siguiente:

- **FROM** – Se determina la(s) tabla(s) fuente y se realizan los _JOINs_ si hay.
    
- **WHERE** – Se filtran las filas según condiciones individuales (antes de cualquier agrupamiento).
    
- **GROUP BY** – Se agrupan las filas en función de las columnas indicadas.
    
- **HAVING** – Se filtran los grupos generados por `GROUP BY` (es como un `WHERE` pero aplicado a grupos).
    
- **SELECT** – Se eligen y calculan las columnas a mostrar (pueden incluir agregaciones como `SUM`, `AVG`, etc.).
    
- **ORDER BY** – Se ordenan los resultados finales.
    
- **LIMIT / OFFSET** (si existieran) – Se limita el número de filas devueltas.

Veamos cada una de las palabras clave:

# SELECT 

SELECT es el ultimo en ejecutarse, luego de tomar una tabla, filtrarla y aplicarle agrupamientos, ordenarla y limitar o adelantarse, se seleccionan las columnas que se quieren tomar de la tabla resultante de todas esas operaciones

Ejemplo

```
SELECT E.Nroemp,E.Apelemp,E.Nomemp,C.Catego,C.Descrip
```


# FROM

Indica obtener todas las filas de una tabla, si se realiza con multiples tablas

***FROM TablaA A, TablaB B***

Se obtiene todas las filas del producto cartesiano entre A y B

### WHERE

Se indican condiciones booleanas para seleccionar filas que cumplan ciertas condiciones (condiciones de búsqueda)

Las siguientes comparaciones son aplicables para múltiples tipos de datos (INTEGER, VARCHAR, DATETIME, ETC)

### -> Comparación: 

Opciones: =, <>,  <, > , <=, >=

ejemplo:

`WHERE E.columna > 40` 

### -> Rango

Sintaxis 

`WHERE E.columna BETWEEN value1 AND value2`

### -> Pertenencia a conjunto

Sintaxis

`WHERE E.columna IN (value1, value2, ... , valueN)`

### -> Correspondencia con patron: similar a un REGEX

opciones: 

% -> coincidencia con cualquier numero de caracteres
_ -> coincidencia con un solo cualquier carácter
LOWER | UPPER -> por defecto like no le importa si es upper o lower, podemos usar esto para hacer que le importe

Sintaxis 

`WHERE UPPER|LOWER(E.columna) LIKE 'patron'`

### -> De tipo

Sintaxis 

`WHERE E.columnas IS [TYPE]`

### -> Negación NOT

Puede usarse la negación para complementar las anteriores condiciones de búsqueda

Sintaxis

`WHERE E.columnas IS NOT [TYPE]`

`WHERE UPPER|LOWER(E.columna) NOT LIKE 'patron'`

`WHERE E.columna NOT IN (value1, value2, ... , valueN)`

`WHERE E.columna NOT BETWEEN value1 AND value2`

`WHERE E.columna NOT > 40` 

# GROUP BY

El agrupamiento esta específicamente definido para usarse con operaciones de agregación

El agrupamiento genera grupos de filas que verifican mismo valor para una columna, a cada grupo se le aplica la operacion de agregación, y se forma una tabla compuesta por una fila por cada valor de agrupación, junto a los valores de agregación 

```
SELECT COUNT(CustomerID), Country  
FROM Customers  
GROUP BY Country;
```

Notar que, toda columna especificada en el SELECT que no sea formada por una operacion de agregación, debe ser una columna de agrupación especificada en el GROUP BY o resultara en un error

Es aplicable luego HAVING, donde podremos aplicar condiciones de busqueda ahora para la tabla agrupada (GROUP BY y HAVING se aplican luego del WHERE)

Ejemplo:

![[Pasted image 20250518174622.png]]

Aplicamos:

```
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country;
```

Resulta entonces:

![[Pasted image 20250518174705.png]]

Si aplicáramos un HAVING:

```
SELECT COUNT(CustomerID), Country  
FROM Customers  
GROUP BY Country  
HAVING COUNT(CustomerID) > 5;
```

![[Pasted image 20250518174928.png]]


# ORDER BY

ORDER BY ordena la tabla resultante luego de los filtrados y agrupaciones

sintaxis 

`ORDER BY _column1, column2, ..._ ASC|DESC;`

realizara una ordenación por la columna que indiquemos, y tendremos que indicar si es un ordenamiento ascendente o descendente, puede realizar ordenamientos tomando en consideración múltiples columnas

# Consultas multitabla con SELECT

Una consulta multitabla es la recuperación de datos procedentes de 2 o mas tablas de la base de datos.

La forma mas básica de consulta multitabla es el producto cartesiano, en forma de:

```
SELECT *
FROM Empleados E, Categorias C
```
(recordemos que FROM con múltiples tablas devuelve el producto cartesiano, luego select selecciona todas las columnas de este)

Sobre este producto cartesiano, es posible trabajar con las mismas herramientas de la consulta simple.

Cada tabla tendra un alias, que usaremos para especificar cuando nos referimos a sus columnas o a las columnas de otras tablas que forma parte de la consulta.

`Alias.columna ...`

Por ejemplo: 

```
SELECT E.Nroemp,E.Apelemp,E.Nomemp,C.Catego,C.Descrip
FROM Empleados E, Categorias C
WHERE E.Catego = C.Id
```


# Uso del JOIN en consultas multitablas

hay una forma mas limpia y explicita de realizar los JOIN implícitos en las consultas multitabla, de modo que evitamos involucrar la clave externa que relaciona las tablas en las condiciones de búsqueda del WHERE

Usando las palabras clave JOIN ON

sintaxis: `FROM A JOIN B ON A.fk = B.pk`

podemos expresar:

```
SELECT E.Nroemp,E.Apelemp,E.Nomemp,C.Catego,C.Descrip
FROM Empleados E, Categorias C
WHERE E.Catego = C.Id
```

como:

```
SELECT E.Nroemp,E.Apelemp,E.Nomemp,C.Catego,C.Descrip
FROM Empleados E JOIN Categorias C ON E.Catego = C.Id
```

Sin embargo, existe una utilidad mas alla de la semántica, que puede ser útil para situaciones del tipo:
"buscar relación A con B pero ademas incluir donde no se cumple la relación"

Esto es con el OUTER JOIN (para esto es útil repasar álgebra relacional).

![[Pasted image 20250521191057.png]]

#### LEFT JOIN

Esto incluirá las concatenacion de las filas que cumplen la condicion y al mismo tiempo filas con los valores de empleados concatenadas con NULL del lado de categorias cuando no verifican la condicion

```
SELECT E.Apelemp,E.Nomemp,C.Descrip
FROM Empleados E LEFT JOIN Categorias C ON E.Catego = C.Id
```
#### RIGHT JOIN

Esto incluirá las concatenación de las filas que cumplen la condición y al mismo tiempo filas con los valores de categorías concatenadas con NULL del lado de empleados cuando no verifican la condición

```
SELECT E.Apelemp,E.Nomemp,C.Descrip
FROM Empleados E RIGHT JOIN Categorias C ON E.Catego = C.Id
```
#### FULL JOIN

Esto incluirá las concatenación de las filas que cumplen la condición y al mismo tiempo filas con los valores de categorías concatenadas con NULL del lado de empleados cuando no verifican la condición, y al mismo tiempo filas con los valores de empleados concatenadas con NULL del lado de categorías cuando no verifican la condición

```
SELECT E.Apelemp,E.Nomemp,C.Descrip
FROM Empleados E FULL JOIN Categorias C ON E.Catego = C.Id
```

## Autocomposición

La autocomposición refiere a combinar una tabla consigo misma. Esto es útil cuando hay relaciones jerárquicas o dependencias dentro de una misma entidad.

Por ejemplo, si quisiéramos obtener todos los empleados junto a sus jefes, y adicionalmente obtener a todos los jefes, que no tendrán un jefe asociado. Y si consideramos que solo tenemos la tabla empleados, y se define que un jefe es un empleado con FK NULL al empleado que seria su jefe, entonces la consulta resulta:

```
SELECT E.Apelemp,E.Nomemp,JEFE.Apelemp AS ApeJefe
FROM Empleados E LEFT JOIN Empleados JEFE ON E.Nrojefe = JEFE.Nroemp
```

Aquí, el LEFT JOIN nos dará primero, todos los empleados junto a su jefe, y adicionalmente, todos los empleados sin jefe (jefes) donde las columnas que pertenencerian al jefe son NULL. Luego, el SELECT seleccionara el apellido y nombre de los empleados y el apellido del jefe, que sera NULL para los jefes