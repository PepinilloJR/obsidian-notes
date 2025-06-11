
SQL permite la ***definición, manipulación, control de acceso, compartir e integridad de los datos***.

SQL es interpretado por el DBMS y realiza acciones sobre la base de datos 

![[Pasted image 20250510123854.png]]

SQL es ***No Procedimental***, es decir, el código no se escribe como una secuencia de instrucciones, de hecho, su ejecución no es secuencial en el orden en el que se escribe el código, para ser específicos, el usuario especifica que se debe hacer, no como hacerlo, por lo que también decimos que es Funcional (No Procedimental = Funcional)

### Estándar de SQL

El estándar SQL es un conjunto de definiciones y reglas que normalizan los sistemas de bases de datos, de modo que todo el software funcione de una forma ***que permita la portabilidad*** entre bases de datos de un DBMS a otro

### Mito de la portabilidad:

Al existir estándares de SQL, se creyó que al elaborar una software para la manipulación de la base de datos, utilizando SQL, podría ser utilizado en cualquier sistema de manejo de datos basado en SQL , pero esto no es mas que eso, un mito, ya que las diferencias actuales existentes entre los dialectos SQL de cada vendedor son suficientemente significativos para que una aplicación, al cambiar el sistema de manejo de datos que utiliza, deba modificarse.

Estas diferencias suelen estar englobadas en los siguientes puntos:

* **Códigos de error**
* **Tipos de datos**-> caracteres (long  o fijos), enteros, decimales, floats, datos extendidos, date, datetime, etc
* ***Diccionario de datos***
* ***SQL interactivo***
* ***Interfaz del programa***
* ***SQL dinámico***
* ***Diferencias semánticas*** -> que significa y afecta Null , funciones que se usan, etc
* ***Secuencias de cotejo*** -> **secuencia de cotejo** determina cómo un DBMS **ordena, compara y diferencia cadenas de texto**, considerando idioma, mayúsculas, acentos, y más.
* ***Estructura de la base de datos***

### Sentencias DDL

Empezaremos por definir las sentencias DDL de SQL, estas son un subconjunto de sentencias de SQL que permiten ***modificar la estructura de la BD***, es decir los metadatos 

```

CREATE: crear y definir una tabla, tambien de forma no estandarizada puede crear         la base de datos en si

DROP: eliminar una tabla

ALTER: modificar la definicion de una tabla (nombre, campos, etc)

```

#### Creación de una base de datos en SQL server

```
CREATE DATABASE database_name  // nueva BD unica
[
    ON 
    [ 
    <filespec> [ ,...n ] // archivo principal, .mdf y archivos secundarios .ndf
    ]
    [ , <filegroup> [ ,...n ] ]
]  
[
    LOG ON  // archivo donde se registran las transacciones realizadas (log)
    { 
        <filespec> [ ,...n ] 
    }
]  
[ COLLATE collation_name ]  // conjunto de caracteres soportados, sensibilidad a mayúsculas/minúsculas y ordenamiento

[ PRIMARY ]  
(
    [ NAME = logical_file_name, ]  -- Nombre lógico para SQL Server.
    FILENAME = 'os_file_name'      -- Ruta física en el sistema operativo.
    [ , SIZE = size ]              -- Tamaño inicial del archivo (ej. 10MB).
    [ , MAXSIZE = { max_size | UNLIMITED } ]  -- Tamaño máximo que puede alcanzar.
    [ , FILEGROWTH = growth_increment ]       -- Incremento al crecer (ej. 5MB o 10%).
) [ ,...n ]


// objetos y archivos de la BD pueden almacenarse en el principal o en un grupo definido por usuario de archivos
<filegroup> ::=  
FILEGROUP filegroup_name  
<filespec> [ ,...n ]
```

Aquí dos ejemplos:

**Creación de una base de datos**

