Maven es una herramienta para la construccion de proyectos de JAVA.

Hay 3 formas de entender a Maven:

#### Como Generador de estructuras de proyecto:
Maven utiliza archetypes, plantillas predefinidas para las estructuras basicas de un proyecto, incluyendo el pom.xml

#### Como administrador de dependencias:
Administra las dependencias automaticamente, declarandolas en el pom.xml

Los conceptos clave aqui son: 

**Dependency Management:** Declaración de dependencias en el archivo pom.xml utilizando la etiqueta `<dependency>`. Maven descargará y gestionará estas dependencias automáticamente.

**Repositorios**: Maven utiliza repositorios remotos (como Maven Central) para buscar y descargar dependencias. También puedes configurar repositorios locales o privados.

**Transitive Dependencies**: Maven resuelve automáticamente las dependencias transitivas, es decir, las bibliotecas que son necesarias debido a otras dependencias.

#### Como orquestador del ciclo de vida del proyecto:
Coordina el ciclo completo de la construccion del proyecto, desde la compilacion hasta la ejecucion de pruebas y la generacion de archivos binarios JAR o WAR que intervienen en el desarrollo

### Ciclo de construccion de un proyecto en Maven:

* Clean: Borra todo lo generado por procesos anteriores
* Validate: verifica que el proyecto este correcto y sus dependencias
* Compile: compila el codigo fuente del proyecto
* Test: ejecuta todas las pruebas automatizadas
* Package: empaqueta el codigo compilado en un JAR o WAR
* Install: instala paquete generado en un repositorio local para su uso en otros proyectos
* Deploy: copia el paquete generado en un repositorio remoto para compartirlo

![[Pasted image 20250813171548.png]]

### Estructura basica del proyecto Maven y pom.xml

![[Pasted image 20250813171643.png]]

Donde `src/main/java` contiene el codigo fuente principal, `src/main/resources` contiene archivos de configuracion, propiedades, para la app por parte del desarrollador

luego, `src/test/java` contiene pruebas unitarias y de integracion y `src/test/resources` contiene recursos utilizados en estas


#### El archivo pom.xml

`pom.xml` tiene la siguiente estructura basica:

```
<project>
    <modelVersion>4.0.0</modelVersion>

    <groupId>ar.edu.utnfc.backend</groupId>
    <artifactId>hola-mundo</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <name>Hola Mundo Maven</name>
    <description>A simple Maven project</description>

    <!-- Especificación de la versión de Java -->
    <properties>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- Dependencias del proyecto -->
    </dependencies>

    <build>
        <plugins>
            <!-- Plugin de compilación -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                </configuration>
            </plugin>
            <!-- Otros plugins de construcción -->
        </plugins>
    </build>
</project>
```

`<modelVersion>` tiene la version de Maven utilizado
`<groupId>` tiene el grupo del proyecto
`<artifactId>` es el nombre del proyecto
`<version>` es la version del artefacto, es decir de la ultima compilacion que es el archivo ejecutable del proyecto
`<properties>` se definen propiedades del proyecto como la version de java usada en `<maven.compiler.source>21</maven.compiler.source>` y `<maven.compiler.target>21</maven.compiler.target>`

En `<build>` se encuentran configuraciones para la construccion del proyecto,  una de estas son los plugins que son componentes de software que se pueden agregar para la construccion del proyecto
y tienen una estructura similar a la descripta 

```
 <plugin>
    <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
        <configuration>
            <source>${maven.compiler.source}</source>
            <target>${maven.compiler.target}</target>
        </configuration>
</plugin>
```

Muchos de los plugins corresponden a los comandos que usaremos en maven, tales como `package` o `compile`

### Directorio .m2

El directorio .m2 se encuentra en la carpeta del usuario sea linux o windows y contiene un archivo `settings.xml` donde se configura maven 


# Creacion de un proyecto

Para la creacion de un proyecto se debe utilizar un **archetype**, en general el mas basico es `maven-archetype-quickstart`

El comando para crear el proyecto es:

```
mvn archetype:generate "-DgroupId=ar.edu.utnfc.backend" `
"-DartifactId=hola-mundo" `
"-DarchetypeGroupId=org.apache.maven.archetypes" `
"-DarchetypeArtifactId=maven-archetype-quickstart" `
"-DinteractiveMode=false"
```

Donde se deben definir varias de las propiedades del proyecto

### Compilacion, empaquetacion y ejecucion

Todo lo generado aqui se encuentra en `./target`

para utilizar el plugin de compilacion es `mvn compile`
para utilizar el plugin de empaquetacion es `mvn package` donde se genera el paquete `.JAR` para ser compilado y ejecutado en java

para que funcione este ultimo se requiere instalar el plugin 

```
<plugin>
<artifactId>maven-jar-plugin</artifactId>
<version>3.0.2</version>
<configuration>
<archive>
<manifest>
<mainClass>ar.edu.utnfc.backend.App</mainClass>
</manifest>
</archive>
</configuration>
</plugin>
```

Dentro de este paquete puede encontrarse el MANIFEST.MF que indica la clase Main del programa

Alternativamente, la forma rapida de ejecutar mediante maven es usando `mvn exec:java` que es un plugin que permite ejecutar Java bajo configuraciones tales como la clase que contiene el metodo Main

para utilizarlo debe instalarse el plugin:

```
<plugin>

<groupId>org.codehaus.mojo</groupId>

<artifactId>exec-maven-plugin</artifactId>

<version>3.1.0</version>

<configuration>

<mainClass>ar.edu.utnfc.backend.App</mainClass>

</configuration>

</plugin>
```

es conveniente porque utiliza el classpath de maven, el classpath en java es el que utiliza para buscar clases y dependencias al compilar y ejecutar un proyecto en Java


### Repositorios de dependencias de Maven

* ***Repositorio Central***: El repositorio central es un repositorio de dependencias mantenido por la comunidad Maven. Es el repositorio predeterminado desde el cual Maven descarga dependencias
* ***Repositorio Remoto***: Un repositorio remoto es un servidor que almacena dependencias y artefactos de proyectos. Puedes configurar Maven para usar repositorios remotos adicionales además del repositorio central
* ***Repositorio Local o Cache:*** El repositorio local es un directorio en tu sistema donde Maven almacena las dependencias descargadas. Por defecto, este repositorio está ubicado en la carpeta .m2

### Uber Jar

Uno de los plugins importantes que vamos a usar es Uber Jar, que genera un paquete .jar con todas las dependencias incluidas en el proyecto, package solo incluye las clases definidas en el proyectos disponilbes en el classpath

```
<plugin>
<artifactId>maven-assembly-plugin</artifactId>
<version>3.3.0</version>
<configuration>
<archive>
<manifest>
<mainClass>ar.edu.utnfc.backend.App</mainClass>
</manifest>
</archive>
<descriptorRefs>
<descriptorRef>jar-with-dependencies</descriptorRef>
</descriptorRefs>
</configuration>
<executions>
<execution>
<id>make-assembly</id>
<phase>package</phase>
<goals>
<goal>single</goal>
</goals>
</execution>
</executions>
</plugin>
```

Donde el plugin puede ejecutarse como: 

`mvn assembly:single` 

### Deploy y repositorios remotos

finalmente, para compartir paquetes mediante maven, se realiza un deploy. Para ello debe configurarse un repositorio remoto accesible por Maven. Algunos son: 

![[Pasted image 20250814235004.png]]

Para ello, se configura en pom.xml en `<distributionManagement>` y se deben configurar credenciales en `~/.m2/settings.xml` y tener el servidor del repositorio remoto ejecutandose

finalmente, se ejecuta `mvn deploy`