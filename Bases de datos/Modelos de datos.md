En los ambientes de bases de datos, existen modelos de datos usados para almacenar los datos de una organización.
En si, un modelo de datos es una colección de herramientas conceptuales que brinda conceptos sobre esta, y define las reglas, relaciones y estructuras para el almacenamiento de datos, para luego manipularlos. Por lo tanto, las funciones de un modelo de datos son:

* Describir los datos
* relacionar los datos
* definir restricciones a los datos

### Uso de modelos de datos conceptuales de alto nivel

Podemos ver en el gráfico una descripción simplificada del proceso para diseñar una base de datos.

Lo importante es ver que, luego de la recopilación de requisitos y su análisis, el siguiente paso es crear un ***esquema conceptual***, que es una descripción concisa de los requisitos de los datos, tipos de entidades, relaciones, restricciones de ***una base de datos especifica*** (esto lo diferencia del propio modelo), esto se realiza haciendo uso de un ***modelo de datos conceptual de alto nivel***, nosotros mencionamos que los modelos de datos sirven para describir las estructuras, conceptos y reglas aplicadas a los datos, esto se realiza desde un ***nivel alto de abstracción***, no nos importan estructuras internas de implementacion.
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

Al presentar limitaciones tan grandes ambos modelos, resulto un alivio el modelo relacional de datos, que presenta un ***DBMS relacional (RDBMS)***, el cual esta desarrollado en el markdown de bases de datos relacionales

### Modelos NoSQL

los modelos ***NoSQL*** son otro tipo de bases de datos, también denominados ***Bases de datos No relacionales***, se implementan mediante un DBMS y presentan las siguientes características que las distinguen de los otros modelos:

* ***No necesitan un esquema de datos para operar***, es decir, los datos pueden almacenarse de diferentes formas, con diferentes estructuras, que no necesitan estar predefinidas en el sistema de bases de datos
* Son ***mas rápidos en el desarrollo***, al no necesitar pensar en un esquema de datos de antemano, el diseño, etc. esto acelera la codificación del sistema
* Se puede conseguir un ***mapeo mas directo*** entre los datos de la base de datos y los objetos/datos de un programa en diversos lenguajes de programación
* Al no tener un esquema predefinido, es ***altamente flexible***, permitiendo el poder adaptarse a datos con estructuras cambiantes
* Permiten manejar de forma optima grandes bancos de informacion (big data) y la ***escabilidad horizontal***, esto es, añadir mayor capacidad a la totalidad de la base de datos agregando otros servidores de bases de datos, repartiendo los datos entre cada una de las maquinas, también denominado fragmentacion. Esto ultimo es mas complejo en bases de datos SQL, debido a los propiedades ACID, y pudimos verlo en arquitecturas distribuidas que suelen involucrar escabilidad horizontal de datos, las bases de datos NoSQL no presentan tantas dificultades al no priorizar las ACID

![[Pasted image 20250418144952.png]]

* Las bases de datos NoSQL están ***pensadas para ser distribuidas***, trabajando en clusters, que como vimos da la posibilidad de una escabilidad mas rápida y sencilla de estos, por lo que permiten la replicacion y la distribución de datos en múltiples servidores de bases de datos
* Transacciones ***BASE*** en lugar de ACID, que viene de ***coherencia eventual flexible básicamente disponible***, básicamente disponible porque permite el acceso concurrente de la base de datos, flexible porque los datos tendrán estados transitorios, que cambiaran con el tiempo sin necesidad de un usuario, por ejemplo, al subir una publicación en una red social, el cambio no sera visible de forma inmediata para todos los usuarios, pero se actualizara en algún momento para todos. Coherencia eventual se refiere a que, cada nodo podrá tener valores diferentes en sus datos, pero cuando las actualizaciones simultaneas a estos registros finalicen, los nodos que contienen estos registros alcanzaran la coherencia.

El modelo NoSQL presenta diferentes tipos:

### Modelo NoSQL clave-valor

El modelo clave-valor es sencillo en cuanto a funcionalidad, cada elemento esta identificado por una llave única, permitiendo su recuperación rápida, es comparable a un Hash-Map.
Este modelo puede no ser adecuado en situaciones de acceso por valor (falta de indices), actualizaciones frecuentes de estos valores (se debe escribir el registro completo de nuevo) o si debemos almacenar relaciones entre los diferentes registros.

![[Pasted image 20250418160430.png]]

### Modelo NoSQL documental

