Las bases de datos requieren un medio físico de almacenamiento para que el DBMS pueda recuperar, actualizar y procesar esta base de datos desde el.

recordemos algunos conceptos de almacenamiento

### Jerarquías de almacenamiento

**Almacenamiento primario o principal:**
Medios de almacenamiento rápidos y de baja capacidad, memoria principal, memoria cache, son algunos ejemplos.
Al almacenamiento primario es usado por el CPU para operar y ejecutar instrucciones.

**Almacenamiento secundario:**
Medios de almacenamiento lentos y de alta capacidad, tienen un carácter no volátil, y no puede ser usado directamente por el procesador, por lo que la información del secundario debe subirse a la principal antes de usarse

**Almacenamiento terciario:**
Medios de almacenamiento lentos y de menor capacidad que el secundario, cumplen las mismas propiedades que los secundarios, pero adicionalmente suelen ser extraibles

### Discos magnéticos

Los discos magnéticos tienden a ser el medio almacenamiento mas común para las bases de datos, ya que proveen una velocidad aceptable a la vez que capacidades de almacenamiento muy grandes, y una vida útil bastante larga.

Es útil entonces para el DBA conocer sobre este a un nivel básico:

![[Pasted image 20250402115734.png]]

Los discos magnéticos, se colocan en paquetes de discos, denominados cilindros, cada uno tiene una cara o doble cara según el diseño, donde en cada cara se guarda información:

la cara esta subdividida en pistas, donde se contiene información, estas pistas a su vez se subdividen en sectores, que están formados a su vez en bloques

estas subdivisiones son esenciales para organizar y poder localizar la información, por lo general, del modo **Nro.cilindro + Nro.pista + Nro.bloque.**

los **bloques o paginas** se establecen en el SO en su formateo, y se separan entre los denominados huecos entre bloques, que contienen información de control sobre los bloques adyacentes.

como sabemos, el disco es direccionable, usando un cabezal que lee y escribe movido por un brazo, por lo que accede a la información con acceso aleatorio, haciendo uso de la información anterior, por lo general, el controlador del disco, mapea un numero denominado dirección de bloque lógica (LBA) para llegar al bloque de informacion requerido, a su vez, con la dirección de un buffer que se encargara de almacenar la información recuperada en caso de lectura o de contener información a escribir en una operación de escritura

a veces la operación requiere leer un conjunto contiguo de bloques, denominado clúster.

### Registros y ficheros

el sistema operativo, organiza todos los datos contenidos en los bloques de un disco, en lo que denominamos ficheros de registros.

> Un registro es una colección de valores de datos que pueden interpretarse como hechos relacionados con las entidades, sus atributos y sus relaciones. 
> una colección de nombres de campos y tipos de datos constituyen un tipo de registro, o mas bien, la definición de un formato de registro.

Para cubrir otras necesidades, como el almacenamiento de elementos de datos grandes y desestructurados, como lo pueden ser imágenes, vídeos, audios, etc.
se crean los denominados BLOBS (objetos binarios grandes) que son almacenados de forma independiente, y su registro incluye un puntero a este.

Entonces, un fichero sera un conjunto de registros, los registros pueden ser de tamaño fijo o variable.

### Organización extendida de registros

los registros de un fichero se deben asignar a bloques del disco, el problema es que los bloques son fijos, por lo tanto, pueden darse dos situaciones

1. El bloque supera el tamaño del fichero, desperdiciando espacio.

2. El fichero no cabe en un bloque, teniendo que dividir el fichero en múltiples bloques, pudiendo generar la primera situación con una fracción del fichero.

Puede aprovecharse este espacio almacenando parte de un fichero en uno de estos espacios, y la otra parte en otro, usando punteros al final del bloque para poder indicar el resto del registro, esto se denomina organización extendida de ficheros o registros.

si fijáramos que un registro no pudiera superar el tamaño de un bloque, tenemos una organización no extendida, puede servir si los ficheros fueran de registros fijos, ya que no se daría ninguna de las dos situaciones de perdida de espacio.

![[Pasted image 20250402125437.png]]
### Asignación de bloques a ficheros

los bloques puede asignarse de forma continua, enlazada o indexada

la continua consiste en asignar a los ficheros una serie de bloques contiguos, esto permite una lectura rápida pero genera con el tiempo mucha fragmentación

la enlazada consiste en colocar un puntero en cada bloque asignado que señale al siguiente bloque contenedor del fichero, aprovechando mas el espacio del disco al precio de una lectura mas lenta.

la indexada consiste en tener uno o mas bloques de indices, que contienen punteros a todos los bloques del fichero, entonces no se requiere la lectura del puntero al final del bloque, si no que puede trasladarse directamente ya conociendo la ubicación de cada bloque habiendo leído los bloques de indices, ahorrándonos fragmentación y es mas rápido que la enlazada.
### Cabeceras de ficheros

