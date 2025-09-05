Un package en java no es otra cosa que una carpeta o directorio que agrupa un conjunto de clases, y que su nombre identifica su ruta fisica, y que logicamente es declarada en los .java para indicar su pertenencia 

Los packages de Java estan almacenados en el `classpath`

```
package com.paquete;

public class MiClase { }
```

Un nombre completo de un package se forma escribiendo el nombre de todas las carpetas desde la carpeta origen (el classpath)

por ejemplo, si el classpath es `src` y el package se ubica en `src/com/paquete/miclase.java`, entonces el nombre del paquete sera `com.paquete`

Recordemos, en **Java**, el _classpath_ es la ruta (o conjunto de rutas) donde la JVM y el compilador buscan las clases y paquetes, tanto para su compilacion como ejecucion en JRE, objetivamente los archivos `.class`. pero en proceso de compilacion usamos los `.java` que se organizan de la misma forma

## Empaquetado y distribucion

todos los paquetes y clases por defecto de java se encuentran en rt.jar, un archivo que contiene todas las .class nativas de java, y en general se encuentre en el directorio del JDK

aqui su contenido:

![[Pasted image 20250905122924.png]]

Nosotros podemos realizar lo mismo, utilizando `jar.exe` contenido en JDK, es posible crear un paquete comprimido `.jar` que contiene todos nuestros paquetes con su bytecode en las `.class`.

Se le debe pasar por argumento una carpeta o directorio con un proyecto compliado de Java.

Adicionalmente, si alguna de las clases contenidas en el empaquetado posee un `main()` entonces sera posible ejecutar el `.jar` desde la JRE (mediante el comando java o abriendo simplemente el .jar)

### Estructura del .jar

Un .jar contendra dos carpetas, `META-INF` que contendra MANIFEST.MF, archivo de texto con indicaciones para JVM, caso de ser ejecutable contendra la linea `Main-Class:` y luego una carpeta (dependiendo de como estructuramos nuestro proyecto) con todas las `.class` dentro de sus correspondientes paquetes