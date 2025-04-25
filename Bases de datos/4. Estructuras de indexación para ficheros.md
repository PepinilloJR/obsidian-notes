**La estructura usada en los ficheros de las bases de datos, es la de indexacion**, que es una forma de acceder a los registros de forma alternativa, sin afectar la ubicación física de los registros (a los que obviamente tenemos que seguir accediendo, esto es una capa por encima).

El fichero ya presenta una organización, sea desordenado,ordenado o disperso. ->  Los tipos de índices predominantes están basados en los ficheros ordenados (índices de un nivel), porque asumen que los ficheros están ordenados por el mismo campo del indice, aun así, los índices también pueden construirse basándose en la dispersión o en otras estructuras de datos de búsqueda

Específicamente, la indexacion se crea usando un campo cualquiera de los registros del fichero, denominado clave de búsqueda o campo de indexación, que usamos para encontrar el bloque que contiene al registro, y es un tipo de estructuración para el acceso a datos, que usamos en bases de datos relacionales.

## Índice principal.

El indice principal es un fichero ordenado cuyos registros son de longitud fija de dos campos, el primero indica el tipo de dato del campo clave de ordenación (clave principal) del fichero, luego el segundo campo es el puntero hacia un bloque de disco, que contiene una serie de registros, un bloque puede contener múltiples registros.

El campo de búsqueda tiene como valor el valor de la clave principal del ***primer registro del bloque***:

![[Pasted image 20250405143820.png]]

Un ***indice principal es escaso*** (no denso), es decir, solo tiene entradas para algunos de los valores de búsqueda que podamos insertarle, ya que este incluirá una entrada por cada bloque de disco del fichero de datos, pero no una entrada para cada registro como tal, si no ***una entrada por cada primer registro de cada bloque.*** denominado ***registro ancla***.

Esto presenta 2 ventajas a simplemente usar los registros del fichero
1. El fichero de indice principal necesita menos bloques que el fichero de datos -> menos entradas de indice que registros en el fichero de datos, y cada entrada del indice tiene un tamaño generalmente mas pequeño que los registros
2. Una búsqueda binaria en el fichero indice es mas rápida que realizar la búsqueda binaria directa en el fichero ordenado

### Problemas en la modificación del archivo del indice principal:

Como con todos los ficheros ordenados, la inserción y la eliminación de un registro, requiere el reordenamiento del resto de registros en los bloques, pero ademas, requerirá que cambiemos algunas entradas del indice, debido a que ***podría cambiar el registro ancla***

Una solución es aplicar un ***archivo de desbordamiento***:
las inserciones nuevas se guardan en un archivo de desbordamiento, luego, cada bloque tendrá un puntero al archivo de desbordamiento (u otro modo para acceder al archivo de desbordamiento), para realizar búsquedas adicionales en este de no encontrarse un registro recientemente agregado.
-> Es esencial crear una ***reorganización cada cierto tiempo*** para ahorrar espacio
-> Para los borrados, aplicamos ***borrado lógico*** mediante marcadores de borrado

### Índices agrupados.

Si los registros del fichero están ordenados físicamente por un campo no clave (es decir, un valor que puede repetirse para mas de un registro), se lo denomina campo agrupado.

El indice agrupado es entonces un fichero ordenado de registros de dos campos, el primero es del mismo tipo que el campo agrupado del fichero de datos, el segundo sera un puntero al primer bloque del fichero que tiene un registro con ese valor para su campo agrupado.

![[Pasted image 20250405155014.png]]

Aquí, el indice agrupado tendrá una entrada por cada valor distinto del campo agrupado, por lo tanto también es escaso (no denso).

El indice agrupado presenta los mismos problemas a la hora de inserción y eliminación de datos, aunque presenta una solución adicional.
Esto es reservar un bloque o bloques contiguos por cada valor distinto del campo de indexacion, por lo tanto, cada puntero sera a un bloque con todos registros con los mismos valores del campo de agrupamiento, donde este bloque tendrá a su vez un puntero a los demás bloques reservados para ese campo, pudiendo lograr entonces una inserción directa, y si se acaba el espacio, se encadena un bloque mas.

![[Pasted image 20250416225046.png]]

### Índices secundarios.

Un indice secundario proporciona un medio secundario de acceso a un fichero, para el que ya existe algún acceso principal (indice principal preexistente).

El campo puede ser una clave candidata, de valor único en cada registro, o puede ser no clave, similar al campo agrupado, esto significa que estamos hablando de un ***campo no ordenado***.

El indice presentara de nuevo, registros ordenados de dos campos, el primero del mismo tipo que el campo **no ordenado** del fichero (campo de indexacion) y el segundo un puntero a un bloque, o aquí lo interesante, un puntero a un registro

