
Axios es una librería de JavaScript utilizada para facilitar las peticiones HTTP, ya que ahorra múltiples pasos generalmente necesarios si se usa fetch()

```
npm install axios
```

Axios ofrece en general las siguientes ventajas:

1. ***Facilidad de uso:*** Axios ofrece una API sencilla y fácil de usar para realizar solicitudes HTTP, lo que facilita su integración con proyectos de React.

2. ***Manejo de errores***: Axios permite un manejo de errores más estructurado y fácil de entender, ya que utiliza promesas y captura errores de forma más explícita.

3. ***Interceptores***: Axios permite interceptar solicitudes y respuestas antes de que sean manejadas por el código que las consume. Esto es útil para agregar funcionalidades adicionales, como la autenticación, el registro de errores, la modificación de encabezados, etc.

4. ***Cancelación de solicitudes***: Axios proporciona una forma sencilla de cancelar solicitudes en curso, lo que puede ser útil en ciertos casos, como cuando el usuario navega fuera de una página antes de que se complete una solicitud.

5. ***Transformación de datos***: Axios permite transformar los datos enviados y recibidos en una solicitud, lo que puede ser útil para la serialización y deserialización de datos, así como para la manipulación de datos antes de su procesamiento.

6. ***Configuración global***: Axios permite establecer configuraciones globales para todas las solicitudes realizadas, lo que facilita la configuración de elementos comunes, como encabezados, tiempos de espera, etc.

Axios provee métodos HTTP por defecto 

```
axios.get(url, config])

axios.post(url, data, config])

axios.put(url, data, config])

axios.delete(url, config])
```

***data*** es la informacion del body, ***config*** es un objeto para modificar otras opciones 

```
let config = {
  headers: {
    header1: value,
  }
}
```


### Ejemplo de un get


```
import axios from 'axios';
const API_BASE_URL = 'https://dds.frc.utn.edu.ar/api'; //URL de ejemplo

async function BuscarTodos() {

	try {
		const response = await axios.get(`${API_BASE_URL}/articulos`);
		return response.data;
	} catch (error) {
		console.error('Error al obtener los datos:', error);
		throw error;
	}
};


export const articulosService = {
	BuscarTodos
};
```

Puede notarse que no es requerido ningún tipo de parseo de los datos, el propio get retorna la informacion ya parseada.

Aquí en el post, puede pasarse el objeto simplemente como un parámetro, sin necesidad de serializarlo.

```
import axios from 'axios';

const API_BASE_URL = 'https://tudominio.com/api'; // Cambia esto por tu URL real

async function Grabar(item) {
  if (item.IdArticulo === 0) {
    await axios.post(`${API_BASE_URL}/articulos`, item);
  } else {
    await axios.put(`${API_BASE_URL}/articulos/${item.IdArticulo}`, item);
  }
}

// Supongo que tienes otra función llamada Buscar
async function Buscar() {
  const response = await axios.get(`${API_BASE_URL}/articulos`);
  return response.data;
}

export const articulosService = {
  Buscar,
  Grabar
};
```

Ejemplo de la baja, usando DELETE 

```
async function Baja(item) {
  await axios.delete(`${API_BASE_URL}/articulos/${item.IdArticulo}`);
}

export const articulosService = {
  Baja
};
```

En general puede notarse que es una sintaxis muy cercana al backend, usando express, por lo que puede ser bastante intuitivo y rápido.

### Interceptores

Los interceptores son funciones que se ejecutan antes o después de una solicitud, permitiendo modificarlas antes de que salgan o lleguen a la aplicación.

Se definen como un callback que pueden tomar el data y config de las request, y el body de una response

```
axios.interceptors.request.use(
	(data, config) => {...}
)

axios.interceptors.response.use(
	(response) => {...}
)
```

Algunas formas de utilizarlos son las siguientes:
#### Manejo de errores global

```
axios.interceptors.response.use(
	(response) => {
	// Si la respuesta es exitosa, simplemente devuélvela
		return response;
	},
	(error) => {
	// Si hay un error, muéstralo en una notificación
	console.error("Se produjo un error:", error.message);
	// Rechazar la promesa con el error para que pueda ser manejado en la
	aplicación
	return Promise.reject(error);
}
);
```

#### Agregar headers

Podemos agregar un header que se aplique a todas las solicitudes que enviemos al API, por ejemplo el token de autorización

```
axios.interceptors.request.use((config) => {
	// Agregar el token de autenticación al encabezado 'Authorization'
	config.headers.Authorization = `Bearer ${localStorage.getItem("authToken")}`;
	return config;
});
```

#### Monitorear el progreso de una solicitud

Esto no es un ejemplo de interceptores, pero demuestra una utilidad de Axios, que es monitorear el progreso de una solicitud

```
const config = {
	onUploadProgress: (progressEvent) => {
	const percentCompleted = Math.round((progressEvent.loaded * 100) /
	progressEvent.total);
	console.log(`Progreso de carga: ${percentCompleted}%`);
	},
};

axios.post("/upload", data, config);
```