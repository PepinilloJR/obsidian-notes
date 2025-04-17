Las bases de datos distribuidas surgen de la unión de dos tecnologías/conceptos:

* Bases de datos relacionales
* comunicación de datos y redes

para comprender estas, debemos introducirnos a nuevos conceptos adicionales que complementan los antes vistos en bases de datos relacionales:

## Conceptos de las BDD

las bases de datos distribuidas se basan en un **sistema de computación distribuido**, este consiste en un numero de ***elementos, sitios, nodos***, de procesamiento, interconectados mediante una red de computadoras, cooperando entre ellos para realizar tareas.

ahora bien, definimos a una base de datos distribuida (BDD, o DDB) como una colección de múltiples bases de datos interrelacionadas de forma lógica mediante una red de computadoras o servidores

la administración de esta base de datos se da mediante un sistema de administración de bases de datos distribuidas (DDBMS), el encargado de administrar la BDD, ejecutándose en cada uno de los nodos o ***sitios*** (un sistema de BD en si mismo con su hardware, software, usuarios y datos) de la red de computadoras, y se encarga de hacer **transparente*** al usuario esta distribución, es decir, el usuario vera todo como si fuera una única base de datos ejecutándose en su computadora, no necesita saber donde esta conectado, como se transfieren los datos, ni detalles del estilo.

**la transacción de los datos en una BDD puede ser local o global***.
no todos los cambios en una BDD debe requerir que se realice en cada uno de sus sitios, pueden haber transacciones locales que no necesiten sincronizarse con los demás

## Arquitectura "Nada compartido"

En contra parte al sistema distribuido, existe la arquitectura de sistema paralelo, donde un sistema muliprocesador maneja diferentes operaciones en paralelo y se comunica con los demás sin gastos derivados del intercambio por red, ya sea por memoria o por otro medio mucho mas rápido que la red

Un sistema de bases de datos construido bajo esta arquitectura se denomina sistemas de administración paralelos de bases de datos, en contra parte al DDBMS.

uno de estos es el denominado ***arquitectura "Nada compartido"***
cada procesador tiene su propia memoria y disco, y se comunican a través de un bus o switch de alta velocidad.

En el contexto de bases de datos, cada uno de estos nodos de procesamiento maneja su propia base de datos especializada, pudiendo haber coordinación entre estas o totalmente aisladas

en general es difícil mantener la consistencia de datos entre todas las bases de datos.

un detalle importante que lo diferencia de un sistema distribuido es que los nodos son homogéneos, mientas que en sistemas distribuidos pueden presentar mayor heterogeneidad 

![[Pasted image 20250413135758.png]]

Luego, un sistema de BDD puede presentarse en dos formas

### Arquitectura en red con una base de datos centralizada

El sistema centraliza la BDD en uno de los sitios, donde el resto de sitios actúan como clientes de procesamiento de datos a través de la red de comunicaciones.
es decir, aquí no se presenta la ventaja en disponibilidad, pero si se distribuye el procesamiento de las diferentes operaciones en la BDD.

### Arquitectura de BDD autentica 

En este modelo, el sistema de DDB presenta a cada sitio con su propia base de datos local, y colaboran entre si a través de una red de comunicaciones.
Este es el modelo que tomamos en consideración, y genera algunas cuestiones sobre como distribuir los datos entre todas las bases de datos de cada sitio. A esto se le llama bases de datos fragmentadas y/o replicadas

Esta arquitectura presenta ventajas así como problemas que trataremos mas adelante.

![[Pasted image 20250413162709.png]]

## Técnicas de diseño de BDD (autenticas)

### Fragmentación de datos

En una BBD, se debe decidir que porciones de la base de datos almacenar en cada sitio, es decir, como se distribuyen las relaciones de la base de datos en cada uno de los sitios.

como unidad lógica se usa las propias relaciones, es decir, se considera que cada relación completa se almacenara en un sitio particular, no subdividirla en distintos sitios, sin embargo puede realizarse lo contrario.

#### Fragmentación horizontal

La Fragmentación horizontal se realiza sobre una relación, como un subconjunto de tuplas de esta, las tuplas pertenecientes a cada fragmento se especifican mediante una condición sobre sus atributos
por ejemplo, podemos fragmentar una relación EMPLEADO según que departamento trabaje ( dp = 0, dp = 1, dp = 2, ...).
Luego, cada uno de estos fragmentos puede asignarse a diferentes sitios del sistema distribuido.

***La Fragmentación horizontal derivada*** se da con la Fragmentación de una relación en base a otra ya fragmentada, generalmente relacionada a esta mediante una FK, de este modo, los datos relacionados están localizados de forma cercana.

Por ejemplo, si empleado se fragmenta según el departamento, si la relación departamento ya esta fragmentada, los fragmentos de empleado se colocaran en el mismo sitio que los fragmentos de departamento correspondientes.

#### Fragmentación vertical

