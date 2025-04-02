### Arquitectura centralizada

la arquitectura centralizada es la mas primigenia, proporcionaba un mainframe que provee de absolutamente todas las funcionalidades del sistema, incluyendo la del DBMS, accediendo cada usuario a través de una terminal.

![[Pasted image 20250401203904.png]]
### Arquitecturas cliente / servidor básicas:

El modelo cliente / servidor aplicado a SO, donde se definen servidores especializados para funcionalidades especificas y sirven servicios a clientes en una red, esto es aplicable también al DBMS 

![[Pasted image 20250401213133.png]]
### Cliente / servidor de dos capaz

aquí, denominamos al servidor como servidor de consultas o transacciones, proveyendo estas dos funcionalidades, también denominado servidor SQL en un DBMS relacional

por otro lado, la interfaz de usuario y las aplicaciones se disponen en el cliente, mientras, cuando se requiere acceder al DBMS, se crea una conexión con el servidor para realizar las transacciones requeridas.

Aquí las dos capas serian el servidor y el cliente.

la conexión se realiza mediante el estándar de conectividad abierta de bases de datos (ODBC, Open Database Connectivity), que proporciona una API que permite a los clientes realizar llamadas al servidor que contiene al DBMS, en general el proveedor del DBMS lo implementa.

![[Pasted image 20250401220938.png]]
### Arquitecturas de tres capas y n capas para las aplicaciones web

Las aplicaciones web, por lo general utilizan esta arquitectura, añadiendo una capa intermedia entre el cliente y el servidor de la base de datos, denominada servidor de aplicaciones o servidor web.
Su rol es almacenar las reglas comerciales (procedimientos o restricciones) usados para el acceso de los datos del servidor de la base de datos, proveyendo ademas una capa extra de seguridad antes de modificar datos, a la que el usuario tiene complicado acceso.

consideremos adicionalmente, que podemos agregar mas capas, desagregando las funcionalidades de cada capa, en mas capas, como puede ser el middleware, una capa que se encargara de facilitar, configurar, validar y flexibilizar la comunicación entre la aplicación del cliente hacia el servidor web, o por otra parte, lo mismo entre el servidor web y el servidor de base de datos

![[Pasted image 20250401223403.png]]

La siguiente imagen resume como funciona el acceso a los datos interno considerando los esquemas vistos durante toda esta sección.

![[Pasted image 20250401231040.png]]