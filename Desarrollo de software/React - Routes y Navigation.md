Instalación de React Router

```
npm install react-router-dom
```

react-router-dom permite crear enrutamiento por lado del cliente, es decir, en lugar de hacer una peticion al servidor al actualizar la ruta, este actualiza la URL sin request al servidor, renderizando el componente asociada a la URL en la interfaz del usuario.

Por lo tanto, las ventajas se dan al evitar solicitar nuevos documentos, y ***permite aplicar animaciones entre cambios de rutas***

Uso típico de BrowserRouter:

```
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Acerca from "./components/Acerca";
import Inicio from "./components/Inicio";
import Detalle from "./components/Detalle";
import Error404 from "./components/Error404";
function App() {
	return (
	<div>
		<BrowserRouter>
		<Routes>
			<Route path="/" element={<Inicio />} index/>
			<Route path="/acerca" element={<Acerca />} />
			<Route path="/detalle/:id" element={<Detalle />} />
			<Route path="*" element={<Error404 />} />
		</Routes>
	</BrowserRouter>
	</div>
);
}
export default App
```

Primero, BrowserRouter es el componente padre que provee la capacidad a los hijos a incluir funcionalidades de react-router.
Luego, Routes contiene cada ruta definida, a la cual le pasamos una URL relativa y el componente a renderizar al posicionarse en esa ruta.

### ¿Que es BrowserRouter?

El componente BrowserRouter maneja la navegación SPA provista por React Router, y lo hace mediante History API, que es una API provista por el navegador que permite manipular el historial de la pagina, y entre otras cosas, permite modificar el contenido de la URL sin recargar la pagina o intentar acceder a un recurso del servidor.

Entonces, al modificar la URL, BrowserRouter actualiza el historial y renderiza un componente, sin realizar una solicitud HTTP al contenido (excepto si se recarga la pagina manualmente, dando ahí un problema en servidores que sirven archivos estáticos sin lógica de ruteo, como github pages)

Para ello existe como alternativa ***HashRouter***, que en lugar de tomar la URL toma el Hash de la URL que nunca se envía en peticiones HTTP

### Rutas anidadas

Las rutas anidadas permiten crear subrutas, y se hace simplemente anidando una ruta dentro de otra, o un conjunto de rutas dentro de una.

```
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Admin from "./pages/Admin";
import Usuarios from "./pages/Usuarios";
import Configuracion from "./pages/Configuracion";

function App() {
return (
<BrowserRouter>
<Routes>
	<Route path="/admin" element={<Admin />}>
		<Route path="usuarios" element={<Usuarios />} />
		<Route path="configuracion" element={<Configuracion />} />
	</Route>
</Routes>
</BrowserRouter>
);
}
export default App;
```

Aquí, la ruta que da Usuarios, es `/admin/usuarios`


### useParams y useNavigate

Para rutas `/ruta/:parametro` es posible acceder a estas en el componente mediante ***useParams***

```
import { useParams } from "react-router-dom";
export default function Detalle() {
const { id } = useParams();

return (
	<div>
		<h2>Detalle del elemento</h2>
		<p>ID: {id}</p>
	</div>
);
}
```

Para poder navegar de forma automatizada, sin necesidad de un Link o NavLink, se hace uso de ***useNavigate***  

Primero se crea un Navigate, que tomara una dirección y cambiara la URL actual a esta

```
import { useNavigate } from "react-router-dom";
const navigate = useNavigate();
```

Luego, podriamos aplicarlo por ejemplo en un useEffect, que cuando queramos que cierto estado cambie a cierto valor, se redirija la pagina a otro componente

```
useEffect(() => {
	if (contactos) {
		navigate("contactos")
	}
}, [contactos])
```

### Componente Navigate y useLocation

Navigate es un componente que permite navegar de forma programada en una aplicacion react, alternativamente a useNavigate.

```
function App() {

const [redirectToAcerca, setRedirectToAcerca] = useState(false);

const handleClick = () => {
	setRedirectToAcerca(true);
};

return (

<div>
	{redirectToAcerca && <Navigate to="/acerca" />}
	<button onClick={handleClick}>Ir a Acerca</button>
	<Routes>
		<Route path="/" element={<Inicio />} />
		<Route path="/acerca" element={<Acerca />} />
	</Routes>
</div>
);
}
```

En este ejemplo, cuando redirectToAcerca es true, se renderiza el componente Navigate con una URL en la propiedad **to**, haciendo que se redirija a esta pagina

Luego, si queremos saber en que ubicación estamos, es decir la ruta, los parámetros pasados en la ruta y demás, podemos usar useLocation().

Por ejemplo, si se quiere redirigir al usuario a la pagina desde donde vino o una pagina por defecto luego de un logueo, podemos hacer uso de useLocation()

```
import { useLocation, useNavigate } from "react-router-dom";
import { useEffect } from "react";

export default function Login() {
  const location = useLocation();
  const navigate = useNavigate();
  const from = location.state?.from?.pathname || "/";

  const handleLogin = () => {
    // Suponemos login exitoso
    navigate(from, { replace: true });
  };

  return (
    <div>
      <h2>Iniciar sesión</h2>
      <button onClick={handleLogin}>Ingresar</button>
    </div>
  );
}

```

Luego, el componente desde donde se redirije seria este:

```
function RutaProtegida({ children, usuario }) {
	const location = useLocation();
	if (!usuario) {
		return <Navigate to="/login" state={{ from: location }} replace />;
	}
	return children;
}
```

Lo que esta sucediendo aquí es, cuando nosotros usamos Navigate o useNavigate, es posible pasar un objeto ***state*** que contendrá diferentes propiedades opcionales las cuales podemos usar a modo de referencia, y que se accederán desde el objeto devuelto por useLocation()

En este caso, si usuario no esta definido en la ruta protegida, se usa Navigate para redirigir a `/login` y ademas se le pasa mediante ***state*** la URL original desde donde se le redirijio.

El componente Login toma estos datos y lo usara para navegar de vuelta a la RutaProtegida una vez se realizo correctamente el Login