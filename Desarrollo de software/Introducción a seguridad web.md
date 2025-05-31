La seguridad de sitios web eficaz requiere de esfuerzos de diseño a lo largo de la totalidad del sitio web: en tu
aplicación web, en la configuración del servidor web, en tus políticas para crear y renovar contraseñas, y en el
código del lado cliente.

Muchos de los frameworks actuales proveen mecanismos de seguridad bastante robustos

### OWASP

OWASP o Open Web Application Security Project, es una fundacion que trabaja en proyectos de codigo abierto para mejorar la seguridad del software.

OWASP presenta a modo de concienciación un listado de vulnerabilidades mas comunes de los sitios web

![[Pasted image 20250526145349.png]]

### Broken Access Control

un usuario accede a recursos que no debería debido a una mala implementacion en los controles de acceso de estos recursos.

Aquí algunas mitigaciones:

![[Pasted image 20250526145644.png]]

### Cryptographic Failure

Vulnerabilidades causadas por utilizar una criptografia de forma errónea o cuyo algoritmo es debil, deprecated o defectuoso, puede ocurrir en el almacenamiento de contraseñas, la transmisión de datos, autenticación de usuarios, etc

### Inyección SQL

Las vulnerabilidades de inyección SQL habilitan a usuarios maliciosos ejecutar código SQL de forma remota y arbitraria en una base de datos a la cual no debería tener acceso

Esta vulnerabilidad está presente si la entrada del usuario que se pasa a la sentencia SQL subyacente puede cambiar el significado de la misma

Por ejemplo:

Si se pretende listar los usuarios con un nombre, que se suministra de forma directa desde un formulario HTML.

```
statement = "SELECT * FROM users WHERE name = '" + userName + "';"
```

Aquí puede insertarse sentencias SQL que alteren el comportamiento original

`a';DROP TABLE users; SELECT * FROM userinfo; --`

Resultando

```
SELECT * FROM users WHERE name = 'a';DROP TABLE users; SELECT * FROM userinfo; --
';
```

La manera de evitar esta clase de ataque es asegurar que cualquier dato de usuario que se pasa a un query SQL no puede cambiar la naturaleza del mismo. Una forma de hacer ésto es eludir/evitar todos los caracteres en la entrada de usuario que tengan un significado especial en SQL (y las palabras clave de SQL).

En general los frameworks que vimos tienen cuidado de evitar esta vulnerabilidad por el programador.

### Insecure Design y Security misconfiguration

El ***Insecure Design*** se origina en la etapa de diseño y ocurre cuando el diseño no considera adecuadamente los diferentes riesgos de seguridad posibles.
Afecta a la mayoría si no todas las áreas de la aplicación que implican riesgos de seguridad.


La ***Security misconfiguration*** se origina en la configuración de la aplicación una vez implementada
Esto puede suceder al configurar el servidor web, la base de datos, la plataforma donde se desarrollo, o en la aplicación en si.

Por ejemplo, contraseñas débiles, exposición de informacion de depuración del servidor, permisos de ejecución excesivos de la aplicación, usar versiones viejas del software, falta de auditorias y pruebas de seguridad

![[Pasted image 20250526152136.png]]

### Vulnerable and Outdated Components

Se da cuando existen componente de software obsoletos o con vulnerabilidades conocidas que forma parte de la aplicación. Por ejemplo frameworks, modulos, paquetes, librerias, etc.

![[Pasted image 20250526152349.png]]
### Identification and Authentication Failures

Se produce cuando la aplicación no implementa adecuadamente los mecanismos para identificar y autenticar un usuario, por ejemplo usando contraseñas débiles, transmisión de datos no encriptada, la falta de la misma en áreas criticas, o gestionando de forma inadecuada las diferentes sesiones.

### Software and Data Integrity Failures

Vulnerabilidad que se produce cuando los datos o el software de una aplicación web son modificados o dañados de manera malintencionada o accidental. Dada la naturaleza del daño o modificación, esto puede dar lugar a problemas de seguridad.

### Security Logging and Monitoring Failures

Vulnerabilidad que se da cuando la aplicación no cuenta con mecanismos para el seguimiento y registro de la seguridad de la aplicación.
Por lo tanto no existe forma de visualizar los eventos de seguridad que sucedieron y no es posible desarrollar una respuesta.

### Cross Site Request Forgery (CSRF) 

También llamados ataques CSRF, permiten a un usuario malicioso ejecutar acciones usando credenciales de autenticacion de otro usuario.

Este tipo de ataque fuerza al navegador web que se encuentra validado por la pagina, a enviar una peticion a la aplicación, desde otro sitio.

![[Pasted image 20250526154016.png]]

Parte del truco, es que si el usuario tiene una sesión iniciada en este sitio, el navegador mantiene la informacion necesaria para autenticar las acciones por parte del usuario (cookies por ejemplo), por lo que peticiones desde el mismo navegador desde otro sitio serán pasados como validos

