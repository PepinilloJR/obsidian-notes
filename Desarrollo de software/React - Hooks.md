
Los hooks nos permiten realizar diferentes acciones relacionadas con el estado de un componente.
Tales como crear un estado o cambiarlo, generar un efecto secundario al cambiar algún estado, crear un contexto de múltiples estados compartidos para un conjunto de componentes.

***useState***: 
Este hook permite a los componentes funcionales de React tener estado. 
Con useState, puedes declarar una variable de estado y una función para actualizarla dentro de un componente funcional.

```
const [count, setCount] = useState(0);
return (
	<div>
	<p>You clicked {count} times</p>
		<button onClick={() => setCount(count + 1)}>
		Click me
		</button>
	</div>
);
```


***useEffect***: 
Este hook permite a los componentes de React realizar efectos secundarios (como llamadas a API) después de la renderización del componente, disparado por el cambio de un estado especificado en el array que se le pasa a la función.

```
useEffect(() => {
	getResumenEmpresa();
}, [empresa]);
```


***useContext***: 
Este hook permite a los componentes de React acceder al contexto definido en un componente superior sin necesidad de pasar props a través de componentes intermedios.

Context esta diseñado para compartir datos globales hacia un árbol de componentes, que en general necesitaran una referencia a ese dato, si el árbol es muy grande, hacerlo a través de props resulta inconveniente.

Primero se crea un contexto, con un valor default que en general lo dejamos como undefined

```
const MyContext = React.createContext(defaultValue);
```

Luego, MyContext tendrá un elemento Provider, que pasara los valores globales al árbol de componentes embebidos en este.

```
<MyContext.Provider value={/* algún valor */}>
	// componentes
</MyContext.Provider> 
```

Luego, finalmente en cada componente, para acceder a los valores utilizamos UseContext(), adicionalmente es posible descomponer el contexto para tomar solo los valores que queremos usar en ese componente.

```
function ThemedButton() {
	const { theme, setTheme } = useContext(ThemeContext); //descompongo y solo tomo theme del contexto, y la funcion para cambiarlo (es un estado)
	
	return <button theme={theme}>Soy un botón con tema {theme}</button>;
}
```

### Renderizacion Condicional

Como solo se realizara una nueva renderizcion de los elementos HTML cuando hay un cambio de estado, todo tipo de renderizacion condicional debe ser realizado con un Estado como condicional

```
// condicional if...

<h1>Hello!</h1>
	{unreadMessages.length > 0 && <h2>You have {unreadMessages.length} unread messages.</h2> }
</div>

// condicional if ... else ...
<div>
	The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
</div>

```

