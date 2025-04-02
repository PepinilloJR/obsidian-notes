como vimos, un DBMS es un sistema de software complejo, compuesto por muchas partes que se interrelacionan para proveer sus funciones

> ***definición***, ***construcción***, ***manipulación*** y ***compartición*** de bases de datos entre usuarios y aplicaciones, y de proveer **seguridad, integridad de sus datos, respaldo y recuperación de datos**, y de manejar correctamente las **transacciones**

Tomemos de referencia el siguiente gráfico, que describe las interacciones y componentes propios del DBMS:

![[Pasted image 20250401171853.png]]

### Componentes del DBMS:

**Compilador DDL**: 

El Compilador realiza la función inicial para la configuración de una base de datos de un DBMS, que es procesar las definiciones de los esquemas en sentencias DDL, almacenando luego las descripciones de los esquemas en un **catalogo sistema / diccionario de datos**, estas descripciones son lo que llamamos **metadatos**.

>  Este catalogo contiene información crucial de la base de datos, tales como nombres y tamaños de los archivos, nombres y tipos de los datos, detalles del almacenamiento de estos, entre otros, que son importantes para que los módulos del DBMS pueda funcionar. 

**Compilador de consultas**:

Para usuarios casuales, que recordemos realizan consultas no predefinidas, mediante una interfaz de consulta interactiva (cualquier interfaz que le permita realizar la consulta a la base de datos), el compilador de consultas analiza estas consultas sistemáticamente para garantizar la corrección de las operaciones que se quieren realizar, que los nombres de los elementos sean correctos, etc. luego, se compila en un formato interno del sistema, este formato se envía entonces al **Optimizador de consultas**.

Ejemplo de consulta interactiva, realizando una consulta SQL, a través de alguna interfaz de consultas interactiva:

```
SELECT nombre, edad FROM clientes WHERE ciudad = 'Madrid';
```


**Optimizador de consultas**:

El optimizador de consultas toma los formatos internos resultantes de la compilación de consultas interactivas, y los reconfigura, reordena, elimina redundancias, y usa algoritmos para mejorar la ejecución de la consulta, genera entonces finalmente, un código ejecutable capaz de realizar las operaciones que fueron solicitadas en la consulta

**Precompilador DML**: 

El precompilador se encarga de tomar programas de aplicación provistos por programadores, otro tipo de usuario que ya mencionamos, y extrae todos los comandos DML que este contiene para su compilación, el resto del lenguaje Host es enviado al compilador del lenguaje Host.
Finalmente, los comandos DML son enviados al compilador DML.

**Compilador DML:** 

El Compilador DML recibe los comandos DML, y los traduce en código objeto (conjunto de instrucciones que el equipo puede ejecutar, recordar SSL)

Finalmente, se enlaza el código objeto de los comandos DML y del lenguaje Host, formando así una transacción enlatada o transacción compilada, estas transacciones resultan útiles, ya que son las que usan los Usuarios finales principiantes o paramétricos

es decir, estamos hablando de aplicaciones que usan SQL directamente en su código, sin ORM, es decir, SQL embebido, esto es algo antiguo y típico de C, COBOL, entre otros, y que interactuan directamente con la base de datos.

> En realidad, estas transacciones son **programas de aplicación** escritos en un lenguaje como **C, COBOL o Java**, que incluyen comandos SQL embebidos. Estos programas se **precompilan y enlazan** con el sistema de base de datos para que puedan ejecutarse muchas veces sin recompilarse.

**Procesador de bases de datos runtime**

el procesador de bases de datos runtime es una pieza de software del DBMS, que se encargara de finalmente ejecutar, tanto comandos privilegiados del DBA, así como las consultas ejecutables y las transacciones enlatadas que mencionamos en los anteriores compiladores.

Este trabaja usando el Catalogo del sistema, se ayuda del administrador de datos almacenados para poder realizar operaciones de E/S en el almacenamiento del SO, realizando así las operaciones finalmente en la base de datos almacenada en disco

adicionalmente, se destaca que el procesador, tiene subfunciones que se encargan de que, al realizar operaciones, sea posible la recuperación, la creación de copias de seguridad y controlar la concurrencia de las operaciones 