Para el caso de que estemos hablando de un campo clave, recibe el nombre de ***clave secundaria***, donde habrá entonces una entrada por cada registro del fichero (***indice denso***), y contendrá el valor de la ***clave secundaria del registro***, y un puntero al bloque que tiene almacenado el registro, o al ***propio registro.***

 Esto ultimo se da ya que, ***no podemos utilizar un registro ancla***, debido a que los registros no estarán ordenados por el campo clave secundario en los bloques.
 Una vez que se obtiene el bloque contenedor del registro, se busca en este hasta encontrarlo de forma lineal.

![[Pasted image 20250405161834.png]]

El indice secundario presenta algunas desventajas, ya que ***ocupa mayor espacio de almacenamiento al ser denso***, y el ***tiempo de búsqueda se hace mayor*** al tener mas entradas.

El indice secundario proporciona una ordenación lógica de los registros, (es posible realizar búsqueda binaria en el indice secundario), y no física como el principal, esto significa que podríamos acceder a los registros por el orden de entradas del indice secundario, aunque el orden físico como tal no lo sea, de este modo tendremos a los registros ordenados por el campo de indexacion, aunque físicamente no lo estén.

**Índice secundarios para campos no clave:**

En el caso de un indice secundario que utiliza como campo indexado un valor que se repite en mas de un registro, la opción mas usada es crear la siguiente estructura.

Se mantiene en el indice secundario una entrada por cada valor del campo del indice ***(no denso)***, luego, esta entrada apuntara a un bloque de punteros de registro, que no es aun el bloque del fichero de datos.

Este bloque de punteros de registro, tendrá como entradas punteros que apuntan a un registro del fichero de datos con el valor del campo de indexacion que buscamos. 
adicionalmente, podemos no usar bloques de disco para contener estos punteros, si no que realizar una lista enlazada de estos, ya que podrían haber muchos elementos con ese mismo campo, superando el tamaño del bloque

esto presenta desventajas y ventajas:

1. la recuperación a través de un indice requiero uno o mas accesos a bloques debido a ese nivel extra de direccionamiento que agregamos
2. los algoritmos de búsqueda en el indice y de inserción, son bastante directos, ya que no estamos contemplando un orden

![[Pasted image 20250405162720.png]]


### Índice multinivel.

Hasta ahora, recapitulando, entendemos que tenemos un indice ordenado, al que se le aplica búsqueda binaria para localizar los punteros a un bloque de registros. 
Nosotros podemos optimizar esta búsqueda, consiguiendo búsquedas que mejoran incluso la velocidad de la binaria.

El concepto clave es el siguiente, podemos tomar nuestro indice, de cualquier tipo, y realizar un indice de este indice, para optimizar la búsqueda binaria que hubiera requerido anteriormente.

Ahora, desde un marco de definición, tomemos lo siguiente:

El indice multinivel considera el fichero de indice denominado ***primer nivel*** o ***base***, como un fichero ordenado con un valor distinto por cada campo clave principal, es decir, ***denso***, por lo tanto, podemos para el primer nivel, implementar un indice principal, que señale a los bloques que contienen los registros del primer nivel, este indice lo denominamos ***segundo nivel***.

Como el segundo nivel es un indice principal, se pueden utilizar anclas de bloque, es decir, de modo que ***el segundo nivel tenga una entrada por cada bloque del primer nivel, y no por cada registro o valor de campo clave distinto.***

***Esto ya nos provee una búsqueda binaria***, Pero implementamos un ***tercer nivel***, que es un indice principal para el segundo nivel, teniendo ***una entrada por cada bloque del segundo nivel.***

> debe notarse que si el segundo indice ***no ocupa mas de un bloque***, el tercer nivel terminaría siendo ***un paso extra contraproducente***, ya que se haría acceso extra a un bloque que siempre seria el mismo, para acceder a otro bloque que también siempre sera el mismo.

Este proceso puede repetirse, hasta llegar al denominado nivel de ***indice superior***, aquí, las ***entradas de este nivel entran todas en un solo bloque***, donde ***ya no es necesario añadir otro indice con punteros a sus bloques***.

Aquí la búsqueda binaria sera optima, haciendo la búsqueda enormemente rápida incluso para archivos de datos con millones y millones de registros.
## Algunos detalles:

Puede implementarse un indice principal multinivel no denso, es decir, el nivel base tiene punteros a bloques de registros, por lo tanto no es denso.
Puede verse esto en este indice principal multinivel por el campo clave de ordenación del archivo.

![[Pasted image 20250405175249.png]]

La ***inserción*** se realiza usando algún tipo de archivo de desbordamiento que se combina periódicamente con el multinivel.