La Fragmentación vertical toma otra perspectiva, quizás cada sitio no necesita todos los atributos de una relación, si no un fragmento de ellos.
Podemos dividir una relación verticalmente por columnas, cada fragmento tendrá una porción de las columnas de una relación, es decir solo una porción de los atributos. 
En general, en cada fragmento se debe añadir la clave primaria de la relación, para poder asociar cada porción con el resto de sus atributos de ser necesario.

#### Fragmentación mixta (híbrida)

se combinan ambos tipos de Fragmentación, cada sitio puede contener una fracción de los atributos de una relación, donde ademas cada fragmento tendrá un conjunto de estos atributos en base a uno de sus valores.

### Replicación de datos

la replicación de datos se utiliza para mejorar la disponibilidad de los datos, en caso de perderse la disponibilidad de un sitio, es posible recuperar los mismos datos de otro sitio.

existen dos posibilidades para la replicación de datos, podemos tener una ***base de datos distribuida totalmente replicada***, donde cada sitio tiene replicado toda la base de datos distribuida, pudiendo el sistema funcionar incluso con un solo sitio funcionando, y ademas mejorando el rendimiento de la recuperación de datos, al estar localmente disponible en cada sitio la informacion

Sin embargo, una base de datos totalmente replicada implica que para actualizar una base de datos se requiere actualizarla en la totalidad de los sitios, llevando a un desmejoramiento del rendimiento bastante notable.

Como alternativa esta la ***base de datos distribuida parcialmente replicada***, algunos fragmentos de la base de datos se replican en algunos de los sitios, donde el numero de copias puede ir de 1 a el total de sitios disponibles.

el grado de replicación así como la elección de que sitios en ultima instancia depende de los objetivos de rendimiento del sistema que se esta desarrollando.

### Ventajas de las BDD

##### 1. Administración de datos distribuidos con distintos niveles de transparencia:

un DDBMS presenta una distribución transparente, en el sentido de que oculta detalles, similar al DBMS que vimos sobre el almacenamiento de datos, donde adicionalmente se presentan las siguientes transparencias:

* ***Transparencia de red***: la transparencia de red oculta los detalles de las operaciones de red al usuario, donde la ***transparencia de localización*** referencia a que una tarea llevada a cabo sera independiente de la ubicación física de los datos (sitio) y la ***transparencia de denominación*** implica que una vez especificado un nombre puede accederse a los datos sin mayores especificaciones sobre ubicación o de red

* ***Transparencia de replicación***: la transparencia de replicación permite al usuario no enterarse de la existencia de copias, para el los datos son una sola colección a la que acceder

* ***Transparencia de fragmentación***: la transparencia de fragmentación permite al usuario no enterarse sobre la existencia de fragmentación de los datos, esto se hace transformando una consulta global del usuario en varias consultas fragmentadas dirigidas a cada fragmento, cosa que el usuario no tiene porque enterarse.

* ***Transparencia de diseño y ejecución***: se evita mostrar al usuario el como esta diseñada la BDD y donde se ejecuta cada transacción.

##### 2. Se incrementa la fiabilidad de los datos y su disponibilidad:

Estas son las ventajas principales de una BDD, un sistema distribuido tendrá mayor fiabilidad al poder funcionar usando otro sitio si uno cae, pudiendo funcionar inclusive con solo uno funcionando, adicionalmente, un sistema distribuido tendrá mayor disponibilidad de datos por la misma razón que la fiabilidad, pudiendo obtener los datos incluso con alguno de los sitios caído.
##### 3. Mejor rendimiento:

Por lo general, las bases de datos serán mas pequeñas en cada sitio al distribuirse entre estos, por lo que el acceso a datos de esta es mas rápido, ademas, cada sitio tendrá que ejecutar un numero menor de transacciones al trabajar por separado, dividiendo las tareas que por lo contrario tendrían que ejecutarse en una base de datos centralizada, pudiendo estas tareas realizarse en paralelo 

##### 4. Expansión mas sencilla:

Se pueden expandir los sistemas de forma mas sencilla, agregando otros sitios, o expandiendo alguno de estos, adicionando mas espacio o poder de procesamiento con mas procesadores en estos.

### Funciones adicionales del DDBMS

El DDBMS presenta una mayor complejidad que el DBMS debido a la adición de nuevas funciones y objetivos.

* Seguimiento de los datos: el DDBMS debe tener la capacidad de controlar la distribución de los datos, la fragmentacion y la replicacion expandiendo el catalogo DDBMS (metadatos del DDBMS)
* Procesamiento de consultas distribuidas: el DDBMS debe tener la capacidad de acceder a sitios remotos y de transmitir consultas y datos mediante la red
* Administración de transacciones distribuidas: el DDBMS debe tener la capacidad de diseñar estrategias para la ejecución de consultas y transacciones que manejan datos que están en mas de una ubicación, y de sincronizar estos manteniendo la integridad entra todas las bases de datos.
* Administración de datos replicados: el DDBMS debe tener la capacidad de decidir que copia de los datos acceder y como mantener la consistencia entre todas las copias.
* Recuperación de una base de datos distribuida: el DDBMS debe tener la capacidad de recuperarse de la caída de un sitio o un fallo en las redes de comunicación
* Seguridad: el DDBMS debe poder ejecutar transacciones con administración de la seguridad de los datos involucrados, contando ademas con un sistema de permisos/privilegios para cada usuario, al funcionar todo en red, es mas vulnerable a intromisiones.
* Administración del catalogo/directorio distribuido: el catalogo contiene informacion en forma de metadatos, sobre los datos de la BDD, este puede ser global para toda la BDD o local para cada sitio. Donde se coloca este y su distribución esta mas relacionado con las políticas del negocio y el diseño

