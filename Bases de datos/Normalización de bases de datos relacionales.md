La normalizacion de bases de datos se refiere a modelar una base de datos relacional y saber como se genera una estructura relacional de datos valida.

Normalizando, se llega a un diseño de la base de datos a la que el resto de especialistas, normalizando, también llegarían. 
Por ello, cuando normalizamos, no modificamos los datos, y solemos partir desde el mismo nivel de conocimiento sobre el tema a modelar en la base de datos, lo que hacemos es llegar a cumplir los siguientes objetivos. 

Los objetivos de normalizar entonces son:
* Reducir la redundancia de datos e inconsistencias
* Facilitar el mantenimiento de los datos y programas que los acceden
* Evitar anomalías al operar los datos
* Reducir el impacto de los cambios en los datos

para llegar a esta forma es requerido entender un conjunto de fundamentos

##### Dependencia Funcional:

La **DF** es una limitación al conjunto de relaciones aceptadas según las formas normales, donde se establece una relación fuerte y persistente entre dos atributos, si se cambia uno esto determina un cambio de valor en otro

Es decir, dada una relación R, el atributo Y de R depende funcionalmente del atributo X de R, denotado 

$$
R.X  á R.Y
$$

significa que un solo valor Y en R esta asociado a cada valor X en R

esto nos quiere decir que, que si R.X á R.Y, entonces, para dos tuplas de mismo R.X, forzosamente R.Y debe ser el mismo.
##### Dependencia Funcional Completa:

Dada la relación R, el atributo Y de R depende funcionalmente de forma completa del atributo X de R, si depende funcionalmente de X y no depende funcionalmente de ningún subconjunto de X

aquí hablamos de subconjunto, ya que ponemos considerar que X es una combinación de múltiples atributos, donde si hay dependencia funcional completa por parte de Y, entonces si tomamos un subconjunto de X y lo quitamos de la relación, Y ya no tendrá ninguna dependencia funcional, haciéndola así funcional completa de X

si X es un solo atributo, entonces cualquier dependencia funcional de la forma 
R.X  á R.Y es **siempre completa**.

##### Dependencia Funcional Transitiva:

la dependencia funcinal transitiva se da cuando un atributo Y de R depende funcionalmente de X, y a su vez, dada otra relacion S, un atributo Z de S depende funcionalmente de Y de S, entonces por transitvidad, Z de S depende funcionalmente de X de R

por ejemplo, si tenemos una relación que se basa en las prestaciones de libros, se puede dar que un préstamo contenga el ID del libro, y el genero, a su vez, un libro contiene un genero en su relación, por lo tanto, se da una dependencia funcional transitiva, y puede descartarse el genero en la relación préstamo, ya que este se puede conocer por el libro 

##### Formas normales:

**Primera forma Normal (1FN)**

Una relación se encuentra en 1FN si todos los dominios simples de los atributos contienen valores atómicos (nombre, fecha, teléfono -uno solo- son ejemplos) y sus claves primarias son monovalentes (los valores de los atributos no se repiten en otras tuplas refiriéndonos a claves primarias)

ejemplos de valores no atómicos pueden ser atributos como 
"Sucursal y numero de factura"

La corrección seria crear 2 atributos distintos para "sucursal" y "numero de factura".

Ejemplos de valores no monovalentes son los siguientes

![[Pasted image 20250322214406.png]]

donde podemos ver que se repiten los valores en sucursal y numero de factura en varias tuplas.

![[Pasted image 20250322214730.png]]

la corrección es mas compleja, requiriendo transformar la relación o incluso dividirla en subrelaciones haciendo uso de claves foráneas

**Segunda Forma Normal (2FN)**

Una relación 1FN es 2FN si ademas, todos los atributos No clave, dependen por completo de la clave primaria, y no debería poder encontrarse dependencias con otros subconjuntos No clave.

![[Pasted image 20250322235345.png]]

por ejemplo, en detalle factura, id articulo, que forma parte de la PK, nombre articulo tiene una dependencia con esta, no cumpliendo así la condición de la 2FN

la solución para estos casos es extraer el atributo No clave y enviarlo a otra relación donde la PK sea el atributo con el que creaba una dependencia, luego, en la relación original, hacer FK al atributo del que dependía el No clave.

![[Pasted image 20250323002923.png]]

**Tercera Forma Normal (3FN)**

Una relación 2FN es 3FN si ademas ningún subconjunto de atributos no claves tiene dependencia funcional entre si, donde transitivamente dependen también de la clave primaria PK


![[Pasted image 20250323003555.png]]

Puede verse que esto mismo sucede en la forma 2FN, donde "cliente" depende de "id cliente" y "forma de pago" depende de "id pago"
la solución es la misma que en la 2FN.

![[Pasted image 20250323003740.png]]

Resultando así normalizada en 3FN. habiendo cumplido los objetivos de:
* Reducir la redundancia de datos e inconsistencias
* Facilitar el mantenimiento de los datos y programas que los acceden
* Evitar anomalías al operar los datos
* Reducir el impacto de los cambios en los datos

### Relaciones uno a muchos

las relaciones uno a muchos se dan cuando uno de los modelos, referencia a otro modelo con la posibilidad de ser muchos, es decir, una relación A puede asociarse a mas de una relación B.

para situaciones así, para mantener la normalizacion de la base de datos, la relación que se asocia a muchos, debe ser reverenciada en las relaciones individuales mediante una clave foránea, 
Y NO POR EL CONTRARIO, la relación tener múltiples claves foráneas a cada relación asociada, esto debe evitarse.

Un ejemplo, aquí, un perro puede tener múltiples posibles padres, cada uno con una probabilidad distinta de ser el padre verdadero, por lo tanto, cada relación padre, tiene una clave foránea a un perro, y una clave foránea al perro mismo que es padre, formando así una clave primaria única, y que resuelve el problema de uno a muchos

![[Pasted image 20250409220313.png]]

### Relaciones muchos a muchos

las relaciones muchos a muchos son algo mas complejas, y suelen requerir utilizar una relación intermedia para solucionarlas

la relación muchos a muchos se da cuando una relación A se asocia a múltiples relaciones B, y a su vez, cada relación B se asocia a múltiples relaciones A

la solución común es utilizar una relación intermedia C, con PK formada por las claves foráneas de A y B, haciendo que se asocie con una relación A y una relación B, sirviendo así de camino entre las relaciones A y B

Un ejemplo, se da la situación en la que una empresa puede tener múltiples directores, a su vez, luego, una persona puede pertenecer a la junta de múltiples empresas, es decir, asociarse a mas de una empresa como director.
Para solucionar esto, podemos crear una relación intermedia denominada director, un director tiene como clave primaria las claves foráneas de una persona y una empresa, 
de este modo, las empresas tienen múltiples directores, y a su vez, una persona puede ser director de múltiples empresas.