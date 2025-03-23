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
##### Formas normales:

**Primera forma Normal (1FN)**

Una relación se encuentra en 1FN si todos los dominios simples de los atributos contienen valores atómicos (nombre, fecha, teléfono -uno solo- son ejemplos) y monovalentes (los valores de los atributos no se repiten en otras tuplas refiriéndonos a claves primarias)

ejemplos de valores no atómicos pueden ser atributos como 
"Sucursal y numero de factura"

La corrección seria crear 2 atributos distintos para "sucursal" y "numero de factura".

Ejemplos de valores no monovalentes son los siguientes

![[Pasted image 20250322214406.png]]

donde podemos ver que se repiten los valores en sucursal y numero de factura en varias tuplas.

![[Pasted image 20250322214730.png]]

la corrección es mas compleja, requiriendo transformar la relación o incluso dividirla en subrelaciones haciendo uso de claves foráneas

**Segunda Forma Normal (2FN)**

Una relación 1FN es 2FN si ademas, todos los atributos No clave, dependen por completo de la clave primaria, y no debería poder encontrarse dependencias con otros atributos No clave.

![[Pasted image 20250322235345.png]]

por ejemplo, en detalle factura, id articulo, que forma parte de la PK, nombre articulo tiene una dependencia con esta, no cumpliendo así la condición de la 2FN

la solución para estos casos es extraer el atributo No clave y enviarlo a otra relación donde la PK sea el atributo con el que creaba una dependencia, luego, en la relación original, hacer FK al atributo del que dependía el No clave.

![[Pasted image 20250323002923.png]]

**Tercera Forma Normal (3FN)**

Una relación 2FN es 3FN si ademas ningún subconjunto de atributos no claves tiene dependencia funcional entre si, donde transitivamente dependen también de la clave primaria PK.

![[Pasted image 20250323003555.png]]

Puede verse que esto mismo sucede en la forma 2FN, donde "cliente" depende de "id cliente" y "forma de pago" depende de "id pago"
la solución es la misma que en la 2FN.

![[Pasted image 20250323003740.png]]

Resultando así normalizada en 3FN. habiendo cumplido los objetivos de:
* Reducir la redundancia de datos e inconsistencias
* Facilitar el mantenimiento de los datos y programas que los acceden
* Evitar anomalías al operar los datos
* Reducir el impacto de los cambios en los datos