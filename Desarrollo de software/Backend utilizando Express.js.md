
Antes de utilizar Express, veamos algunos conceptos sobre backend y arquitectura de software

### Arquitectura de microservicios

Todos los procesos del software se distribuyen en múltiples partes pequeñas e independientes, denominados "componentes de servicios" o "microservicio" que están enfocados en realizar una tarea o encargarse de una área del negocio de la aplicación.

La funcionalidad de cada componente se expone a través de una API para que los demás componentes accedan a esta.

### API - HTTP

En general el API para cada uno de los microservicios utilizan HTTP para el acceso a estos.

Un API debe tener las siguientes características:
Extensibilidad: Su funcionalidad debe ser fácil de extender
Predictibilidad y consistencia: El API debe tener un comportamiento esperable y consistente entre sus diferentes funciones.
Errores claros: se deben evitar y manejar los fallos, y dar informacion clara sobre estos. (imagina si por fallar una subida de datos todo el microservicio fallara porque el API no implementa esto)

### API REST

Las API REST son un estándar sobre la arquitectura que debe tener una API basado en el protocolo HTTP

* Cliente/servidor: Se basa en el modelo cliente/servidor, el API define una interfaz de comunicación entre ambos
* Sin estado: Cada peticion realizada al API es completamente independiente de la que sigue.
* Identificador único (URI): todo recurso del servicio se accede desde el API con un identificador único. 
* Métodos HTTP: REST debe respetar los verbos y códigos de estado de HTTP 

Definiremos al ***API RESTful*** como una interfaz entre los que dos sistemas intercambian informacion usando una arquitectura REST

### Principios de diseño de API RESTful

1. Keep it simple: las URI (no URL) deben ser simples, cortas, e intuitivas sobre que recursos son accedidos.
2. Usar SUSTANTIVOS no VERBOS en URI: evitemos usar los verbos HTTP en las URI, en su lugar sustantivos que describan al recurso
3. Usar adecuadamente los métodos HTTP y los códigos: debemos usar los métodos HTTP de forma adecuada para cada funcionalidad del API, por ejemplo, no usaremos un GET para crear un recurso, y si se crea usaremos el código apropiado como respuesta.
4. Plurales en colecciones de recursos: Si el URI referencia a un recurso en masa, seria correcto referirse a este en plural en el URI
5. Versionado: a la hora de cambiar el API, utilizar versiones para que los clientes tengan tiempo para cambiarse a la mas nueva
6. Usar paginacion: Cuando la API trabaja con grandes cantidades de datos, es buena practica permitir parametros de limit y offset para determinar cuantos datos se quieren recuperar, entregando paginas de estos.
7. Formatos soportados: en general se puede usar el formato que se quiera para las respuestas y peticiones, pero en general los mas usados son JSON y XML
8. Usar mensajes de error adecuados.



# EXPRESS

Express es un framework de desarrollo web para Node.js, que proporciona herramientas para desarrollar aplicaciones web y APIs de forma rapida y sencilla.

En general, facilita las siguientes funcionalidades de una API RESTful:

* Enrutamiento: Express proporciona una API sencilla y flexible para definir rutas de servidor y responder a solicitudes HTTP.
* Manejo de middleware: Express permite utilizar y crear middleware de manera sencilla, lo que permite integrar funcionalidades adicionales a la aplicación web.
* Renderizado de vistas: Express.js admite el renderizado de vistas en el lado del servidor. Se pueden utilizar motores de plantillas como EJS, Pug y Handlebars para renderizar vistas.
* Acceso a la base de datos: Express puede utilizarse junto con una variedad de bases de datos populares, como MongoDB o MySQL, para crear aplicaciones web con funcionalidades avanzadas.
* Integración con otras librerías de Node.js: Express es compatible con una variedad de librerías y paquetes de Node.js, lo que permite crear aplicaciones web robustas y escalables.
* Unopinionated: “no dogmático”: lo que significa que contrariamente a otros frameworks nos dará total libertad para plantear nuestro producto mientras que todas las decisiones acerca de la arquitectura, las mejores prácticas y las convenciones deberán ser establecidas por nosotros mismos.

### Enrutamiento basico:

Aqui un ejemplo de un enrutamiento y servicio de recursos basico usando Express.js

```
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
res.redirect('/Inicio'); // redireccion
});

app.get('/Inicio', (req, res) => {
res.send('Pagina de Inicio!');
});

app.get('/AcercaDe', (req, res) => {
res.send('Pagina de Acerca!');
});


// .use permite el uso de middleware
app.use('', (req, res, next) => {
res.send('Pagina no econtrada!');
next()
});

app.listen(port, () => {
console.log(`Aplicacion Hola Mundo escuchando en el puerto ${port}`);
});
```


### Propiedades de una Request

Las request son un poco menos intuitivas que las response

***request.params***: accede a los parametros de la ruta, que son parte de la ruta y representan recursos especificos y ***son obligatorios***

```
app.get('/articulos/:id', (req, res) => {
res.send('Se solicito el articulo ' + req.params.id);
});
```

***request.query***: accede a los parametros de consulta de la ruta, que van al final de la ruta luego de un ***?*** y ***son opcionales***

