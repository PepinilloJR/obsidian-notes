En los ambientes de bases de datos, existen modelos de datos usados para almacenar los datos de una organización.
En si, un modelo de datos es una colección de herramientas conceptuales que brinda conceptos sobre esta, y define las reglas, relaciones y estructuras para el almacenamiento de datos, para luego manipularlos. Por lo tanto, las funciones de un modelo de datos son:

* Describir los datos
* relacionar los datos
* definir restricciones a los datos

### Uso de modelos de datos conceptuales de alto nivel

Podemos ver en el gráfico una descripción simplificada del proceso para diseñar una base de datos.

Lo importante es ver que, luego de la recopilación de requisitos y su análisis, el siguiente paso es crear un ***esquema conceptual***, que es una descripción concisa de los requisitos de los datos, tipos de entidades, relaciones, restricciones, esto se realiza haciendo uso de un ***modelo de datos conceptual de alto nivel***, nosotros mencionamos que los modelos de datos sirven para describir las estructuras, conceptos y reglas aplicadas a los datos, esto se realiza desde un ***nivel alto de abstracción***, no nos importan estructuras internas de implementacion.
todo este proceso se denomina ***diseño conceptual***, que haciendo uso de un modelo de datos conceptual, obtiene un **esquema conceptual** de la base de datos que necesita el cliente

el siguiente paso es realizar la implementacion real, mediante un DBMS, el DBMS creara entonces un ***esquema lógico*** mediante lo que se denomina un ***modelo de datos de implementacion***, en el proceso de ***diseño lógico***, luego este esquema lógico es la base para finalmente le diseño físico de la base de datos.

![[Pasted image 20250417131654.png]]
### Tipos de modelos de datos

Existen múltiples modelos de datos, clasificados según en que estructuras se basen:
Nosotros podemos clasificar los modelos de datos según estén
***basados en objetos, basados en registros***, o en ***bases de datos no relacionales***.

![[Pasted image 20250417122756.png]]

### Modelos basados en objetos

los modelos basados en objetos se pueden dividir en dos, el modelo Entidad-Relación (ER) y el Modelo orientado a objetos.

##### Modelo Entidad-Relación

A diferencia de los otros modelos que veremos, el modelo entidad-relación es un modelo puramente conceptual de alto nivel.

El modelo ER describe los datos como ***entidades, relaciones y atributos***. 

Una ***entidad*** es un objeto básico representado por el modelo ER, una cosa del mundo real con existencia independiente de otras entidades, generalmente perteneciente a una colección de entidades de mismos atributos, denominado ***tipo de entidad***, como dijimos esta entidad tendrá ***atributos***, que son propiedades que los describen (edad, nombre, dirección, etc), este puede ser ***compuesto*** (hecho de otros atributos) o ***atómico***.
A diferencia que lo que vimos en el relacional, cosa de mas adelante, los atributos pueden ser ***multivalor***, es decir, presentarse como un conjunto de valores (Colores, Licenciaturas, Medidas, Sueldos). Luego, se presenta la posibilidad de derivar atributos de otros atributos almacenados, por ejemplo la Edad (***atributo derivado***) puede obtenerse como derivado del atributo FechaNacimiento (***atributo almacenado***). Un ***atributo complejo***, es aquel que puede ser multivalor y compuesto. Un ***atributo clave*** es un atributo que se restringe a ser único por cada entidad, y que identifica inequívocamente a esta

Finalmente, las entidades estarán relacionadas, esto es, una asociación entre dos entidades, cada entidad se denomina participante, la forma que se asocian puede ser descrita, por ejemplo, trabaja_para, controla, usa, etc.
una relación tendrá grado, que es el numero de participantes.

Existen, considerando el concepto de relación, entidades débiles y fuertes
las débiles no tienen atributo clave propio y se identifican como relacionadas a una entidad fuerte, denominada relación de identificación, es decir que se identifica a una entidad A porque esta relacionada con una entidad B.
Una entidad débil presenta lo que se llama dependencia de existencia, que implica que la existencia de una entidad dependerá de si esta relacionada con otra, no sucede a la viceversa es decir, no toda dependencia de existencia implica entidades débiles.

estos concepto son mas amplios y hay mucho mas, y puede averiguarse mas en el libro, sin embargo con entender los conceptos clave es mas que suficiente.

