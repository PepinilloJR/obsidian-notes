Como vimos, un sitio estático, esta contenido todo en archivos HTML, donde el servidor que lo hostea, tiene la responsabilidad de mapear cada URL a un archivo HTML especifico, esto hace que cada archivo se relacione a través de Links, a su vez, cambios en la pagina, se traducen a cambios de los archivos HTML.

Una SPA es un programa, o aplicación web, que es posible utilizar desde una sola pagina, es decir, el navegador lo descarga de forma completa, y a través de cambios en la lógica o los datos del programa, cambia lo que muestra al usuario, pero sin necesidad de cargar otros archivos HTML o nuevos recursos

### HTTP

HTTP (hypertext transfer protocol) es un protocolo que se usa para la comunicación a través de archivos XML o HTML

esta orientado a realizar transacciones, siguiendo el esquema de petición-respuesta típica del modelo cliente-servidor, adicionalmente, es sin estado, es decir, es puramente transaccional, no guarda información sobre conexiones anteriores.

La estructura usada es en forma de mensajes HTTP, que tienen una estructura como la que sigue:

**Petición**:

-> Request line: consiste del método a usar, la URL del servidor y la versión del protocolo HTTP
-> Header: el header contiene metadatos del mensaje que indicaran criterios sobre la petición al servidor, por ejemplo, aceptar archivos JSON.
-> Body: datos en formato String que se le envía al servidor como parte del mensaje

**Respuesta**:

-> Status line: sección del mensaje de respuesta que contiene el estado con el que llego
-> Header: en este caso, metadatos del mensaje para el cliente, por ejemplo, indicando el tipo de valor que contiene la respuesta
-> Body: contenido de la respuesta, generalmente un String con formato JSON o HTML

#### Métodos y códigos de estado

Los métodos HTTP son un indicativo al servidor para decirle que hacer, ya sea retornar un conjunto de datos o realizar cambios en alguna base de datos, cada método tiene un propósito especifico, aunque a veces pueden usarse fuera de su objetivo original.

![[Pasted image 20250327162654.png]]

A su vez, para cada petición, el servidor debe enviar una respuesta al cliente, y es importante que contenga un código de estado acorde al resultado que se dio en el servidor.

![[Pasted image 20250327162749.png]]

### HTTPS y HTTP

HTTP originalmente es un protocolo de transferencias que no cifra la información que es enviada a través del servidor o el cliente

Esto genera problemas y posibilita vulnerabilidades como un **man in the middle.**

una solución es el protocolo HTTPS, una versión mas segura y cifrada de HTTP, que ademas tiene peculiaridades como la necesidad de un certificado SSL, esto en general lleva algo mas de trabajo para el ingeniero, pero bueno, es mas seguro

esto no significa que una pagina HTTP sea peligrosa de por si, solo que se debe tener mas cuidado con el tipo de información que se enviara a través de esta.