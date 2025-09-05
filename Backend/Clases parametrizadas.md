Para evitar el uso de parametros tipo Object y tener que realizar casting para obtener cada tipo de objeto, lo que seria poco conveniente.

para esto se utilizan los Generics, esto se denomina parametrizacion de clases con un tipo especifico:

```
// sin parametrizacion 
ArrayList a = new ArrayList();

// con parametrizacion
ArrayList<String> b = new ArrayList<>()
```

Es posible utilizar los Generics en la declaracion de clases, esto es util para realizar metodos que sean independientes del tipo de objeto que se utiliza

```
public class ListaArrayMejorada<E> {
	
	public void insertarElemento(E elemento ) {
	...
	}
	
}
```

Se debe indicar E como parametro entre `<>`, luego para referenciar a este tipo en los metodos, se declaran de tipo E, siendo este reemplazado por el que el programador decida al instanciar un objeto de la clase

Puede limitarse la generalidad de E mediante la extension de clases, por ejemplo, si nuestra clase requiere de comparaciones en sus elementos, es necesario que el tipo E sea comparable, es decir, extienda de Comparable

```
public class ListaArray<E extends Comparable> {
...
}
```

Si quisieramos que solo fueran numeros, se extenderia desde Number, y asi.


