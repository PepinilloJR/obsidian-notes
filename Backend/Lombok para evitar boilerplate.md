para evitar la escritura de codigo repetitivo tales como los getters setters, toString(), equals(), denominado boilerplate, utilizamos Lombok

Lombok esta basado en un mecanismo denominado annotation processing, este es un estándar que permite modificar el código durante su compilación

Este intercepta el compilador y modifica el árbol de sintaxis abstracta, el árbol de sintaxis abstracta es la estructura resultante del analizador sintáctico (AST - referir al apunte de  SSL)

Inyectando así el código necesario según las anotaciones utilizadas en la definición de la clase, y que finalmente aparecen en el bytecode

## Instalación de Lombok

Inicialmente debe agregarse la dependencia al `pom.xml`

```
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifactId>lombok</artifactId>
	<version>1.18.30</version>
	<scope>provided</scope>
</dependency>
```

Luego, es requerido modificar el IDE, en el caso de IntelliJ debe instalarse un plugin desde el marketplace, en el caso de ***visual, el soporte para lombok se obtiene desde el Java Extension Pack***

## Principales anotaciones

## @Override

@Override no es propio de lombok pero es importante, y sirve para indicar que una function 

### @Getter y @Setter

```
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class Persona {
	private String nombre;
	private int edad;
}
```

Estos agregan automaticamente los metodos getAtributo y setAtributo
Es posible agregar individualmente a cada atributo estas anotaciones

```
@Getter @Setter
private int edad;
```

### @ToString

```
import lombok.ToString;

@ToString
public class Producto {
	private String nombre;
	private double precio;
}
```

Hace Override al toString() dando el formato "Producto(nombre=" + nombre + ", precio=" + precio + ")"

### @NoArgsConstructor y @AllArgsConstructor

```
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;

@NoArgsConstructor
@AllArgsConstructor

public class Usuario {
	private String email;
	private String clave;
}
```

**@NoArgsConstructor** genera un constructor sin argumentos y **@AllArgsConstructor** un constructor con argumentos

### @EqualsAndHashCode

Genera equals() y hashCode() considerando todos los atributos.

```
import lombok.EqualsAndHashCode;

@EqualsAndHashCode
public class Documento {
	private String tipo;
	private int numero;
}
```

### @Data

```
import lombok.Data;

@Data
public class Alumno {
	private final int legajo;
	@NonNull
	private String nombre;
	private int idCurso;
}
```

@Data incluye todas las configuraciones vistas antes y @RequiredArgsConstructor, es decir, solo incluye atributos marcados como final y @NonNull

## @Builder

Permite generar un patron Builder de la clase sin la necesidad de crear la implementacion nosotros

```
import lombok.Builder;

@Builder
public class Cliente {
	private String nombre;
	private String direccion;
	private int edad;
}
```

Resultando en un builder dentro de la clase

```
Cliente c = Cliente.builder()
.nombre("Juan")
.direccion("Av. Siempreviva 123")
.edad(40)
.build();
```

## @Value

Value declara todos los atributos como ***private final***, la clase como **final** y genera los constructores con todos los campos, los getters y los override de los tipos object tipicos

```
import lombok.Value;
@Value
public class Punto {
	int x;
	int y;
}
```

Tiene similitudes con la palabra reservada ***record***, que aplica una estructura de clase inmutable pensada específicamente para modelar data carriers, y realiza lo mismo que @Value, ya que ambos están especificados para esto, es decir, portar datos a ser usados mas adelante.

![[Pasted image 20250827150434.png]]