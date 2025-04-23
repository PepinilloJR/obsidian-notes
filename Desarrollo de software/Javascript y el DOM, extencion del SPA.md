Con Javascript podemos crear paginas dinámicas, esto es, que cambien según el input del usuario, esto se puede hacer haciendo uso del DOM (Document
Object Model) en javascript, que nos da acceso al HTML de la pagina en forma de un objeto de javascript

Esencialmente es una API de programación para documentos HTML para javascript, que nos permitirá acceder y modificar a este.


![[Pasted image 20250421220729.png]]
(debemos pensar en el DOM como un árbol de objetos (etiquetas) HTML)

El objeto raíz es document, que referencia a la pagina HTML o al árbol de etiquetas de la pagina actual.
Cada elemento interno de document puede ser un ELEMENT o un NODE, ELEMENT es la representación genérica de una etiqueta HTML, NODE es una unidad básica, la cual puede ser un ELEMENT, texto, etc.

Todos los elementos HTML tendrán un tipo especifico que tendrá eventos, propiedades y métodos propios

![[Pasted image 20250421221213.png]]

Para obtener estos elementos podemos obtenerlos por referencia desde document, usando por ejemplo el ID:
Esto lo que hara sera buscar por todo el arbol del DOM un elemento con el ID y devolverlo como un objeto de tipo especifico HTML

![[Pasted image 20250421221657.png]]

Una vez obtenido, podemos modificarlo y se aplicaran los cambios en la pagina 

![[Pasted image 20250421221728.png]]

### Extensión del concepto de SPA

Nosotros sabemos que una SPA es una pagina web que todo su contenido es cargado una sola vez, con la descarga de un único HTML. (a veces con la excepción de poder cargar nuevo contenido de forma dinámica sin tener que recargar o acceder a otra pagina, mediante peticiones)

Aplicando cambios dinamicos al DOM, podemos permitirnos tener multiples vistas para la pagina SPA.

sobre esto, debe saberse que no implica que no haya cambios en la URL -> esto no implica que se carga una nueva pagina, si no que se cambia de vista, por esto es necesario entender las rutas en SPA.

### Hash URL 

Una URL puede tener una palabra separada por un símbolo #, conocido como elemento hash de la URL, esto posiciona al usuario en una parte determinada de la pagina, sin cambiar a otra pagina.

Es posible conocer informacion sobre la URL mediante el objeto ***Location***, donde podemos acceder al valor del Hash

adicionalmente, existe el evento en ***window***, denominado hashchange, para detectar cuando se cambio el hash.

con estas dos herramientas podemos definir un ruteo para las URL en una SPA, de modo que mostremos diferente contenido segun en que hash nos encontremos

### Peticiones AJAX 

las peticiones AJAX (_Asynchronous Javascript and XML_) son peticiones HTTP que se pueden realizar de forma asincrona desde javascript, de este modo, con la necesidad de cargar nuevo contenido en una SPA, como podria ser hacer una peticion al Backend al ir a otra seccion de la SPA (que no hubiera hecho falta que estuviera cargado antes), podremos traer estos datos sin recargar la pagina y el resto de recursos.

![[Pasted image 20250421223523.png]]

### SEO en una SPA

El SEO en una SPA queda totalmente inservible, debido a que gran parte del HTML se inserta de forma dinamicamente y no como parte del archivo HTML principal, que es el analizado por el SEO.

Sin enbargo, con el tiempo los buscadores han implementado mecanismos para detectar codigo HTML generado dinamicamente por una SPA mediante javascript




