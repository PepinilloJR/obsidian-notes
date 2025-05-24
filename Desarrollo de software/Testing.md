
El testing de software es el proceso de evaluar y verificar que el software haga lo que se supone tiene que hacer, mediante un conjunto de pruebas para prevenir errores, reducir los costos y mejorar el rendimiento

### Tipos de pruebas:

El testing puede dividirse en dos tipos fundamentales, de las cuales derivan otros tipos, ***pruebas funcionales*** y ***no funcionales*** 

### Pruebas funcionales

El objetivo de las pruebas funcionales es determinar si cada característica de la aplicación funciona en base a los requisitos del proyecto.

#### Pruebas unitarias: 

Se evalúa individualmente un componente básico de software, para poder garantizar que cada pieza de código pequeña capaz de ser probada funcione

Este tipo de prueba básico y esencial es una técnica de prueba de caja blanca,
esto es, centrada en los detalles procedimentales del software, es decir, altamente ligado al código fuente, esto implica que estas pruebas están basadas en la implementacion concreta de la funcionalidad a probar, por ello, cambiar esta implica cambiar la prueba.

Finalmente, el objetivo de la prueba es, garantizar que el código que funciona siga funcionando ante cambios.
La prueba consiste en escoger distintos valores de entrada, analizar el flujo de ejecución del programa (que funcione) y confirmar la validez de los resultados para lo que se supone que debe hacer el componente.

#### Pruebas de integración:

Si se integran dos o mas componentes, las pruebas de integración se realizan considerando a estos como un solo cluster, y se verifica que interactúan como se esperaba.

#### Pruebas de interfaz:

Incluida en la prueba de integración, específicamente para probar si el intercambio de datos entre componentes es correcto

#### Pruebas del sistema:

reúne todos los módulos de la aplicación, prueba todo el sistema como una unidad, verificando que cumpla los requisitos.

#### Pruebas de regresión:

Consiste en repetir pruebas de escenarios que habían funcionado antes, ante un cambio en componentes del software.

#### Pruebas de humo 
son pruebas que verifican la funcionalidad básica de una aplicación y su objetivo es
asegurar que las características más importantes del sistema funcionen como se espera.

#### Pruebas de saneamiento

suele ser un subconjunto de las pruebas de regresión; se realiza cada vez que se
lanza una nueva versión de una aplicación estable. Solo después de ejecutar el conjunto de pruebas de saneamiento, la compilación pasa al siguiente nivel de prueba. 

***La diferencia entre humo y saneamiento*** es que las pruebas de humo se realizan en una aplicación inicialmente inestable, mientras que saneamiento se realiza
en una aplicación estable.

#### Las pruebas de aceptación

son pruebas formales, ejecutadas para verificar si un sistema satisface sus
requerimientos de negocio, requieren que el software se encuentre en funcionamiento, y se centran en replicar el comportamiento de los usuarios, a fin de rechazar cambios si no se cumplen los objetivos.


### Pruebas No funcionales

El objetivo de las pruebas No funcionales es comprobar el rendimiento de la aplicación:

#### Pruebas de rendimiento

Se mide el rendimiento de la aplicación en condiciones reales, se miden:
tiempo de respuesta, escalabilidad, estabilidad, eficiencia en el uso de recursos, etc.

#### Pruebas de estrés:

Se mide el rendimiento en condiciones anormales o extremas:
Algunas de las preguntas que responde esta prueba son: ¿Cómo funciona el sistema en condiciones de estrés? Si falla, ¿es posible recuperarlo y reiniciarlo?

#### Pruebas de capacidad:

Se mide el rendimiento del sistema cuando maneja una base de datos de gran cantidad de datos:
¿La base de datos es capaz de almacenar y procesar grandes cantidades de datos? ¿Habrá algún problema debido a un gran volumen de datos?

#### Pruebas de carga:

Se mide el rendimiento de la aplicación bajo la carga esperada como en el mundo real. 

Se simulan todas las cargas posibles en la aplicación y se comprueba el rendimiento. Por ejemplo, en un día normal, se espera que 1000 usuarios visiten un sitio web

#### Pruebas de seguridad:

Se mide la seguridad de la aplicación, desde diferentes puntos de vista (red, datos, sistema, aplicación), garantizando la fiabilidad y confianza en la aplicación para usarlo con datos sensibles

#### Pruebas de escalabilidad:

Se mida la capacidad de ser escalable la aplicación, es decir, que su arquitectura y diseño funcione al agregar mas hardware implicado en su ejecución (mas servidores, mas usuarios, mas equipos, etc).

#### Pruebas de usabilidad:

También de accesibilidad, se refiere desde la perspectiva del usuario, de si es posible utilizar esta aplicación eficientemente

#### Pruebas de mantenibilidad:

Se mide la capacidad de la aplicación de adaptarse a cambios y actualizaciones en su código o el sistema en el que se ejecuta.

