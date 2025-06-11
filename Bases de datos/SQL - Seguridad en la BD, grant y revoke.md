
La seguridad en las BD es esencial, y el primer responsable es el DBA.

Hay diferentes amenazas hacia una BD, afectando perdida o degradando alguna de las siguientes propiedades de la BD:

1. Integridad (modificaciones inadecuadas)
2. Disponibilidad
3. Confidencialidad (accesos no autorizados, violación a leyes de privacidad, seguridad nacional)

### Medidas de control 

* Control de acceso: Restringir con creación de cuentas y contraseñas diferentes secciones de la BD
* Control de inferencias: evitar acceso a la informacion estadística de la BD, por grupos de edad, ingresos, educación, etc
* Control de flujo: evitar que cierta informacion llegue a usuarios no autorizados
* Cifrado de datos: los datos deben estar cifrados mediante algún algoritmo y con posibilidad de descifrarlos mediante una clave

### Seguridad y DBA

El DBA debe encargarse de las siguientes cuestiones en seguridad de la BD

* Creación de las cuentas para usuarios o grupos de acceso al DBMS
* Concesión de derechos/privilegios a usuarios 
* Retiro de derechos/privilegios
* Asignación del nivel de seguridad a las cuentas
* Auditoria de la BD: el DBA debe implementar el registro de las acciones de los usuarios

### Tipos de control de acceso

***Control de acceso discrecional:***

El mas común en los DBMS, son flexibles pero bastante vulnerables.

Consiste en crear niveles de asignación de privilegios del uso del sistema de BD mediante cuentas, las cuales tendrán mayor o menor permisos para el acceso, creación, modificación y eliminación de tablas/vistas

***Control de acceso obligatorio:***

Es menos común en los DBMS, son rígidos y con una protección alta, poco vulnerables

Consiste en realizar las siguientes acciones:

- Clasificar los datos y los usuarios según a que pertenecen (servicios de inteligencia, militares, gobierno)
- Crear clases de seguridad (altoSecreto - TS, Secreto - S, Confidencial - C, no clasificado - U)
- Quien es sujeto a estos controles es un usuario, cuenta y programa, donde los objetos a limitar son las tablas, filas, columnas, vistas, operaciones

Ningún sujeto puede leer un objeto de nivel mas alto de seguridad que el nivel de autorización que este tiene

Ningún sujeto puede escribir un objeto de clasificación de seguridad menor que el nivel de autorización que este tiene, cada nivel se encarga de su nivel de autorización y nada mas, esto evita filtraciones a un nivel inferior de datos.

***Control de acceso basado en roles:***

El control de acceso basado en roles es similar al discrecional, pero con mayor organización, mas seguro y escalable:

* Se crean roles (administrador, lector, editor, etc) y se le asignan privilegios específicos, y se asocian múltiples usuarios a cada rol
* Es posible que los los roles reciban restricciones temporales
* Es muy típico en modelos para aplicaciones web


### Privacidad

La privacidad de los datos suele ser un desafió importante para la seguridad en las BD

Por lo general, se realizan las siguientes acciones para garantizar una mayor privacidad:

* Evitar almacenar grandes BD en una sola ubicación (single point of failure, SPOF)
* Distorsionar los datos de forma que pierdan la identidad, es decir, datos que identifiquen a la persona a la cual pertenecen los datos, bajo ciertas vistas de la BD


### Concesión de privilegios SQL

Para proveer de privilegios específicos a un grupo de usuarios, se utiliza ***GRANT

Sintaxis: 

```
GRANT [Permisos SELECT, INSERT, ETC]
ON [Usuarios]
TO [tabla/vista/basedatos]

***es posible para algunas acciones como update, ***el especificar los campos de la vista o tabla

GRANT UPDATE (direccion, telefono)
ON alumnos
TO departamentos
```

Ejemplos: 

```
GRANT SELECT
ON alumnos
TO PUBLIC

GRANT SELECT, INSERT, UPDATE
ON alumnos
TO depsistemas

GRANT ALL PRIVILEGES
ON alumnos
TO dep_alumnos

GRANT UPDATE (direccion, telefono)
ON alumnos
TO departamentos

GRANT SELECT
ON promedios
TO nancy
WITH GRANT OPTION 

**permite transmitir privilegios
```


### Revocación de privilegios SQL

Para revocar de privilegios específicos a un grupo de usuarios, se utiliza ***REVOKE

Sintaxis: 

```
REVOKE [Permisos SELECT, INSERT, ETC]
ON [Usuarios]
FROM [tabla/vista/basedatos]

***es posible para algunas acciones como update, ***el especificar los campos de la vista o tabla

GRANT UPDATE (direccion, telefono)
ON alumnos
FROM departamentos
```

Ejemplos:

```
REVOKE SELECT
ON alumnos
FROM nancy CASCADE / RESTRICT

REVOKE INSERT, UPDATE
ON alumnos
FROM usuariox


REVOKE ALL PRIVILEGES
ON promedios
FROM depalumnos, ...
```