Notas teóricas sobre javascript, no tanto su sintaxis excepto bajo situación que lo requiera para explicar un concepto

### Tipado dinámico

Javascript es de tipado dinámico, por lo tanto no debemos declarar los tipos de los datos, estos son inferidos por el interprete. A su vez es util saber que es case-sensitive y se considera buena practica utilizar camelCase 

### Comparaciones booleanas 

algo extraño de JS, es que usamos triple igual para las comparaciones booleanas, denominado comparación estricta

```
=== # igual
!== # no igual
```

esto es ya que, el uso típico de == y != aplican una conversión implícita de los tipos (coerción) antes de realizar una comparación

por ejemplo:

```
'3' == 3 // true → convierte string a número
false == 0 // true → false se convierte a 0
'' == 0 // true → ambos se consideran "falsy"
null == undefined // true → única excepción explícita
```

### Conversión y coerción de tipos

Es posible en JS utilizar el mecanismo de coercion de tipos, que realiza una conversión de tipos de forma implícita, esto permite que algunas operaciones que no deberían ser posibles y resultar en NaN, si sean posibles.

```
const a = "3";
const b = 9;
console.log(a + b);
```

Es posible la conversión explicita, usando métodos de parseo o expresiones del tipo Tipo(valor de otro tipo) 


### Expresión de función invocada inmediatamente (IIFE)

Es posible definir una función que se ejecuta inmediatamente en la propia definición, esto nos ahorra la linea para invocarla:

```
(function () {
var number = Math.random() * 10;
console.log(number >= 5);
})();
```

### Objetos en JS

Lo primero que debe aclararse de JS es que no es un lenguaje orientado a objetos, es mas, es un **lenguaje de objetos basado en prototipos.**
Esto es, todos los objetos heredan de otros objetos, y no de clases como tal, con ultima instancia heredando del objeto **Object {}**

![[Pasted image 20250410190608.png]]

un objeto puede crearse usando diferentes metodos

#### Notación literal

creación de un objeto utilizando llaves {}, es decir, definimos un objeto especificando los métodos y propiedades con sus valores ya instanciados.

```
const juan = {
nombre: 'Juan',
correo: 'juan@gmail.com',
profesion: 'Comerciante',
fechaNacimiento: new Date(1994, 9, 1),
saludar: function () {
console.log('Hola soy ' + this.nombre);
}
};
```

aquí aparece la palabra **this**, en el contexto de notación literal, **this** hace referencia al objeto mismo que estamos creando

en una función común, **this** haría referencia a **Window**

#### New Object()

podemos crear un objeto con el operador New, seguido luego de una "clase" (que ya veremos que no son como tal clases)

particularmente, podemos usar Object en esta operacion para crear una instancia de un objeto especificando todos los métodos y propiedades con sus valores ya instanciados, pero podemos usarlo en otros contexto por ejemplo, para pasar objetos vacíos en funciones de algún tipo especifico, por ejemplo new Date()

```
const persona = new Object();
persona.nombre = 'Juan';
persona.correo = 'juan@gmail.com';
persona.profesion = 'Comerciante';
persona.saludar = function(){
console.log('Hola soy ' + this.nombre)
};
```

#### Función constructora

Las funciones constructoras nos permiten definir "Clases"
que no son mas que funciones que crean un objeto al ser llamadas mediante el operador new

y en ese sentido es un constructor:

```
function Persona(nombre, correo, profesion, fechaNacimiento) {
this.nombre = nombre;
this.correo = correo;
this.profesion = profesion;
this.fechaNacimiento = fechaNacimiento;
this.saludar = function () {
console.log('Hola soy ' + this.nombre);
};
this.edad = function () {
const hoy = new Date();
return hoy.getFullYear() - this.fechaNacimiento.getFullYear();
};
}

# instanciar

const persona = new Persona('Juan', 'juan@gmail,com', 'Comerciante');


```

#### Uso de prototype

prototype nos permite afectara todas las instancias de Object, desde una misma instancia o desde el constructor

```
# para el constructor
Persona.prototype.esMayorDeEdad = function () {
// if (this.edad() > 18)
// return true;
// return false
return this.edad() > 18;
};


# para la instancia, usamos __proto__ en lugar de prototype

carla.__proto__.esMayorDeEdad = function () {
return false;
};
```

#### Class

desde ES6, javascript admite class, y permite asi definir un tipo de objeto con una sintaxis mas tipica de un lenguaje POO

Esta sintaxis implicitamente define objetos por medio de funciones constructoras, ya que el concepto de clases no existe en JS y la creación de objetos se termina haciendo a través de prototype. Lo que realmente nos facilita ES6 es una sintaxis amigable conocida como syntactic sugar o lenguaje más dulce para los programadores.

```
class Persona {
constructor(nombre, correo, profesion, fechaNacimiento) {
this.nombre = nombre;
this.correo = correo;
this.profesion = profesion;
this.fechaNacimiento = fechaNacimiento;
}
saludar() {
console.log('Hola soy ' + this.nombre);
}
edad() {
const hoy = new Date();
return hoy.getFullYear() - this.fechaNacimiento.getFullYear();
}
}
```

y luego, se instancia de forma similar a las funciones constructoras.

### Cadena de prototipos

todos los objetos de JS heredan de Object, y se da debido a la cadena de prototipos.

es decir:

![[Pasted image 20250410204227.png]]
![[Pasted image 20250410204233.png]]


### Arrow functions y el uso de this

las arrow functions no tienen su propio this como sucedía al usar las funciones constructoras, si no que lo heredan del contexto en el que están definidas

por ejemplo:

![[Pasted image 20250410204453.png]]

en una función tradicional, el resultado es distinto

![[Pasted image 20250410204517.png]]