```
app.get('/articulos', (req, res) => {
console.log(req.query.Nombre);
console.log(req.query.Activo);
res.send(´Se solicito los artículos con Nombre: ${req.query.Nombre} y con
Activo: ${req.query.Activo}´;
});
```

### Router

La clase express.router tiene como finalidad crear un manejador de rutas montable y modular, una instancia router es un sistema de middleware, por lo que funciona como una miniaplicacion que se encarga de gestionar las rutas de la aplicacion.

De este modo, podemos mediante .use montar un router, para una ruta inicial, de esta forma es posible organizar mejor el proyecto y darle un funcionamiento mas modular 

Veamos un ejemplo:

```
const express = require('express');
const usuariosRouter = express.Router();

usuariosRouter.get('/', (req, res) => {
  res.send('Lista de usuarios');
});

usuariosRouter.post('/', (req, res) => {
  res.send('Crear usuario');
});

module.exports = usuariosRouter;
```

Aqui creamos entonces un router para los usuarios, que podremos insertar en app.js como un middleware

```
const express = require('express');
const app = express();

const usuariosRouter = require('./routes/usuarios');

app.use('/usuarios', usuariosRouter);

app.listen(3000, () => {
  console.log('Servidor corriendo en el puerto 3000');
});
```

Es decir, el ***Router*** es una pieza de middleware que se encarga de, a partir de una ruta principal, organizar y despachar las subrutas asociadas de forma agrupada bajo un mismo punto de entrada. Esto nos ahorra tener que especificar la ruta raíz en cada definición de endpoint y nos permite modularizar la gestión de rutas de manera más clara y mantenible.

### Servir sitios estáticos a través de Express.js

Los archivos estáticos, son recursos que se entregan al cliente tal como son, sin la necesidad de ser generados, manipulados o procesados por el servidor, estos pueden ser imágenes, CSS, HTML, JavaScript.

Aquí entra entonces el directorio ***public*** y Express.

El directorio public es un estándar donde se suele colocar todos los archivos estáticos que una aplicación web debe servir al cliente.

Express permite acceder a archivos estáticos, pero debe indicarsele el directorio a partir del cual se puede solicitar estos archivos antes de que sea posible.

```
app.use(express.static('public'));
```


### CORS

El CORS o Cross-origin Resource Sharing es una politica del navegador web, que se aplica para prevenir que ciertos dominios accedan a recursos de otro dominio del tipo AJAX (fetch, XMLHttpRequest).

Por lo tanto, para que un dominio acceda a los recursos de otro, debe permitirse desde la configuración del CORS.

Existe un middleware en Express a traves del cual podemos configurarlo de forma sencilla, debe llamarse al inicio del script app.js si queremos que se aplique antes de las rutas (importante)

```
app.use(cors());
```


### Middlewares

hasta ahora venimos mencionando múltiples middlewares, pero ¿que son exactamente?

Podemos definir a un middleware como un puente entre dos partes de la aplicación, por la que fluye la informacion y la transforma antes de entregarla.

entonces, el middleware recibirá informacion de una función de código y la enviara a una función distinta, ya sea otro middleware o la aplicación principal.

```
// definicion de un middleware
function(req,res,next){ … }
// la funcion next() le dice al middleware que pase al siguiente o vuelva a la aplicacion principal y que datos pasarle como argumento, se debe ejecutar si se quiere continuar luego del middleware
```

El middleware se ejecuta en el orden en que se define en el código, y se aplica a todas las solicitudes que llegan a la aplicación, luego de su definición.

Ejemplo:

```
const express = require('express');
const app = express();
function miMiddleware(req,res,next){
console.log('Este es un middleware');
next();
}
app.use(miMiddleware);
```


### Manejo de errores mediante Middlewares

El manejo de errores en express se hace mediante middlewares, es lógico entender que este debe ir al final luego de todas las llamadas a rutas.

```
app.use(function(err, req, res, next) {
console.error(err.stack);
res.status(500).send('Actualmente tenemos inconvenientes con procesar su
solicitud, intente nuevamente mas tarde!');
});
```

Especificamente, errores de codigo sincrono dentro de los controladores de cada ruta, son tratados automaticamente por Express.

Sin embargo, errores devueltos por funciones asincronas dentro de los controladores de ruta y middleware, requieren ser pasados por next() para que Express los detecte, o la aplicacion crasheara.

Lo siguiente puede ser manejado por Express automáticamente, y sera enviado al middleware de manejo de errores.

```
app.get('/testerror', (req, res) => {
throw new Error('probando desencadenar un error');
});
```

Lo siguiente requiere que manejemos nosotros el error y lo pasemos por next()

```
router.get("/api/articulosfamilias-testerror", async function (req, res, next) {
try {
let items = await db.articulosfamilias.findAll({
attributes: ["CampoInexistenteParaGenerarUnError"],
});
res.json(items);
} catch (error) {
next(error);
}
});
```

Luego, el error que pasamos con next(), ira al middleware que se encarga de manejar este error que definimos mas arriba

Explicación: Al usar `await`, estás esperando una _promesa_, y si esa promesa falla, el error **vive en otro contexto** (en el manejador de promesas, no en la pila de ejecución original).  
Por eso Express **no puede atraparlo automáticamente**.  
Por eso es necesario  capturarlo manualmente con `try...catch` y pasarlo al `next(error)`.