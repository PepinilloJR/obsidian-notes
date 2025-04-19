para la manipulación de datos en una BD relacional, se provee lo que se denomina ***álgebra relacional***, que proporciona los fundamentos formales para las operaciones que pueden realizarse en el modelo relacional, y se usa como ***base para implementar y optimizar las consultas en un RDBMS***

debemos recordar la propiedad de cerradura, esto es, el conjunto de operadores que componen al álgebra relacional, al operar sobre una relación o dos relaciones, producen otra nueva relación.

## Operadores tradicionales

Los operadores están basados en las operaciones de conjuntos del álgebra de conjuntos, estos son la unión, la intersección, la diferencia, y el producto cartesiano
### UNIÓN - binaria 

La unión tiene la condición de que las relaciones sean compatibles, para ello deben tener mismas cabeceras y mismos dominios para los campos de valores.

A U B -> resulta en una relación con todos los elementos de A y todos los elementos de B (sin repetir)

Esta es conmutativa y asociativa

![[Pasted image 20250418192448.png]]

### DIFERENCIA - binaria
La diferencia tiene la misma condición que la unión

A - B -> resulta en una relación con todos los elementos de A que no están en B

![[Pasted image 20250418193011.png]]

### INTERSECCIÓN - binaria
La diferencia tiene la misma condición que la unión, mismo dominio y cabecera para los campos de ambas relaciones

A ^ B -> resulta en una relación con todos los elementos de A que también están en B

![[Pasted image 20250418193209.png]]

Las tablas resultantes hasta hora de estas operaciones tendrán siempre las mismas columnas, esto es diferente en la siguiente operación.
### PRODUCTO CARTESIANO - binaria
El producto cartesiano no requiere que sean compatibles por unión (mismas cabeceras y dominio), estas pueden ser disjuntas

A X B -> resulta en una relación con la combinación de todas las filas de A con todas las filas de B, donde tendrá todas las columnas de ambas tablas, y una fila por cada combinación posible de filas, específicamente m x n filas, siendo m el numero de filas de A y n el numero de filas de B

teóricamente es conmutativo al no haber orden en las filas y columnas de la tabla, pero ***No es conmutativo*** al importar en la implementacion el orden de las columnas

por ejemplo:

![[Pasted image 20250418203013.png]]

el resultado del producto cartesiano sera:

![[Pasted image 20250418203032.png]]

### Operadores relacionales especiales

Los operadores especiales se alejan del álgebra de conjuntos.
### Selección - unaria

La selección obtiene una relación que contiene un subconjunto de filas de otra relación, que verifican una condición.

![[Pasted image 20250418210227.png]]

Por ejemplo, la siguiente operación:

![[Pasted image 20250418210252.png]]

Esto resultara en una relación con las mismas columnas de Estudiantes, pero solo las filas cuyo campo Legajo es mayor a 14000.
Permite utilizar operadores lógicos en las expresiones booleanas tales como AND - OR - NOT.

![[Pasted image 20250418210414.png]]

### Proyección - unaria

La proyección obtiene una relación que contiene a un subconjunto de las columnas de otra relación, especificando la lista de atributos que se quieren mantener o recuperar, cabe aclarar que se obtendrán todas las filas.

![[Pasted image 20250418210641.png]]

Se deben entonces especificar el listado de cabeceras que se quiere obtener.

![[Pasted image 20250418210654.png]]

Existe adicionalmente, la ***proyección generalizada*** que permite aplicar operaciones sobre las columnas seleccionadas antes de crear la relación, denominadas funciones

![[Pasted image 20250419153101.png]]

Donde F1, F2,...Fn son funciones que se aplican a las columnas, las cuales pueden involucrar constantes, para realizar productos, sumas, divisiones, etc.

Por ejemplo:

![[Pasted image 20250419152702.png]]


### Reunión / JOIN / Concatenación - binaria

JOIN concatena filas de dos tablas basándose en una condición booleana, entre los atributos de las diferentes tablas.
La relación resultante tendrá como columnas a la concatenación de ambas tablas.

Si existen atributos comunes en ambas tablas, podría dar error o ser manejado por el sistema de bases de datos.

![[Pasted image 20250418210954.png]]

Existen algunas variantes, específicamente el INNER JOIN/EQUIJOIN une dos tablas bajo una condición, cuando los valores de una columna que tienen en común se usan en la condición y presentan valores iguales, el resultado incluirá las dos columnas comunes a ambas tablas lo que resultara en una columna superflua. Luego esta el JOIN NATURAL, este requiere que haya columnas comunes entre ambas tablas para funcionar, y usara automáticamente estas columnas comunes como condición para la combinación, luego eliminando una de las columnas superfluas 

### OUTER JOIN / Reunión externa / Concatenación externa - binaria

