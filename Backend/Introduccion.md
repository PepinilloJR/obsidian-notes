### La capa del backend

El backend de una aplicacion es un conjunto de servicios y componentes que trabajan en conjunto para dar soporte a la funcionalidad de la aplicacion.

Esto presenta multiples desafios:
###### Manejo de la lógica de negocio: 
El desarrollo de backend implica la implementación y gestión de la lógica
de negocio de una aplicación, es decir, las reglas y procesos que definen cómo funcionará el sistema y cómo se llevarán a cabo las operaciones.

###### Escalabilidad
La capacidad del backend para manejar un aumento en la carga de trabajo

###### Seguridad
Se deben proteger los datos y recursos del sistema de accesos no autorizados y ataques maliciosos
-> deben implementar prácticas de seguridad sólidas, como autenticación, autorización, cifrado de datos y validación de entrada

###### Rendimiento
Capacidad del backend para responder rapidamente a solicitudes de los usuarios

###### Integracion con sistemas externos
El backend debe integrarse muchas veces con sistemas externos, como servicios en la web o APIs de terceros

### Organizaciones comunes para los componentes del backend

Arquitectura monolitica:

Todos los componentes se agrupan dentro de una unica aplicacion desplegable, formando una unica unidad altamente cohesiva y acoplada

Es sencilla de desarrollar y probar, y menos compleja de desplegar, pero a su vez dificulta su escalabilidad y mantenimiento

Arquitectura basada en microservicios:

Se divide la aplicacion en servicios pequeños especializados de baja cohesion y bajamente acoplados, por lo que pueden ser desarrollados, desplegados y escalados de forma separada. Estos microservicios se comunican atraves de interfaces bien definidas.

Esto resuelve los problemas de la monolitica a cambio de una mayor complejidad en el desarrollo y gestion

Arquitectura hexagonal:

Se separa la logica del negocio (dominio o nucleo de la aplicacion) del acceso a datos y de la interaccion con sistemas externos (adaptadores), y se comunican mediante interfaces denominadas puertos


![[Pasted image 20250811170248.png]]
![[Pasted image 20250811170445.png]]
Aqui da igual que implementa el adaptador de SMS por ejemplo, ya que lo que le interesa al nucleo es poder comunicarse atraves del puerto de notificaciones, luego el adaptador podra ir cambiando su implementacion sin afectar al nucleo u otros adaptadores

Arquitectura basada en servicios (SOA):

La arquitectura basada en servicios se enfoca en crear aplicaciones como un conjunto de servicios independientes que se comunican atraves de interfaces bien definidas, por lo tanto, cada funcionalidad de la aplicacion es un servicio desacoplado y altamente interoperable

En general, su mayor diferencia con la implementacion de microservicios (una interpretacion mas moderna del SOA) es que el primero se implementa como un sistema monolitico, que implementa servicios como mecanismos para poder interactuar con otros sistemas o exposicion de funcionalidades 
