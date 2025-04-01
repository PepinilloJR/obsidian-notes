Una base de datos es una colección de datos (hechos grabados de significado implícito) relacionados, ejemplos son números de teléfono, direcciones de personas, entre otros. 

Una base de datos, para considerarse una, tendrá que tener las siguientes propiedades:

* representa aspectos del mundo real, considerando un universo de discurso, cambios en este reflejan cambios en la base de datos.
* los datos están coleccionados de forma coherente con algún tipo de significado, no son datos aleatorios surtidos sin razón.
* la base de datos tiene un propósito especifico y esta dada para un grupo pretendido de usuarios.

una base de datos puede tener cualquier complejidad y tamaño limitados solamente por la tecnología en la que esta sostenida.

si estamos hablando de una base de datos computarizada, tenemos que hablar de una **DBMS**. 

**Un sistema de administración de datos** (**DBMS**, database management system) es una colección de programas que permite a los usuarios crear y mantener una base de datos, es decir, **un sistema de software que permite la ***definición***, ***construcción***, ***manipulación*** y ***compartición*** de bases de datos entre usuarios y aplicaciones, y de proveer **seguridad, integridad de sus datos, respaldo y recuperación de datos**, y de manejar correctamente las **transacciones**, también se le conoce como **Motor de Base de Datos**

* Definir: especificar los tipos de datos, estructuras y restricciones de los datos, para ello se usa un **Lenguaje de definición de datos (DDL)**, esta información que describe a la base de datos se conoce como **metadatos** y se almacena como un catalogo para la base de datos, este catalogo debe consultarse para conocer la estructura de los archivos de una base de datos específica, como el tipo y el formato de los datos a los que accederá. 

* Construcción: es el proceso de almacenar los datos de la base de datos en algún medio de almacenamiento controlado por el DBMS, esto es creando las tablas, índices y relaciones

* Manipulación: consiste en realizar acciones sobre la base de datos como consultar, recuperar, actualizar y generar informes, para la manipulación se utiliza un **lenguaje de manipulación de datos (DML)**.

* Compartir: permitir a varios usuarios y programas acceder a la base de datos de forma simultanea

* Seguridad: se provee de protección contra el acceso no autorizado o malintencionado a la base de datos, usando un **lenguaje de control de datos (DCL)**.

* Integridad de los datos: permite definir reglas que definan cuando los datos son íntegros en cada transacción, por ejemplo, que cierto valor no pueda ser NULL

* respaldo y recuperación de datos: otorga herramientas para recuperar versiones de la base de datos.

* transacciones: una acción (transacción) sobre una tabla representa muchas veces una serie de acciones sobre otras tablas de valores, en general la transacción se diferencia de la consulta en que la primera incluya la escritura y lectura, la ultima solo la recuperación de información, esto mediante una aplicación de consultas/peticiones.
  podemos definir entonces a una transacción como un programa en ejecución o proceso que incluye uno o más accesos a la base de datos, como la lectura o la actualización de los registros de la misma, esta debe ser ***atómica***, es decir, se deben ejecutar todas las operaciones de esta o ninguna, y debe ser **aislada***, es decir que parezca que se ejecute de forma totalmente separada de otras transacciones.

cuando decimos que permite, decimos que el DBMS provee los lenguajes y herramientas para que podamos implementar estas propiedades.

Llamaremos a **Sistema de bases de datos** a la combinación de una DBMS y una base de datos, y aplicaciones que interactúan con ellas.

Podemos visualizarlo de la siguiente forma:

![[Pasted image 20250319220944.png]]

## Características y diferencias con el sistema tradicional 

podemos ver que el sistema de bases de datos se diferencia mucho del **modelo tradicional de utilizar ficheros** en el sistema para guardar la información requerida, presentando las siguientes diferencias si se tratase de implementar el sistema tradicional de ficheros:

**Sistemas tradicionales de ficheros:**

* Orientado a procesos
* Se utilizan archivos específicos para cada aplicación
* Se genera redundancia de datos debido a la necesidad de que varios usuarios accedan a los mismos datos.
* Los datos son inconsistentes debido a que múltiples usuarios usan archivos diferentes.
* Perdida o derroche de espacio físico debido a los puntos anteriores
* Se depende físicamente de los datos (dependencia física de datos), es decir, los datos están estrechamente ligados a como se almacenan en disco (formato, ubicación, etc.), y lógicamente (dependencia lógica de los datos), es decir, de la lógica interna de los archivos, donde si cambia alguna de estas dependencias, habrá que cambiar los programas que los acceden. 

