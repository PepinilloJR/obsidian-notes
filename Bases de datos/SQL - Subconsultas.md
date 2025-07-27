
Una subconsulta es una sentencia SELECT embebida  (insertada, incrustada, etc.) en una clausula (generalmente WHERE) de otra sentencia SQL.

```
SELECT E.apellido, E.nombres
FROM Empleados E
WHERE E.fechaNac = (SELECT MAX(Em.fechanac)
FROM Empleados Em)
```

### Características

* Una subconsulta puede utilizar todas clausulas de SELECT menos ORDER BY 
* Se usa encerrado entre paréntesis
* Siempre se utiliza a la derecha del operador que requiere la consulta
* No puede haber UNION de otros SELECT, solo actúa como una tabla intermedia

```
SELECT nombre FROM empleados WHERE salario = 
( SELECT salario FROM tabla1 UNION SELECT salario FROM tabla2 );
```

Si puede haber en otros contextos 

```
SELECT *
FROM (
    SELECT nombre FROM empleados_arg
    UNION
    SELECT nombre FROM empleados_uru
) AS todos_los_empleados;
```

* Pueden incluir referencias externa


# Operaciones típicas con subconsultas

#### Comparación (=, <>, <, <=, >, >=)

compara un valor con el valor obtenido de la subconsulta (la subconsulta debe ser una consulta de agregación)

Ej: Listar apellido y nombres del empleado más joven

```
SELECT E.apellido, E.nombres
FROM Empleados E
WHERE E.fechaNac = (SELECT MAX(Em.fechanac)
FROM Empleados Em)
```

#### Pertenencia (IN)

Compara un único valor de datos con una columna producida por una subconsulta, para detectar si esta o no en el resultado de la subconsulta.

Ej: Obtener descripción de los productos no vendidos en el 2021

```
SELECT A.descrip
FROM Articulos A
WHERE A.id NOT IN (SELECT DISTINCT D.idArt 
				   FROM Detalle D JOIN Facturas F
				   ON D.nrofact=F.nrofact
				   WHERE YEAR(F.fecha)=2021)
```


#### Existencia (EXISTS)

Verifica si la subconsulta devuelve al menos una fila de resultados, no se usan los valores devueltos y por ello se recomienda usar una constante. No controla si la subconsulta devuelve más de una columna.

Ej.: Obtener descripción de productos no vendidos en 2021

```
SELECT A.descrip FROM Articulos A
WHERE NOT EXISTS (SELECT 1
                  FROM Detalle D JOIN Facturas F ON D.nrofact=F.nrofact
                  WHERE YEAR(F.fecha)=2021
                  AND D.idArt=A.id)
```