Pruebas de compatibilidad:

Se verifica que la aplicación funcione con respecto diferentes SOs, navegadores, hardware, velocidades de red, etc.


## Pruebas manuales y de automatización

Las pruebas que se realizan, pueden ser de ***carácter manual*** (la llevan a cabo personas reales, siguiendo unos pasos concretos) o pueden ser de ***carácter automatizado***, mediante el uso de herramientas y scripts que ejecutan los casos de prueba de forma automática una vez se ejecutan las pruebas.


### Desarrollo guiado por pruebas o Test-driven development (TDD)

La idea principal del TDD es que los requisitos sean traducidos a pruebas, de este modo, las pruebas que son pasadas, garantizan que el software cumple los requisitos

Se escribe una prueba, y se verifica que falle, luego, se implementa el código que hace que la prueba pase satisfactoriamente, y se refactoriza el código.

![[Pasted image 20250516152150.png]]

## JEST 

Jest es el framework de pruebas unitarias más popular en JavaScript, pruebas se ejecutan en paralelo, por lo que se ejecutan mucho más rápido que otros marcos de prueba. Esta adaptado para integrarse bastante bien con React y otros frameworks front-end.

#### Instalación recomendada:

***npm install  --save-dev -g jest***

#### Configuración de package.json

```
"scripts": {
"test": "jest --testTimeout=10000 --force-exit",
"test:jsmodule" : "node --experimental-vm-modules jest --verbose --silent --
detectOpenHandles"
// si no funciona 
"test:jsmodule": "node --experimental-vm-modules ./node_modules/.bin/jest --testTimeout=10000 --force-exit",
},
"devDependencies": {
"jest": "~29.7.0"
}

```

# Importación correcta de jest y supertest si se usa Modules

```
import { jest } from '@jest/globals';

import request from "supertest";
```


### Ejemplo de test básico

```
import sumar from "./index.js";
test("Esto suma dos n�meros", () => {
	const a = 1;
	const b = 1;
	const expected = a + b;
	const res = sumar(a, b);
	expect(res).toBe(expected);
})
```

Por partes

test -> funcion para plantear una prueba, toma un nombre o descripcion, y una funcion callback que se ejecutara al invocarse el test

expect -> funcion de afirmacion, toma un valor y devuelve un objeto que tiene una serie de funciones de afirmacion, esto es condiciones que verifican una especifica expectativa sobre un valor, y son usadas para validar la sallida de una funcion que esta siendo testeada

toBe() -> una de las funciones de afirmacion disponibles en el retorno del expect, verifica que el valor pasado al expect sea igual al valor pasado al toBe()


## SuperTest

SuperTest es una biblioteca de Node.js que ayuda a los desarrolladores a probar las API http. Extiende otra biblioteca llamada superagent, un cliente HTTP de JavaScript para Node.js y el navegador. 

SuperTest puede ser utilizado junto con marcos de prueba como Jest, especificamente, nos ayudara a consumir los endpoints HTTP, y Jest construira los test, la evaluacion de resultados, y la ejecucion automatizada de estos.

```
npm install supertest --save-dev
```


Especificamente con Express, requerimos tener el App:

```
import request from "supertest"
import app from "app.js"

test("Servidor inicia", async () => {
	await request(app)
			.get("/api")
			.expect(200)
			.expect(async (res) => {
				// el siguiente expect es de jest
				expect(res.text).toBe("servidor iniciado")
			})
})


```

Aquí, ***request*** sera el punto de partida, toma una aplicación express o una instancia de un server http (si usamos http puro), y devuelve un objeto con funciones preparadas para simular peticiones http, que son provistas por supertest (no son las mismas de jest, aunque tengan mismo nombre)

.get() -> una de las posibles peticiones simuladas, devolverá una promesa y finalmente un response (podemos usar await de ser necesario)

finalmente, podemos usar funciones de afirmación provistas por SuperTest, para verificar el estado, o para analizar el response mediante un calllback pasado al expect (única razón de existir de un callback en el expect)

Aquí algunas funciones de afirmación posibles

```

//comprobar status
.expect(200)


//comprobar header
.expect('Content-Type', /json/)
.expect('X-Custom-Header', 'value')

//comprobar body (text, json, etc)

.expect('servidor iniciado')

o

.expect({ mensaje: 'ok' }) 

// callback para validaciones personalizadas al response
.expect((res) => {
  expect(res.status).toBe(200) // estas ya son validaciones de JEST
  expect(res.text).toContain('OK')
})


```


### Describe() y it()

Describe es una función cuya utilidad es la de agrupar varios test con algún criterio, de forma que se ejecutan en conjunto.

it() es lo mismo que test(), puede considerarse un alias que garantiza la compatibilidad con otras librerías que no tienen definido test()