Una prevención posible es que el servidor ademas de los datos de autenticacion contenido en las cookies, requiera también que la peticion incluya una palabra secreta especifica para el usuario que es generada por el sitio y no es accesible para el otro sitio
Los frameworks web incluyen con frecuencia tales mecanismos de prevención de CSRF.

### Cross-Site Scripting (XSS)

El ataque CSRF atacaba la confianza del servidor en el usuario, en cambio, el XSS ataca la confianza del usuario en el servidor web.

Esta clase de ataques permiten al atacante inyectar scripts del lado del cliente a través del sitio web de confianza

Hay dos aproximaciones principales para conseguir que el sitio devuelva scripts inyectados al explorador

#### XSS Indirecto (reflejado)

Consiste en modificar valores que la aplicación web utiliza para pasar variables entre dos páginas, sin usar sesiones y sucede cuando hay un mensaje o una ruta en la URL del navegador, en una cookie, o cualquier otra cabecera HTTP (en algunos navegadores y aplicaciones web, esto podría extenderse al DOM del navegador).

Por ejemplo:

![[Pasted image 20250526160318.png]]

Otro ejemplo seria, consideremos un sistema de comentarios (no almacenados), si no se vigila lo que entra al servidor, y el mensaje se inserta (muy comunmente) en el HTML, entonces es posibe pasar un <script/> como mensaje e insertarlo en el HTML

```
// como se veria usado de forma bien intencionada
<h1> Titulo de comentario </h1>

// como se veria usado de forma mal intencionada mandando el dato <script/>

<h1> <script> Ejecutar codigo malvado <script/> </h1>
```
#### XSS Directo (persistente)

Este tipo de XSS comúnmente filtrado, y consiste en insertar código HTML peligroso en sitios que lo permitan; incluyendo así etiquetas como ``(<script> o <iframe>``.
Es persistente ya que genera cambios en el servidor y termine afectando a todos los usuarios de la pagina

Por ejemplo:

![[Pasted image 20250526160519.png]]

Un ejemplo seria un sistema de comentarios, el atacante sube un comentario cuyo contenido es un <script/> de JS, el servidor lo recibe, lo almacena y lo envia de modo que es ejecutado en todos los clientes que entren a la pagina a ver el comentario.

```
// como se veria usado de forma bien intencionada
<h1> Titulo de comentario </h1>

// como se veria usado de forma mal intencionada mandando el dato <script/>

<h1> <script> Ejecutar codigo malvado <script/> </h1>
```

El proceso de modificar los datos del usuario de manera que no puedan utilizarse para ejecutar scripts o que afecten de otra forma la ejecución del código del servidor, se conoce como ***"desinfección de entrada" (input sanitization)***. Muchos frameworks web desinfectan automáticamente la entrada del usuario desde formularios
HTML, por defecto.

### Clickjacking 

El Clickjacking se da cuando un usuario malicioso secuestra los clicks que estan dirigidos a un sitio visible y legitimo y los redirige a una pagina escondida por debajo.

Esto se hace por lo general, consiguiendo un dominio muy parecido al sitio objetivo, luego, utilizando <Iframe/> que permite mostrar una pagina web a través de su URL en otra pagina web, simulara ser el sitio oficial, y asi capture las entradas del usuario.

Como defensa, tu sitio puede protegerse de ser embebido en un iframe de otro sitio configurando las cabeceras HTTP apropiadamente.

### Denegación de Servicio (DoS)

Este ataque es bastante común, se inunda un sitio objetivo con enormes cantidades de peticiones, interrumpiendo el servicio a usuarios legítimos debido a la sobrecarga de recursos.

La defensa de estos ataques suele venir del lado del servidor como tal y no de la aplicación web misma

### Salto de directorios / revelación de archivos

se da cuando el usuario intenta acceder a partes del sistema de archivos del servidor web mediante la navegación, por ejemplo (../../).

Para ello se sanitiza la entrada del usuario en el sistema de navegación eliminando patrones peligrosos (como `../`) de las rutas, permitiendo solo nombres de archivos válidos, entre otras formas.

### Inclusión de archivos y Inyección de Comandos

En este ataque un usuario es capaz de especificar, para mostrar o ejecutar, un archivo "no intencionado para ello" en los datos que le pasa al servidor. Una vez ha sido cargado este archivo podría ejecutarse en el servidor web o en el lado cliente (llevando a un ataque XSS). La solución es desinfectar la entrada antes de usarla.

La inyección de comandos es específicamente conseguir ejecutar comandos del sistema operativo de host, generalmente porque se ejecuta comandos que toman de referencia la entrada del usuario, similar a la inyección SQL.


### Medidas concretas y generales para la seguridad

1. No confiar en la entrada de datos del usuario, es correcto desinfectar todas las entradas.
2. Gestionar las contraseñas de forma efectiva, contraseñas fuertes y cambiantes, autenticacion de dos factores, encriptar las contraseñas guardadas, etc.
3. Configurar el servidor para usar HTTP en lugar de HTTPS, encriptando todos los datos enviados entre cliente y servidor
4. Buscar ataques populares y aprender a como protegerse particularmente de cada uno
