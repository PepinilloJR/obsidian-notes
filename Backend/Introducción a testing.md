
Para el testing automatizado, es util la utilización de la librería ***JUnit***

Especificamente para la realizacion de pruebas unitarias (revisar DDS)

Se compone de tres componentes:

1) JUnit Platfrom: Es la plataforma que permite el descubrimiento y la ejecucion de las pruebas unitarias en JUnit y requiere integracion con un IDE, por lo general los mas conocidos lo tienen
2) JUnit Jupiter: Es el modelo de programacion que permite escribir los tests, provee las anotaciones y extensiones para esto
3) JUnit Vintage: Es la capa de compatibilidad para poder seguir usando tests escritos para JUnit4 en JUnit5

Para importar JUnit se debe instalar la dependencia 

```
<dependencies>
	<dependency>
		<groupId>org.junit.jupiter</groupId>
		<artifactId>junit-jupiter</artifactId>
		<version>5.10.0</version>
		<scope>test</scope>
	</dependency>
</dependencies>
```

## Creación y ubicación de tests

Por convención, dentro de un proyecto de Maven, se guardan los tests dentro de `src/test`

Los test unitarios no son mas que un método anotado ***@Test***

```
// clase contenedora de los tests
public class EjemploTest {

	@Test
	public void testSuma() {
		int suma = suma(1, 3);
		Assertions.assertEquals(4, suma);
	}
}
```

Adicionalmente, existen multiples anotaciones provistas por JUnit

![[Pasted image 20250827170833.png]]

## Comprobaciones 

Dentro de los tests, se debe verificar que se ejecute algo, que no haya excepciones, y que los resultados provistos por la ejecución sean los esperados, esto ultimo se realiza con la clase ***Assertions***, que introduce multiples métodos estáticos para ello.

![[Pasted image 20250827171130.png]]

es posible importar los métodos estáticos para no necesitar utilizar la clase directamente.

```
import static org.junit.jupiter.api.Assertions.assertTrue;
```

## Ejecución de los Tests

Para ejecutar los tests, uno de los métodos es utilizando Maven, 
mediante el llamado a `mvn test`.

Adicionalmente, los métodos `deploy`, `package` y `install`.

