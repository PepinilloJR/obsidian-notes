NodeJS provee la capacidad de ejecutar Javascript fuera del contexto de un navegador (no es la única opción para hacer esto).

En si, NodeJS es un entorno de ejecución de javascript (NO UN FRAMEWORK), que funciona como interprete y provee del manejo de librerías y paquetes, entre otras funciones esenciales.

específicamente, NodeJS hace uso de V8, un entorno de ejecución desarrollado por google para ejecutar el código JS. Hace uso de libuv, una librería en C que provee soporte para operaciones de entrada-salida asíncronas , y en general otras librerías necesarias para acceder a funcionalidades importantes del SO

esto se denomina como STACK de Node.js, que son una serie de capas de tecnologia que trabajan juntas para ejecutar codigo Javascript "en el lado del servidor" (fuera del navegador)

![[Pasted image 20250421225110.png]]

### Características principales de NodeJS

1. Mono-hilo (Single Threaded): Solo hay un hilo de usuario (hilo de aplicación) para la ejecución del programa en NodeJS, por lo que todas las operaciones y peticiones se realizan de forma serializada. Debe considerarse evitar tareas intensivas de CPU ya que no son apropiadas para un entorno de este tipo, y que no existen problemáticas de concurrencia.
2. E/S no bloqueante: NodeJS no espera hasta que una operacion E/S termine para continuar ejecutando el código, si no que se maneja como una ***promesa***
3. Asíncrono: Se encarga de cogido que requiere una espera por parte del programa, mas tarde una vez se haya terminado

Estas características proveen ventajas de usar NodeJS:

![[Pasted image 20250421230041.png]]

### Operaciones E/S asincronas y no bloqueantes

Las operaciones E/S son todas aquellas que requieran acceder a recursos locales o remotos, lo cual implica un retraso hasta que la informacion llegue o se termine de escribir.

NodeJS utiliza un Grupo de hilos provistos por la librería Libuv para ejecutar fuera del hilo de ejecución las operaciones E/S de forma asíncrona, es decir, en vez de esperar a que estos se terminen, continuara con el resto del programa y, una vez que se finalizan, el bucle de eventos manejara mediante un callback la devolucion de estos recursos 

![[Pasted image 20250421231340.png]]

Paso a paso:

1. la pila de llamadas, ejecutara bloques de codigo de forma serializada, sin embargo, de encontrarse una operacion bloqueante del hilo, se traslada esta operacion al grupo de hilos de Libuv y se define un callback para cuando se de el evento de finalizacion de la operacion bloqueante

2. Cuando se da el evento de finalizacion, se agrega este evento y el callback a la cola de eventos, la cola de eventos contendra todas las funciones de devolucion de llamada (callbacks) de las funciones bloqueantes finalizadas, que estan a espera de ser ejecutadas en el hilo principal.

3. El bucle de eventos transfiere los callbacks desde la cola de eventos a la pila de llamadas, para ser ejecutadas por el hilo principal

4. Detalle: Cuando la pila de llamadas está vacía y la Cola de Eventos tiene funciones pendientes, el Bucle de Eventos mueve el evento y su devolución de llamada desde la Cola de Eventos a la pila de llamadas para que sea ejecutado por el hilo principal.

### NPM 

NPM es el gestor de paquetes que permite a desarrolladores compartir paquetes e instalar paquetes de forma rapida y facil que viene por defecto en NodeJS.

consiste de dos partes principales:
1. Herramienta CLI (interfaz de línea de comandos) para la publicación y descarga de paquetes.
2. Repositorio en línea que alberga paquetes de JavaScript.

NPM permite incializar un proyecto con una plantilla para la instalacion de paquetes, gestionar las dependencias del proyecto (instalar, eliminar, actualizar), crear scripts para acortar comandos, y ejecutar proyectos en base a la configuración aplicada por NPM.