```
describe("pruebas para API usuarios", () => {
	it ("test 1", async() => {
		await request(app).get("/api/usuarios") ....
	})

	it("test post", async () => {
	  const nuevoUsuario = {
	    nombre: "Juan",
	    email: "juan@example.com"
	  };

	  await request(app)
	    .post("/api/usuarios")
	    .send(nuevoUsuario)             // enviar body JSON
	    .set("Accept", "application/json") // setear header
	    // esperar ahora respuesta del servidor sobre el post
	    .expect(201)                    
	    .expect((res) => {
	      expect(res.body.nombre).toBe("Juan");
	      expect(res.body.email).toBe("juan@example.com");
	      expect(res.body.id).toBeDefined();
	    });
}) 
```
### Mock 

Un Mock en Jest es un objeto que imita la interfaz y salida de una función, clase, o modulo, o en general cualquier elemento de software que defina un comportamiento.

```
// una funcion mock puede obtenerse de 

const mockFunction = jest.fn() -> retornara una funcion mock

```

Usando esta mock function, podemos reemplazar la funcionalidad de ciertas funciones, para que entreguen un resultado especifico, un error especifico, un response especifico o un reject especifico, para poder testear el componente que hace uso de esta función (dependencia), pero que no queremos específicamente testear esta función particular, si no el componente que la usa

-> nos estamos preguntando, si las dependencias del componente funcionan como deberían, funciona correctamente el componente?

### Mockeo de modulos

Es posible hacer un Mock de todas las funciones exportadas de un modulo, realizando `jest.mock("direccion del modulo")`

##### El mockeo de modulos no es posible si se usa Modules en lugar de CommonJS

Entonces, todas las funciones que importemos del modulo serán Mock y podremos acceder a los métodos del objeto Mock para cambiar su comportamiento

Si queremos, podemos usar ***jest.spyOn***, que permite realizar un Mock de una de las funciones de un modulo, dejando al resto con su funcionalidad intacta 

```
// importo todas las funciones dentro de un objeto nav
import * as nav from "./navigation";

// cambio el comportamiento del 
const mockCalculateRoute = jest.spyOn(nav, "calculateRoute");
// spyOn devuelve una referencia a la funcion mockeada, pero puede usarse directamente la funcion desde nav, ya que estara mockeada tambien
```

### EJEMPLO de testing de routes

tenemos una ruta de barrios, estos utilizan el servicio de barrios para la obtención de los datos, y ante una solicitud HTTP, deben devolver estos datos.

nosotros queremos exclusivamente testear las rutas, que devuelvan los valores correctos, por ello debemos considerar un Mock de los servicios de barrio

el código es el siguiente: 

```
import BarriosService from "../../services/barrios.service";
import barriosRouter from "../../routes/barrios.routes.js";
import {jest} from "@jest/globals";
import {describe, it, expect, afterEach } from "@jest/globals";
import express from "express";
import request from "supertest";

// hacemos el mock de los servicios de barrio

jest.mock("../../services/barrios.service.js");


// el testeo esta fuera de la aplicacion principal, debemos configurar un //express() con las rutas para el testeo

const app = express();
app.use(express.json());
app.use("/barrios", barriosRouter)


// definimos un test, uno para la obtencion de los valores desde la ruta, 
// y otra para el manejo de un error interno, si tiene un error interno, 
// deberia poder manejarlo y indicar el error desde la ruta

describe("Barrios Routes", () => {
	// limpia la informacion de los Mocks ( conteos de veces llamados, argumentos llamados, retornos almacenados, etc)
    afterEach(() => {
        jest.clearAllMocks();
    });
    
    describe("GET /barrios", () => {
        it("should return all barrios ordered", async () => {

			// configuramos el mock para el servicio obtenerTodosOrdenados, que              // es usado en la ruta
			
            const mockResponse = [{ idBarrio: 1, nombre: "Barrio Test" }];
            BarriosService.obtenerTodosOrdenados.mockResolvedValue(mockResponse);


            // cuando llamamos a la ruta, considerando que sabemos que el                    // resultado del servicio sera el valor mockeado mockResponse
	        // entonces la ruta deberia indicar un status 200 y dar por                      // resultado el mockResponse
            const response = await request(app).get("/barrios");
            expect(response.status).toBe(200);
            expect(response.body).toEqual(mockResponse);
        });

        // para el caso de que hubiera un error en el servicio, el route                 //deberia ser capaz de retornar una respuesta que lo indique
        
        it("should handle server errors", async () => {
            BarriosService.obtenerTodosOrdenados.mockRejectedValue(new Error("Test error"));
            const response = await request(app).get("/barrios");
            expect(response.status).toBe(500);
            expect(response.body).toEqual({ error: "Error interno del servidor" });
        });
    });

```

Luego, si quisiéramos testear los servicios en si, tendríamos que realizar un Mock de los repositorios involucrados, esto se puede hacer utilizando la misma lógica presentada en el código anterior.