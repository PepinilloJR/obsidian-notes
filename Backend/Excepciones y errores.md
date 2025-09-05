Java implementa una jerarquia de clases de Throwable, que son objetos que responden a la palabra reservada throw

![[Pasted image 20250827151525.png]]

Los errores presentados como Error son de hardware y estan ahi para notificar al usuario y son manejados por la Java Virtual Machine (JVM)

Los demas son errores en la programacion, es decir Exceptions, y en la mayoria deber ser lidiados con un ***try and catch*** segun sean ***checked*** o ***unchecked***, las unchecked no seran obligatorias de manejar y resultaran solamente en el cierre del programa con un error

***los checked no compilaran a no ser que sean lidiados.***

## Bloque try catch

```
try {
// Sentencias Java
}
catch (IOException entradaerror) {
	// metodo de las excepciones para recibir el mensaje de porque
	System.out.println(entradaerror.getMessage())
}
...

---> posibilidad de catchs sucesivas

catch (Throwable nombreVariable) {
// Sentencias Java
}

finally {
// util para cerrar archivos si se realiza originalmente en el try, evita errores
// Sentencias Java
}
```

## Try with resources

El try with resources es una sobre-escritura del Try donde se le permite pesarle por parámetros recursos (objetos que implementen la interfaz de autocloseable o closeable), es decir, que tengan un método close()

Esto hará que al finalizar el try o entrar en un catch, igualmente estos recursos se cierren.

```
try (BufferedReader br = new BufferedReader(new FileReader(ruta))) {
    String linea;
    while ((linea = br.readLine()) != null) {
        System.out.println(linea);
    }
} catch (IOException e) {
    System.out.println("Error al leer el archivo: " + e.getMessage());
}
```

## Declaración de posibles excepciones

Si un método es capaz de lanzar una ***excepción de tipo checked***, debe especificarse que tipos, mediante: 

```
tipo nombreMetodo( argumentos ) throws IOException, Exception, ... {

}
```

## Provocar excepciones con Throw

mediante ***throw*** es posible causar excepciones para que sean manejadas en una clase superior, generalmente mediante la creación de un Throwable o ***recibiendo uno desde un catch***

```
public static void leerArchivo(String ruta) throws IOException {
        if (ruta == null) {
            throw new IOException("La ruta no puede ser null");
        }
        System.out.println("Leyendo archivo en " + ruta);
    }
```