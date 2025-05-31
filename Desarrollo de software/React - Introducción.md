
Para React utilizaremos el entorno de desarollo VITE

```
npm create vite@latest
```


### Detalles técnicos esenciales de react

1. Transpilacion con Babel: React utiliza Babel para transpilar HTML escrito en JSX en JS compatible con el navegador. En Vite se integra Babel junto a otras herramientas para realizar esta, sin necesidad de configuración del usuario
2. Virtual DOM:  El Virtual DOM es una representación virtual en memoria del DOM real, React modifica este Virtual DOM cuando modificamos un elemento HTML, creando una nueva versión de este con cambios en el estado, luego, realiza la comparación con el Virtual DOM anterior y busca las diferencias de estados, luego, aplica al DOM real los cambios mínimos, es decir, aquellos elementos donde hubo un cambio de estado. Esto hace a react mas eficiente 

### Definición de componentes

Los componentes de react son piezas de software que trabajan de forma aislada y general el HTML que sera renderizado, este puede crearse mediante clases o funciones.

### Mediante clases (mas antiguo)

```
import React from 'react';

class MateriaUniversitariaComponent extends React.Component {

	constructor(props) {
	super(props);
	}
	render() {
	const materia = 'Matemáticas'; // Constante local
	return (<div><h1> Materia: {materia} - Cantidad Estudiantes
	{this.props.numeroDeEstudiantes} </h1></div>);
	}
}

export default MateriaUniversitariaComponent;
```


### Mediante Funciones (mas moderno y preferido)

```
export default function MyApp() {
return (
	<div>
	<h1>Welcome to my app</h1>
	<MyButton />
	</div>
);
}
```

Como vemos un componente en React no es mas que una pieza de software ya sea una clase o función, que devuelve HTML. (notar que es obligatorio el uso de mayúscula en la primera letra del componente)


Estos pueden recibir Props desde los componentes padres, adicionalmente, si se trata de una función, debe utilizar Hooks, para que tengan "estado" ya que no son un objeto típico como lo serian si resultaran de una clase.

### Renderizacion

Ya vimos en los detalles técnicos el proceso de renderizacion a la hora de explicar el VirtualDOM.

Para indicar a React que realice esta renderizacion se utiliza ReactDOM.Render()

Por lo general la forma estándar de estructurar esto es:

```
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
	<React.StrictMode>
		<App />
	</React.StrictMode>
);
```

Se define un elemento Root, desde el cual se puede acceder los métodos de renderizacion y que sera un elemento de nuestro index.html


### Utilización de archivos CSS y Estilo en linea

Podemos aplicar estilos a los componentes de dos formas, importando las definiciones de un archivo CSS o utilizando estilo en linea.

Para importar un archivo CSS, hacemos lo siguiente:

```
import './estilos.css';

// resto del componente
....
```

Una vez importado, las definiciones de este archivo se aplicaran al componente y a todos sus hijos.

Adicionalmente, podemos aplicar estilo en linea, de la siguiente forma:

```
function MiComponente() {
return (
	<div style={{ backgroundColor: 'red', color:'white' }}>
			Este es un elemento con estilo en línea
	</div>
);
}
```

(se le pasa un objeto donde cada atributo es una propiedad de CSS expresada en ***camelCase*** y el contenido es un string con las definiciones para esa propiedad)

