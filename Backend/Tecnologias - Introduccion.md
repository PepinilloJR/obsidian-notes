
Para el lenguaje de programacion utilizaremos JAVA, un lenguaje OOP

Luego, utilizaremos Spring Framework, que proporciona una infrastructura completa para crear aplicaciones empresariales, incluyendo modulos para inyeccion de dependencias, acceso a datos, seguridad, transacciones y mas.

este incluye:

Spring boot que facilita la creacion de aplicaciones Spring capaces de funcionar sin instalar un servidor externo (tiene en realidad un servidor embebido llamado tomcat) lo cual es una gran molestia en aplicaciones de Java, y listas para produccion.

Spring data que es un modulo que facilita el acceso a bases de datos

Spring Gateway  o Spring Cloud Gateway proporciona un enrutador para aplicaciones basadas en microservicios, esto facilita el enrutamiento y filtrado de peticiones en los servicios, no es lo mismo que un enrutador del palo express routes, mas bien, se encarga de enrutar la peticion del cliente al servicio correcto. 

Spring security es el modulo de spring para la seguridad, dando herramientas para la autenticacion, autorizacion, proteccion contra distintos ataques y demas.

Luego, como ORM utilizaremos Hibernate, que esta basado en JPA, que es un estandar para los ORMs en Java, por lo que lo que aplique en JPA se aplica en Hibernate (JPA no es una implementacion en si)

Para el testing, utilizaremos JUnit, para la escritura de pruebas automatizadas, y Mockito para facilitar el mocking en las pruebas unitarias.

Finalmente, para las diferentes dependencias, se utiliza Maven, donde su archivo de configuracion en los proyectos es un archivo xml, `pom.xml`

Para la base de datos, utilizaremos H2 inicialmente, que es un motor de bases relacional sencillo y embebido dentro de la aplicacion, luego utilizaremos PostgreSQL