**Sistemas de bases de datos:**

* Orientado a los datos
* No son diseñados únicamente para una aplicación
* permiten reducir la redundancia de datos, al usar solo un almacén de datos al que acceden múltiples usuarios concurrentemente, pudiendo aplicar una **redundancia controlada**, es decir, crear redundancia a propósito por motivos de rendimiento, ahorrando operaciones.
* Evita la inconsistencia de los datos, ya que se mantiene actualizada para todos los usuarios.
* Permiten mantener un formato de datos.
* Tienen independencia física y lógica los programas que consumirán la base de datos, debido al concepto de abstracción de datos, el DBMS da una representación conceptual de los datos que no incluye detalles de su almacenamiento o implementación interna de las operaciones.
* Permiten compartir los datos entre múltiples personas
* Permiten aplicar restricciones de seguridad mas avanzadas
* Permiten mantener la integridad de los datos, es decir, aplicar **restricciones de integridad** que determinan que puede y que no puede ingresarse a la base de datos y como.

por otro lado, los sistemas de bases de datos son mas costosos de instalar, no están estandarizados y suele requerir de un personal especializado para su creación y mantenimiento.

## Usuarios de una base de datos 

##### Administrador de la base de datos (DBA,  database administrator)

Es el responsable de la administración de los recursos de la base de datos, es responsable del acceso autorizado a la base de datos, la coordinación y monitorización de su uso, implementa su integridad, es el responsable de su seguridad y rendimiento, generalmente asistido por un equipo.

##### Diseñadores de las bases de datos

Responsables de identificar los datos que se almacenan en la base de datos y de elegir las estructuras para representarlos y almacenarlos.

##### Analistas de sistemas y programadores de aplicaciones

##### Usuarios finales

los usuarios finales son los que requieren el acceso a la base de datos para trabajar con ella, estos pueden ser 
* Usuarios finales casuales: consultas ocacionales para obtener información que no suele estar enlatada en una petición típica, puede variar lo que requieran.
* Usuarios finales principiantes o paramétricos: consultas y actualizaciones constantes a la base de datos a través de transacciones enlatadas (operaciones programadas y probadas cuidadosamente), por ejemplo, un cajero bancario, reservar una aerolínea. 
* Usuarios finales sofisticados: ingenieros, científicos, etc, que están altamente familiarizados con el DBMS, por lo que lo utilizan para implementar aplicaciones y requisitos complejos.
* Usuarios finales independientes: uso personal del sistema
##### Implementadores de sistemas DBMS
##### Implementador de herramientas
##### Personal de mantenimiento


## Esquemas, instancias y estado de la base de datos

Llamamos ***Esquema de la bases de datos*** a la descripción de esta, que es especificada durante la fase de diseño (cuando definimos una base de datos, definimos su esquema al DBMS), que pude visualizarse con un ***diagrama del esquema***, y donde cada objeto del esquema es denominado ***estructura de esquema***. 

![[Pasted image 20250321130245.png]]

Luego, los datos reales de la base de datos en un momento concreto se denomina ***estado de la base de datos, snapshot o instancias***.

## Arquitectura de tres esquemas

Se define una base de datos con los siguientes tres niveles en su esquema

1. Nivel interno: tiene un esquema interno describiendo la estructura del almacenamiento físico de la base de datos
2. Nivel conceptual: tiene un esquema conceptual que describe la estructura de toda la base de datos para todo el conjunto de usuarios, es decir de única visión, ocultando detalles técnicos del almacenamiento, enfocándose en tipos de datos, relaciones, operaciones, restricciones, etc.
3. Nivel externo o de vista: tiene un esquema externo / vistas de usuario que describe la parte de la base de datos de interés para un grupo particular de usuarios ocultando el resto.

también se conoce como arquitectura ANSI/SPARC

![[Pasted image 20250321140240.png]]

El concepto de mapeado conceptual/interno y externo, hace referencia a cuanto es posible modificar en cada esquema hasta que se termine requiriendo modificar otro de los esquemas, debido a que el mapeado entre ambos se corresponde solo entre algunos de sus elementos y no todos.
