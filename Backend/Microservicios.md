### Características de Microservicios:

* Independencia: Cada microservicio es una unidad independiente, lo que significa que puede ser desarrollado, probado, desplegado y escalado por separado.
* Comunicación basada en APIs: Los microservicios se comunican entre sí a través de interfaces bien definidas, como APIs RESTful o colas de mensajes (streaming de datos?).
* Escalabilidad: Los microservicios permiten escalar solo los servicios que necesitan más recursos, lo que mejora el rendimiento y la eficiencia.
* Resiliencia: Si un microservicio falla, no afecta a los demás, lo que mejora la disponibilidad y la resistencia del sistema.
* Flexibilidad Tecnológica: Cada microservicio puede estar implementado con diferentes tecnologías, lo que permite elegir la mejor tecnología para cada funcionalidad.

### Consideraciones importantes al desarrollar un microservicio

* Cada microservicio puede tener su propia base de datos o compartir una base de datos con otro
* Los microservicios deben poder comunicarse entre si y debe ser gestionado mediante HTTP, mensajes asincronos o eventos
* Cada microservicio debe implementar su propio manejo de errores
* Cada microservicio debe tener medidas de seguridad para la autenticacion y autorizacion
* Cada microservicio debe implementar un metodo para logging y monitorizacion

### Microservicios y endpoints

Los microservicios se comunican atraves de endpoints, son los puntos de entrada para las solicitudes y respuestas

En general, cada endpoint expondra una operacion o mas que puede ser invocadas para ese microservicio, por ejemplo en HTTP, tenemos GET, POST, PUT y DELETE entre otros, y podra ser identificado de forma univoca, en protocolos de bajo nivel atraves de una IP y Puerto y en HTTP por una URI que define al endpoint

### Microservicio especializado

Los microservicios deben estar especializados en una funcionalidad con limites claros, de esta forma se minimiza el acoplamiento entre los microservicios y se maximiza la cohesion interna de estos:

Por ejemplo, en una aplicación de reservas de viajes, podríamos definir los siguientes microservicios: 

* Microservicio de Reservas: Maneja la creación, actualización y cancelación de reservas
* Microservicio de Usuarios: Gestiona la autenticación, autorización y perfil de los usuarios.
* Microservicio de Pagos: Procesa los pagos y gestiona las transacciones financieras.
* Microservicio de Notificaciones: Envía notificaciones por correo electrónico y SMS a los usuarios.

### Principios Básicos de los Endpoints

1. Simplicidad y Consistencia: Los endpoints deben ser simples, intuitivos y consistentes a lo largo de toda la aplicación. Esto facilita su uso y mantenimiento.
2. RESTful: Aunque no es obligatorio, es una práctica común estructurar los endpoints siguiendo los principios REST (Representational State Transfer). Esto incluye el uso de HTTP verbs (GET, POST, PUT, DELETE) y la utilización de URIs claras y significativas.
3. Versionado: Es importante versionar los endpoints para manejar cambios y actualizaciones en la API sin interrumpir el servicio para los clientes existentes