```
CREATE DATABASE Prueba_gda
ON
(
    NAME = 'Prueba_dat',                -- Nombre lógico del archivo de datos
    FILENAME = 'C:\prueba.mdf',         -- Ruta física del archivo .mdf
    SIZE = 10,                          -- Tamaño inicial (en MB por defecto)
    MAXSIZE = 50,                       -- Tamaño máximo (en MB)
    FILEGROWTH = 5                      -- Incremento en tamaño (en MB)
)
LOG ON
(
    NAME = 'Prueba_log',                -- Nombre lógico del archivo de log
    FILENAME = 'C:\prueba.ldf',         -- Ruta física del archivo .ldf
    SIZE = 5MB,                         -- Tamaño inicial del log
    MAXSIZE = 25MB,                     -- Tamaño máximo del log
    FILEGROWTH = 5MB                    -- Incremento en tamaño del log
);
```

**Creación de una base de datos de múltiples archivos**

```
CREATE DATABASE Ejemplo
ON PRIMARY
(
    NAME = 'Arch1',                     -- Archivo principal (.mdf)
    FILENAME = 'C:\arch1.mdf',
    SIZE = 10MB,
    MAXSIZE = 20MB,
    FILEGROWTH = 2MB
),
(
    NAME = 'Arch2',                     -- Archivo secundario (.ndf) en el grupo PRIMARY
    FILENAME = 'C:\arch2.ndf',
    SIZE = 10MB,
    MAXSIZE = 20MB,
    FILEGROWTH = 2MB
)
LOG ON
(
    NAME = 'Log1',                      -- Archivo de log (.ldf)
    FILENAME = 'C:\log1.ldf',
    SIZE = 10MB,
    MAXSIZE = 20MB,
    FILEGROWTH = 2MB
);
```

### Creación y definición de una tabla

La definición de una tabla puede resultar grande e intimidarte, pero es solo cuestión de practica y lectura de la documentación, aquí un ejemplo de una tabla cliente, con alguna asociaciones a otras tablas, considerar que las restricciones a los campos y asociaciones se definen desde un ***CONSTRAINT***

```
CREATE TABLE Clientes (
    id INT, -- id
    nombre VARCHAR(50) CONSTRAINT clientes_name_nn NOT NULL, --nombre
    telefono VARCHAR(25), -- telefono
    direccion VARCHAR(400), -- y lo demas ...
    ciudad VARCHAR(30),
    calificacion_credito VARCHAR(9),
    id_vendedor INT, -- id que usaremos para la foreing key
    comentarios VARCHAR(255),

    -- Clave primaria
    CONSTRAINT clientes_id_pk PRIMARY KEY (id),

    -- Clave foránea hacia la tabla EMPLEADOS
    CONSTRAINT clientes_id_fk_vendedor 
        FOREIGN KEY (id_vendedor) REFERENCES EMPLEADOS (id),

    -- Restricción CHECK para valores válidos de calificación de crédito
    CONSTRAINT clientes_credito_ck 
        CHECK (calificacion_credito IN ('EXCELENTE', 'BUENA', 'POBRE'))
);
```

Veamos un ejemplo con CONSTRAINTs mas simples

```
CREATE TABLE Producto (
    id INT,
    nombre VARCHAR(50)
        CONSTRAINT producto_nombre_nn NOT NULL
        CONSTRAINT producto_nombre_un UNIQUE,
    descripc_corta VARCHAR(255),
    texto_largo_id INT,
    imagen_id INT,
    precio_sugerido DECIMAL(11,2),
    unidades VARCHAR(25),

    -- Clave primaria
    CONSTRAINT Id_producto_pk PRIMARY KEY (id)
);
```

veamos un ejemplo con conceptos interesantes
```
CREATE TABLE Item (
    ord_id INT,
    item_id INT,
    id_producto INT,
    precio DECIMAL(11,2),
    cantidad INT,
    cantidad_enviada INT,

    -- Clave primaria compuesta
    CONSTRAINT item_ordid_itemid_pk 
        PRIMARY KEY (ord_id, item_id),

    -- Unicidad: un mismo producto no puede repetirse en una misma orden
    CONSTRAINT item_ordid_prodid_uk 
        UNIQUE (ord_id, id_producto),

    -- Restricción de validación
    CONSTRAINT item_cantidad 
        CHECK (cantidad > 0),

    -- Clave foránea hacia PRODUCTO
    CONSTRAINT item_fk_producto 
        FOREIGN KEY (id_producto) 
        REFERENCES PRODUCTO (id)
        ON UPDATE CASCADE -- regla de compensacion, CASCADE refiere a que todas las tablas asociadas se actualizaran cuando se cambie desde la tabla Item 
        ON DELETE NO ACTION -- regla de compensacion, aqui NO ACTION nos dice que cuando eliminemos una fila, prohibirlo mientras existan tablas asociadas (si fuera cascade, se eliminarian tambien las filas asociadas de la tabla asociada)
);
```