Adicionalmente, un punto que no tomamos en cuenta pero que es crucial en la complejidad del DDBMS, es el diseño de la red, su topologia y la tecnología empleada

El DDBMS ***implica un mayor uso de hardware adicional***, con la necesidad de múltiples equipos denominados sitios o nodos, conectados mediante algún tipo de red

La conexión puede realizarse a través de una ***red de área local*** o una ***red de área expandida***la primera mediante cableado mientras que la ultima puede utilizar satélites o lineas telefónicas u otras tecnologías para la transmisión de datos a largas distancias, para estas se pueden implementar diferentes topologías que definan las rutas usadas para la comunicación, con un alto impacto en el rendimiento.

### Tipos de sistemas de BDD (DDBMS)

se toman diferentes factores para categorizar los DDBMS, uno de estos es el ***grado de homogeneidad del software del DDBMS***, si todos los servidores y usuarios usan el mismo software, el DDBMS es homogéneo, por lo contrario si utilizan diferentes software para cada sitio, es heterogéneo.

Luego esta el ***grado de autonomía local***, si el sitio local no es capaz de funcionar por su cuenta como un DBMS aislado, se dice que no tiene autonomía local, por el contrario, mientras menos dependa un sitio de otros para funcionar, tendrá un mayor grado de autonomía local, idealmente cada nodo debería tener un alto grado de autonomía local

con el grado de autonomía local, se puede distinguir un modelo denominado ***sistema de bases de datos federado (FDBS)*** donde cada servidor o sitio actúa como un DBMS centralizado independiente y autónomo con su propios usuarios, transacciones, y adicionalmente es ***heterogéneo*** si también utiliza su propio software, presentando distintos DBMS, por lo que presenta un alto grado de autonomía local.

la implementacion de un FDBS presenta las siguientes complicaciones:

1. Resolver las diferencias en los modelos de los datos presentes en cada sitio (relacional, objetos, ficheros)
2. Resolver las diferencias en las restricciones implementadas o como son implementadas para cada sitio (restricciones de integridad, triggers, etc)
3. Resolver diferencias en los lenguajes de consulta utilizados

Otro problema que se presenta en el FDBS es la ***heterogeneidad semántica***
y ocurre cuando existen diferencias en el significado y uso de un dato entre los diferentes DBMS presentes en cada sitio. Esto puede ser por ejemplo, que dos bases de datos una en USA y otra en Japón, tengan distintos atributos acerca de las cuentas de los clientes, o las fluctuaciones en la cotización de las diferentes monedas podrían causar problemas, podrían si no tener diferentes nombres para los mismos elementos de datos y estructuras, entre otras problemáticas que componen a la heterogeneidad semántica

finalmente presenta 2 diferentes posibles autonomías

1. de ejecución: capacidad del DBS de ejecutar operaciones locales sin interferencia de otros DBSs
2. de asociación: potestad de decidir si quiere compartir su funcionalidad y recursos con otros DBSs
### Costes de transferencia de datos

una problemática adicional que trae la DDBMS es el costo de red que implican las trasferencias, por lo que debe realizarse las operaciones con el mínimo posible de estas
### Control de concurrencia y consistencia de datos

En un DDBMS, se pueden dar múltiples problemas con respecto a la concurrencia y consistencia entre las copias de los datos.

Se debe encargar de que todas las copias sean consistentes, para garantizar la recuperación ante fallos de uno de los sitios, para efectuar restauraciones.

El DDBMS debe saber tratar con confirmaciones distribuidas e interbloqueos

La confirmación distribuida se da a la hora de intentar confirmar una transacción que accede a múltiples sitios de bases de datos, si alguno de estos falla, se debe afrontar esta situación para evitar valores no deseados en los datos.

Los interbloqueos se producen cuando uno o mas DDBMSs compiten por recursos de un mismo sitio, debe tener técnicas para gestionarlos

##### Recuperación de fallas

fallos individuales: el DDBMS debe ser capaz de seguir operando cuando uno o mas sitios falla, y cuando se lleva a cabo la restauración de estos, actualizar la copia de la base de datos de esta antes de entrar al sistema de nuevo

fallos de enlaces: el DDBMS debe ser capaz de tratar con fallos que se producen en los enlaces que conectan a los sitios.