Una cabecera de fichero contiene la información sobre un fichero que los programas del sistema necesitan para acceder a sus registros. 
La cabecera incluye entonces información para determinar las direcciones de disco de los bloques que contienen al fichero y las descripciones de formato del registro (que pueden incluir longitudes de campo y orden de los campos dentro del registro en el caso de los registros no extendidos de longitud fija, y códigos de tipo, caracteres separadores y códigos de tipo de registro para los registros de longitud variable)
De no tenerse esta informacion, se tendría que realizar una búsqueda lineal por los datos a través de todos los bloques del fichero, o del registro si por otro lado no se tuviera informacion especifica del registro pero si del fichero. Por lo tanto es esencial que se tenga cabeceras en los ficheros con toda la informacion.

> El objetivo de una buena organización de ficheros, es localizar el bloque que contiene el registro deseado con una mínima cantidad de transferencias de bloques a memoria.

las operaciones una vez ubicados los bloques pueden ser la recuperación o la actualización

### Organización de ficheros

la organización de los ficheros hace referencia al modo que se colocan e interconectan registros y bloques al crear registros nuevos o eliminarlos, dentro de los ficheros.

### Ficheros de registros desordenados (Heap-Pila-Secuencial)

Es el tipo de organización más sencillo y básico, en el cual los registros se guardan en el fichero en el mismo orden en que se insertan, es decir, los registros se insertan al final del fichero. Esta organización se conoce como fichero heap o pila.

se caracteriza por lo siguiente:

•Más sencillo: orden de entrada
•Inserción de registros es muy eficaz: lee último bloque del fichero, agrega o
reescribe bloque.
•Búsqueda puede resultar lenta: lineal, bloque a bloque, no hay acceso aleatorio
•Eliminación produce pérdida de espacio o se hace marca en un byte
•requiere de una reorganización periódica si se quiere mantener.


### Ficheros de registros ordenados

los FICHEROS se ordenan físicamente en el disco en función de los valores de unos de sus campos, denominado campo de ordenación. 

![[Pasted image 20250402134637.png]]

esto presenta varias ventajas y desventajas con respecto a los ficheros desordenados:

•Inserción: costosa ya que se debe hacer espacio entre registros para mantener el orden
•Eliminación: costosa por la misma razón de la inserción
•Búsqueda por el campo de ordenación es eficaz, ya que puede implementarse una búsqueda binaria

### Organización de ficheros usando técnicas de dispersión –hash

se proporciona una función **h** denominada de dispersión, que se aplica a un campo de dispersión especializado del registro, y que produce la dirección del bloque de disco en el que se almacena el registro, permitiendo así, conociendo el campo de dispersión, acceder de forma aleatoria al bloque inicial del registro.

Debemos familiarizarnos con algunos conceptos:

Dentro del fichero, se implementa una tabla de dispersión, usando un array que contiene a los registros (dispersión interna) o punteros a los bloques de los registros (dispersión externa), de indices 0 - M-1, cada registro corresponde entonces a un indice de la tabla, para asignar un registro a uno de los indices, se usa una función hash h que transforma el valor del campo hash en un indice de la tabla. Una común es h(K) = K mod M que saca el resto del campo K de dividirlo por M, dando un valor entero que pertenece a la tabla como indice.

![[Pasted image 20250402141824.png]]

Debido a que el tamaño del campo hash suele ser mas grande que el espacio de direcciones disponibles para cada registro en la tabla, se dan fenómenos como las colisiones.

la colisión se produce cuando el valor del campo hash ya contiene un registro diferente al que se le intenta asignar, esto debe detectarse y cambiar el indice para colocarlo en otra posición, esto se denomina resolución de colisiones y existen múltiples métodos para implementarla:

**Direccionamiento abierto o sondeo lineal:** a partir de la posición que hizo colisión, se busca secuencialmente en las posiciones subsiguientes hasta encontrar una posición sin usar.

**Encadenamiento**: en este método, se conservan en una posición, un puntero a una lista enlazada de direcciones, de este modo, si existe una colisión, se encadena la dirección del registro a la lista enlazada, adicionalmente, se añade un campo puntero con la ubicación del siguiente elemento de la lista, luego, para acceder a esta dirección, se tendrá que recorrer la lista enlazada hasta encontrarlo, podría ser que cada elemento enlazado contenga el campo clave y la dirección para su búsqueda.

![[Pasted image 20250402144639.png]]

**Dispersión múltiple**: el programa aplica una segunda función de dispersión si la primera dio colisión, luego, si esto genera otra colisión, se utiliza el direccionamiento abierto.