### Eliminación y modificación de una tabla

La eliminación de una tabla es sencilla

```
DROP TABLE articulos -- Eliminara la tabla, y todos los datos contenidos en esta
```

La modificación de una tabla es sencilla, pero implica un amplio apartado de opciones de **que** modificar, por ejemplo:

```
---Cambiar dimensión de columna

ALTER TABLE Articulos
ALTER COLUMN descripcion CHAR(20)

--Agregar columna

ALTER TABLE Alumnos
ADD observacion VARCHAR(20) NULL
```


### Manipulación de tablas - DML

Mediante DML, podemos manipular el contenido de una tabla, es decir:

* ***Insertar*** tuplas nuevas
* ***Eliminar*** tuplas
* ***Modificar*** tuplas

A la hora de manipular el contenido de una BD, el DBMS deberá controlar la integridad y el acceso concurrente de esta.

### Inserción de tuplas

La sintaxis para insertar una nueva fila, es decir, una nueva entidad u objeto a una de las tablas, es la siguiente:

`INSERT INTO tabla [(atrib1,atrib2,...)] VALUES (valor1,valor2,...)`

Por ejemplo: 

```
INSERT INTO EMPLEADOS (Legajo, Apellido, Nombre, Fec_nac, Legajo_jefe)
VALUES (199, 'PÉREZ', 'JOSÉ‘, Convert(date,‘13/03/99', 3))

INSERT INTO EMPLEADOS (Legajo, Apellido, Nombre, Fec_nac)
VALUES (202, ‘ÁLVAREZ', ,‘LILIANA‘, Convert(date,‘17/12/99', 3))
```

Existe la posibilidad de insertar múltiples tuplas de una tabla a otra, siempre que sean compatibles, haciendo uso de SELECT (que veremos a profundidad mas adelante)

```
-- insertar todas las filas de empleados en historico que hayan sido dados de baja antes de 2020

INSERT INTO Historico
SELECT * FROM Empleados
WHERE year(fecha_baja) < 2020
```

Existe adicionalmente la ***posibilidad de una carga masiva de filas desde un archivo externo a la BD***, pero no existe un estándar como tal para esto, y dependerá del producto o software utilizado, generalmente desde una interfaz gráfica propia de este

Debemos notar que en la inserción, el no especificar una de las columnas, se asumirá que se ingresa null en su lugar, lo cual puede ser aceptado o rechazado según las reglas de integridad definidas en la tabla.

### Actualización de tuplas

La sintaxis para actualizar una fila, es decir, modificar una entidad existente, es la siguiente:

```
UPDATE tabla
SET atrib1=valor, atrib2=valor, ...
[WHERE condición]
```

Por ejemplo:

```
-- por ejemplo, usando el WHERE, modifico un empleado especifico 674

UPDATE empleados
SET catego = 'B'
WHERE nroemp = 674

-- a un grupo que cumpla una condicion

UPDATE empleados
SET catego = ‘A‘, fecha_baja=GetDate()
WHERE year(fecha_ing)<=2000 and baja IS NULL

-- es posible modificar todos los empleados al no especificar un WHERE

UPDATE empleados
SET catego = 'B

-- podemos usar subconsultas, que por ahora no se desarrolla mucho, pero que basicamente retorna una tabla, con la que podemos realizar ahora una comparacion, como por ejemplo un IN

UPDATE Cuentas_Cli
SET credito=credito*1.15, fecha_act=GetDate()
WHERE nrocli IN
(SELECT DISTINCT nrocliente
FROM facturas WHERE nrofac>932)

-- cambiar el credito y la fecha de todas las cuentas de clientes cullo nro de factura sea mayor a 932

```