El OUTER JOIN devolverá una tabla concatenando las filas que verifiquen la condición lógica, pero también aparecerán concatenadas aquellas filas que no hayan verificado la condición lógica, apareciendo, dependiendo del tipo de OUTER JOIN, las columnas de la fila de una de las relaciones con valores null.

Left Outer Join -> si se tienen las relaciones A y B, las filas no coincidentes de A se concatenan con valores null correspondientes a la falta de una fila coincidente de B, con los campos de esta

Right Outer Join -> si se tienen las relaciones A y B, las filas no coincidentes de B se concatenan con valores null correspondientes a la falta de una fila coincidente de A, con los campos de esta

Full Outer Join -> si se tienen las relaciones A y B, las filas no coincidentes de B se concatenan con valores null correspondientes a la falta de una fila coincidente de A, con los campos de esta. Las filas no coincidentes de A se concatenan con valores null correspondientes a la falta de una fila coincidente de B, con los campos de esta

![[Pasted image 20250419143849.png]]


Veamos un ejemplo:

![[Pasted image 20250419143136.png]]
(a la izquierda el resultado de un Left Outer Join, al medio el resultado de un Full Outer Join, y a la derecha el resultado de un Right Outer Join)

### División - binaria

La División A % B requiere que los atributos del divisor (B) estén incluidos en la cabecera del dividendo (A), es decir, que minimamente, todos los atributos del divisor estén presentes en los atributos del dividendo.

El resultado de una división A % B es una relación C cuyos campos son todos los de A que no están en B.
Donde las filas de C son todas aquella de A que aparecen en combinación con cada fila de B.

Por ejemplo:

![[Pasted image 20250419144852.png]]

Aquí, el resultado de hacer la operación A % B sera:

![[Pasted image 20250419144922.png]]

Podemos ver que como en A, solo cuando uno de los valores en Alumno, se combinan con todos los valores presentes de Materia en B, la división la incluirá en la tabla resultante, con solo los campos únicos de A que no son de B, en este caso Alumnos.

La forma mas intuitiva de pensarlo es ***"¿Quiénes de A están relacionados con todos los elementos de la tabla B?"***

### Renombrar - Unaria

La operación de renombrar se encarga de renombrar tablas o campos de una relación, especialmente útil cuando se están por usar relaciones en una operación que comparten mismo nombre o nombre de sus campos

![[Pasted image 20250419152040.png]]

### Funciones de agregación

Las funciones de agregación son un tipo de peticion que no pueden ser expresadas a través del álgebra relacional, pero que son necesarias para calcular ***funciones matemáticas de agregación*** (Una función de _agregación_ toma un conjunto de valores y devuelve un único resultado de valor) sobre las tablas de valores, estas se aplican considerando uno o mas campos de la relación.

Las mas comunes son SUM (sumatoria), AVG (media), MAX (valor máximo), MIN (valor mínimo), COUNT (conteo de cantidad de filas)

Ejemplo de uso y notación: 

![[Pasted image 20250419153705.png]]

Debemos considerar que aquí el resultado NO sera numérico, si no una tabla, y esta tabla podrá tener dos posibles formas: ***Agregación global*** vs ***Agregación con agrupamiento***
(Group by "atributo").

Cuando se realiza la agregación, si no se especifica un atributo de agrupamiento, se realiza una ***agregación global***, y el resultado sera una tabla de una única fila con el resultado de aplicar la función a todas las filas usando el campo especificado.

Sin embargo, si se especifica un atributo de agrupamiento:

![[Pasted image 20250419160736.png]]
(atributo de agrupamiento legajo)

Entonces el resultado sera una tabla con una fila por cada valor del atributo de agrupamiento, que como campos tendrá el propio atributo de agrupamiento, y el resultado de aplicar la operación a un ***grupo*** de filas que coincidían con este valor del atributo de agrupamiento, y no a la tabla completa (de aquí porque se denomina agrupamiento)

por ejemplo la siguiente operación:

![[Pasted image 20250419160736.png]]

Aplicada a la tabla exámenes:

![[Pasted image 20250419161734.png]]
Resultara en:

![[Pasted image 20250419161745.png]]

Esto es igual para todas las otras operaciones agregadas mencionadas.


Ejemplo aplicado sobre una tabla usando AVG:

![[Pasted image 20250419154003.png]]

### Relaciones temporales

Las relaciones temporales permiten subdividir operaciones complejas y anidadas.
Es poco común que solo apliquemos una sola operación a las relaciones cuando sea hora de manipularlas, por lo tanto pueden crearse operaciones anidadas complejas y difíciles de leer.

Puede entonces guardarse como una relación temporal, el resultado de cada una de estas operaciones, y usar esta para aplicarlo en la siguiente operación anidada.

Por ejemplo:

![[Pasted image 20250419161959.png]]
