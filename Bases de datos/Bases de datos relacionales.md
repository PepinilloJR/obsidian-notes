el modelo relacional es el modelo mas importante y usado actualmente para las bases de datos orientados a registros.

Una base de datos relacional organiza los datos en una estructura tabular de filas (tuplas) y columnas de valores (atributos), **denominadas relaciones** (las tablas), se denominan así y se dice que es ***relacional*** porque esta basado en el concepto matemático de las relaciones en la teoría de conjuntos.

![[Pasted image 20250321172845.png]]

El numero de atributos se denomina ***grado de la relación***, es estático y es igual para todas las filas de la relación. al numero de filas ***cardinalidad***, es dinámico.

finalmente, denominaremos ***dominio*** al conjunto de posibles valores para cada atributo, siendo así **la restricción de lo que se quiere que el atributo represente**, 
por ejemplo, un Legajo es un numero entero positivo mayor que cero, siendo ese el dominio del atributo Legajo, el dominio es por lo tanto un conjunto de valores atómicos. esto se denomina como ***restricción de dominio*** 

El modelo relacional garantiza las siguientes propiedades ***ACID*** en sus datos:

* Atomicidad: Una transacción debe ejecutarse como unidad de trabajo, es decir como una única operación.
* Consistencia: Toda transacción llevará, la base de datos, de un estado válido a otro también válido y consistente.
* Aislamiento: Esto asegura que la realización de dos transacciones, sobre la misma información, sean independientes y no generen ningún tipo de error, asegurando que una operación no afecte a otras, es decir, cada transacción es independiente del resto.
* Durabilidad: asegura que, una vez realizada la transacción, esta persistirá y no se podrá deshacer.

#### Propiedades de las relaciones

El modelo relacional tendrá las siguientes propiedades en sus tablas de datos o relaciones:
1. No existe orden en las tuplas (registros) ni en las columnas (campos/atributos)
2. Cada fila debe ser y es única por al menos un atributo
3. Los valores son atómicos
4. Valor Null como valor desconocido, la falta de un valor en un atributo es posible

#### Restricciones del Modelo Relacional 

##### Restricciones de Dominio

##### Restricciones de Clave

Las restricciones de clave son también restricciones para los atributos, en este caso, para determinar su clasificación en base a condiciones

**Clave candidata**: una clave candidata es un atributo que cumple las siguientes condiciones 

* Unicidad: el valor del atributo es único en toda la vida de la relación, o una combinación única de múltiples atributos.

* Minimalidad: considerando una clave candidata de múltiples atributos, se le llama así a la condición de ser la mínima combinación de atributos posibles para cumplir con una condición de unicidad, haciéndola clave candidata

**Clave primaria**: una clave candidata que el diseñador escogió para identificar a cada entidad almacenada en la base de datos, solo puede existir una sola clave primaria para cada fila, esta clave no puede ser Null, que representa una regla de integridad que vemos en un momento.

**Clave foránea**: la clave foránea es un atributo o combinación de atributos que son clave primaria de otra tupla en otra relación o en la misma tabla de la entidad. Esta misma clave foránea puede formar parte de la clave primaria de la entidad.

---
En el caso de estar ante una relación de **one-to-many,** es decir, que una entidad **A** tenga múltiples **B**, se debe considerar que **B** tendrá en sus atributos la **FK** de **A**, y no al revés, suele ser útil recordar esto al diseñar la base de datos

---


#### Reglas de integridad del Modelo Relacional 

Son otras restricciones que se aplican a la base de datos, asegurando que esta no almacene valores inválidos, siendo estas tres reglas de integridad.

##### Regla de integridad de las entidades:
Se exige que toda clave primaria no asuma nunca un valor Null.

##### Regla de integridad referencial:
Se exige que para cada valor en una clave foránea, exista un valor en la clave primaria de la otra relación. No se exige, sin embargo, que las claves foráneas no sean Null, de hecho, pueden serlas y puede ser bastante practico permitirlo

##### Regla de negocio o comercial: 
Cada organización tiene sus propias reglas para los atributos de sus relaciones, por ello se permite definirlas en la base de datos. Estas reglas pueden ser no permitir que cierto atributo arbitrario sea Null, mayor o menor que cierto valor, entre otras posibilidades.