### Eliminación de tuplas

La sintaxis para eliminar una fila, es decir, eliminar una entidad existente, es la siguiente:
(es bastante similar a la modificacion)

```
DELETE FROM tabla
[WHERE condición]
```

Aquí algunos ejemplos: 

```
-- eliminacion de una fila

DELETE FROM empleados
WHERE nroemp = 12058

-- eliminacion de un grupo de tuplas

DELETE FROM empleados
WHERE nroemp IN (12058,10235,13099,11091)

-- eliminacion de todas las filas (no la tabla en si)

DELETE FROM empleados

-- eliminacion con subconsulta

DELETE FROM empleados
WHERE nroemp IN
(SELECT nroemp
FROM historico)
```

### Consideración respecto a integridad

Consideremos a la integridad de los datos como la validez del estado de estos, es decir, lo que nos dice que los datos son correctos para pertenecer a una relación.

Cuando nosotros modificamos los datos de una relación, estamos dando lugar a que se presente una inconsistencia en los datos, es decir, que no sean íntegros. 

En general, cuando realizamos inserción y actualización, estamos afectando la integridad de la misma forma:

-> Restricciones de dominio: Tipo y valor de los datos ingresados
-> Restricciones semánticas: Que el dato no sea null, que sea mayor que, menor que, etc
-> Restricciones de integridad de la entidad y referencial: 
Se exige que toda clave primaria sea única y no asuma nunca un valor Null.
Se exige que para cada valor en una clave foránea, exista un valor en la clave primaria de la otra relación

En el caso de la ***eliminación de una fila, afectamos específicamente a la integridad referencial,*** esto debido a que:

Si la fila es referenciada en mas de una fila de otra relación mediante clave foránea, eliminar esta fila dejara a estas sin integridad referencial

### Insertar, update, delete y la integridad

Cuando realizamos un INSERT, tenemos las siguientes posibles violaciones a la integridad:

1. PK duplicada o null (unicidad e integridad entidad)
2. FK sin concordancia (integridad referencial)
3. valor duplicado cuando fue aplicada la restricción UNIQUE 
4. valor Null con restricción NOT NULL
5. atributo fuera del dominio (tipo y valor)
6. chequeo de validez

Cuando realizamos un UPDATE, se verifican las mismas adicionando:

7. Valor de PK con hijos

Esto quiere decir que al actualizar la clave primaria, si esta es referenciada como clave foránea en otras relaciones (es padre) entonces pueden quedar desactualizadas

Cuando realizamos un DELETE, se afecta la integridad referencial, es decir, que si la fila tiene hijos en su clave primaria, estos quedaran apuntando a una entidad que no existe

Finalmente existen dos restricciones no tan visibles pero que también pueden darse como una violación de integridad por parte de las operaciones insert, update y delete

#### Ciclos referenciales:

Un ***ciclo referencial*** se da cuando dos o mas tablas se referencian entre si, de forma indirecta o directa:

- Tabla A tiene una FK a Tabla B
    
- Tabla B tiene una FK a Tabla C
    
- Tabla C tiene una FK a Tabla A

Esto genera un ciclo de referencias que genera dos problemas a la hora de insertar una tabla, o eliminar una tabla

***En el insert:*** No se puede insertar una fila en A sin que exista una fila en B y viceversa, porque ambas dependen de que exista al menos una de ellas, para no violar las restricciones de FK.

***En el delete:*** No es posible borrar un A porque depende de un B, y no puede borrarse un B porque depende de un A, por lo que queda bloqueado.
Adicionalmente, si esta en modo cascade, esto genera una cascada de errores de integridad referencial, donde se eliminaran gran parte de las filas.

#### Procedimientos comerciales:

la complejidad del software que interactua con la base de datos, implica una duplicación del esfuerzo y el mantenimiento requerido para el sistema.
Esta al cambiar, requiere un cambio tanto en la base de datos como en los programas que las usan