npm init creara la plantilla inicial, creando el archivo package.json, donde entre otras configuraciones, apareceran los paquetes que vayamos instalando
Es destacable analizar como se simboliza las versiones de estos paquetes en la configuracion:

![[Pasted image 20250422002156.png]]

Si no se encuentra ninguno de estos simbolos, NPM buscara la version exacta indicada en dependencies

ademas de dependencies, existe la seccion de devdependencies, que indicara un conjunto de paquetes que NO deben ir a produccion.
para instalar en modo dev, se usa  ***npm install (paquete) --save-dev***

podemos adicionalmente obtener solo los paquetes de produccion usando ***npm install --production***

### Promesas

Dentro de la programación asíncrona, Javascript presenta un concepto denominado promesa, un objeto que representa la respuesta de una tarea asíncrona, de la que no es posible tener un valor inmediato debido a que no ha finalizado.

Esto nos quiere decir que, si realizamos una peticion HTTP, hasta que esta peticion se ***resuelva***, o se ***rechace***, tendremos a disposición la promesa resultante de la peticion.

Nosotros sabemos entonces que hasta que se termine de procesar la operacion asíncrona, tendremos una promesa, y que cuando esta se termine, tendremos un callback ejecutándose en el hilo principal que se encargara de devolver los datos (spoiler: resolve y reject)

ejemplo:

```
let mensaje = new Promise((resolve, reject)=>{
setTimeout(function () {
resolve('Este es el mensaje');
}, 2000);
});
```

***resolve*** sera un callback que devolverá el resultado una vez se considere que la operacion finalizo, ***reject*** sera el callback encargado de entregar un error en caso de que la operacion fallara.

### Control de promesas: then/catch y async/await

Las promesas pueden controlarse, es decir, capturar los resultados de las promesas una vez finalicen, para ello hacemos uso de then/catch y mas adelante, await.

***then()*** es un método de los objetos promesas con el que podemos indicar un callback que maneje los datos entregados por ***resolve()***

***catch()*** es un método de los objetos promesas con el que podemos indicar un callback que maneje los errores entregados por ***reject()***

```
// suponiendo que mensaje es una promesa
mensaje.then(m =>{
console.log(m);
}).catch(function () {
console.log('error');
});
```

funciones como fetch esta implementado con promesas por lo que podemos usar then/catch con este

```
let pokemones = fetch("https://pokeapi.co/api/v2/pokemon/1");
pokemones
.then(res => {
return res.json()
})
.then(data => {
console.log(data.name);
}).catch(error => console.log(error))
```

El uso de la palabra clave ***await***, por otro lado, pertenece a otro modo de pensar las promesas:

Si nuestra promesa y las operaciones que se harán con los resultados de esta están encapsulados en una función, nosotros podríamos decir que esa función es asíncrona, y dentro de esta, manejar toda la ejecución de forma sincrona.

Esto es lo mismo que venimos haciendo con then y catch (si se hace bien), de modo que, en nuestra función, podremos indicarle con await que espere a la finalizacion de una promesa antes de continuar con el resto de código contenido en la función (esto es lo mismo que usar then) que realizara las operaciones con los datos, donde el resultado de un await promise sera el valor provisto por el resolve o el reject, estamos ***"esperando los resultados de un resolve o un reject"***.

Para poder utilizar await, debe definirse la función como función asíncrona utilizando la palabra clave ***async***, esto resultara en que la función asíncrona que definamos, si tiene un return, actuara como promesa también, por lo que en el hilo principal, sus bloqueos no interrumpirán la ejecución normal del código, y permitirá el uso interno de await.

Esto nos ahorra tener que implementar tantos callbacks dentro de la promesa.

```
function obtener_pokemones(){
let url = "https://pokeapi.co/api/v2/pokemon/";
return fetch(url).then(res => {return res.json()});
}
async function nombrar_pokemones() {
let pokemones = await obtener_pokemones();
pokemones.results.forEach(pokemon => {
console.log(pokemon.name)
})
}
```