Se almacenan los datos en un formato de documento, que pueden ser XML, JSON, BSON, etc.
no se requiere de una estructura definida para el documento, lo que brinda mas ***flexibilidad al momento de almacenar datos con nuevas estructuras.***

Este modelo puede presentar algunas inconsistencias, debido a que, al ser agregado, cada documento debe contener todos los valores asociados al objeto que se busca, y se pueden dar situaciones como la que se presenta a continuación, donde los datos de alumno están en el componente Alumnos, pero también en el de inscripciones, pudiendo presentar inconsistencia entre ambos si se modificara alguno de sus documentos

(notar que documentos es cada objeto y el archivo es el componente)

![[Pasted image 20250418165003.png]]

presenta una ventaja con respecto a los Key-Value, al poder recuperar documentos por valor, la posibilidad de crear indices para la búsqueda, y la posibilidad de modificar solo porciones de un documento

![[Pasted image 20250418160848.png]]

### Modelo NoSQL de grafos

El modelo de grafos almacena los datos que están relacionados entre si, bajo muchos niveles, se presentan entonces dos elementos: Los nodos, que representan entidades, y las relaciones o aristas, que representan las relaciones entre los nodos.

Los datos de los nodos se almacenan en un formato clave-valor, a su vez, las relaciones pueden tener datos en si mismos, por ejemplo, una relación alumno inscrito en comisión, podría existir el dato de fecha de inscripción

![[Pasted image 20250418161513.png]]
### Modelo NoSQL columnar

El modelo columnar en el contexto de bases de datos NoSQL se caracteriza por almacenar los datos organizados por columnas en lugar de por filas, lo que lo diferencia de los modelos relacionales tradicionales. Aunque su nombre puede recordar a las bases de datos relacionales, este es un enfoque más flexible y distribuido, que no sigue las propiedades ACID estrictamente.

Cada fila representa un registro de datos y está identificada por una clave única (key-value). Las columnas, agrupan los campos de los registros, aquí no todas las filas necesitan tener las mismas columnas, ademas, esta estructura permite agrega columnas a las tablas a medida que se vaya requiriendo, en lugar de crear una nueva tabla para una nueva estructura de datos

Se utilizan estructuras denominadas **supercolumnas**, que permiten agrupar múltiples columnas bajo una misma clave o nombre de columna, facilitando una forma de agregación dentro del propio modelo de datos (es decir, agrupar múltiples campos relacionados como una unidad).

Una de las ventajas clave del modelo columnar NoSQL es el rendimiento en operaciones de lectura sobre columnas específicas. Por ejemplo, si se quiere contar cuántos alumnos cursan la materia “GDA”, no es necesario recorrer todas las filas completas: basta con acceder a la columna (o supercolumna) correspondiente a esa materia y realizar el conteo directamente, ya que los datos pueden estar organizados para que esa operación sea eficiente, es decir, en una tabla, las columnas de alumno y materia están colocadas de modo que se asocia a cada alumno una correspondiente materia, lo que hace posible realizar el conteo desde la columna materia sin preocuparse si se esta asociando a un alumno, ya que esta asociación esta implícita en la estructura de la tabla. Este enfoque evita la necesidad de relaciones complejas entre tablas, ya que los datos relacionados suelen almacenarse juntos dentro de una misma fila o columna agrupada, optimizando así las consultas típicas del modelo.

![[Pasted image 20250418170206.png]]

### Modelos agregados y modelos dispersos

Los modelos que vimos, Clave-valor, orientados a documentos y orientados a columnas pertenecen al grupo denominado Modelos agregados, denominados así porque tratan al conjunto de datos como una unidad, denominado un ***agregado***, es decir, una vez que se obtiene el agregado, ***se tiene todo el conjunto de datos del objeto que buscamos***, en una base de datos relacional esto requeriría combinar datos de múltiples tablas para tener al objeto completo

Los modelos dispersos, como el modelo orientado a grafos, los datos están distribuidos en múltiples entidades separadas, que están relacionadas, no están agrupadas en una sola entidad por lo que la recuperación de datos requerirá acceder a múltiples entidades

### Resumen de diferencias principales entre NoSQL y Relacionales

![[Pasted image 20250418191838.png]]

### Clasificación de los DBMS 

nosotros podemos ver que, al existir múltiples modelos de datos, existirán múltiples DBMSs 
y estos podrán clasificarse según los siguientes criterios:

* Modelo de datos usado: RDBMS, OODBMS, NoSql, entre otros
* Numero de sitios: Centralizado o distribuido (homogéneo o heterogéneo)
* Costo de su uso
* Proposito: puede ser de uso general o especial para un objetivo especifico de la organizacion