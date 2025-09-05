Una clase anonima son aquellas que se implementan para un objetivo unico de una sola vez, mediante la declaracion de clases Inline

Dada una interfaz o una clase abstracta, podemos crear una clase que extienda a esta de la siguiente forma:

```
interface Procesador {
void procesar();
}
```

Entonces se puede plantear:

```
Procesador miProcesador = new Procesador() {
@Override
public void procesar() {
System.out.println("Procesamiento en clase anónima.");
}
};
```