![[Pasted image 20250417142618.png]]

##### Modelo orientado a objetos

El modelo orientado a objetos es un modelo de datos de implementacion, que se hizo necesario con la aparición de estructuras mas complejas que representar en una aplicación, la necesidad de tipos de datos nuevos mas complejos, la necesidad de definir operaciones especificas no usadas en bases de datos relacionales y transacciones que requieren de mayor tiempo.

Otra razón para su uso, es con la aparición de muchos lenguajes POO y con la necesidad de hacer que los objetos de estos programas sean persistentes en el tiempo, integrándose directamente con este tipo de bases de datos.

Estos no son muy populares y solo veremos una visión panorámica de los conceptos 

##### Conceptos de la base de datos orientada a objetos (OODB)

Las bases de datos orientadas a objetos (OODB) son administradas por un DBMS de objetos (ODBMS).

el ODBMS generara un OID único e inmutable para cada objeto de la base de datos, la estructura de los objetos es de complejidad arbitraria según la informacion que requiera contener, donde el valor actual (estado) del objeto se construye a partir de otros objetos o valores, utilizando constructores de tipo, estos constructores y tipos se definen utilizando un ***lenguaje de definición de objetos (ODL)***

Esto ultimo se representa con la terna (i, c, v), donde ***i*** es el OID, ***c*** el constructor, y ***v*** el estado del objeto.

el concepto esencial de los sistemas OO es el concepto de ***encapsulamiento***, los objetos a diferencia de las tablas relacionales, requieren de definir las operaciones aplicables a estos, y sobre que componentes de este son accesibles y cuales no, mediante estos mismos métodos (operaciones), toda su estructura interna esta oculta al usuario y es accesible solo a través de los métodos que definamos en estos, a modo de ***interfaz***.

Esto sin embargo puede resultar demasiado exigente, por lo que algunos implementan el sistema de ***visible*** y ***hidden***, es decir, es posible acceder directamente a los atributos si son visible, mediante un lenguaje de consulta de alto nivel o lenguaje de consultas de objetos (***OQL***) 

Las OODB son mucho mas amplias, pero las principales diferencias ya se discutieron, por lo que es ideal pasar a los ***modelos de datos basados en registros***

### Modelos basados en registros

Los modelos de datos basados en registros, los datos se estructuran en un conjunto de campos que conforman un registro, esto es con lo que estuvimos trabajando hasta ahora.
existen 3 principales modelos basados en registros, los primeros 2, ***modelo de datos en red*** y ***modelos de datos jerárquicos***, se consideran obsoletos, el ultimo, el modelo relacional, es con el que trabajamos en la materia, y es la que mas desarrollo tendrá.

#### Modelo de datos en red

Los datos se presentan como una colección de registros, y las relaciones entre ellos mediante enlaces, estos enlaces son campos con punteros que apuntan al registro relacionado.

Este modelo es bastante flexible, donde los campos que almacenan punteros son de longitud variable, debido a la necesidad de mas de un puntero (relaciones uno a muchos, muchos a muchos), pero esto implica una complejidad muy amplia lo que a la larga puede crear una base de datos tremendamente compleja de mantener 

![[Pasted image 20250417175544.png]]

#### Modelo de datos jerárquico

La presentación de los datos es la misma, de la misma forma que las relaciones se presentan con campos que contienen punteros, el detalle particular es que se presenta toda la base de datos como un árbol invertido, presentando restricciones en las relaciones.

Si bien el modelo es capaz de crear relaciones menos complejas y presenta una estructura mas entendible, presentando ciertas inflexibilidades como que si se elimina un registro padre los hijos son eliminados también.

pero el principal problema es a la hora de crear relaciones unos a muchos y muchos a muchos. Específicamente, para que un hijo tenga mas de un registro padre, deben duplicarse los datos para poder representar esa relación, por lo que es un sistema con ***redundancia alta*** para una situación que es de lo mas común a la hora de implementar una base de datos

![[Pasted image 20250417180737.png]]

Al presentar limitaciones tan grandes ambos modelos, resulto un alivio el modelo relacional de datos, el cual esta desarrollado en el markdown de bases de datos relacionales

### Modelo NoSQL