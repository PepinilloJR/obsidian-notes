
### Sinónimos

Los ***sinónimos*** son acotaciones para acceder a una tabla, base de datos u cualquier otro objeto.

`CREATE SYNONYM EJ FOR EJEMPLO` -> los datos de EJEMPLO serán accesibles desde EJ

Es posible eliminar el sinónimo con `DROP SYNONYM EJ`


### Creación de indices

Ya vimos el concepto de indices en el teórico, en este caso es esencial saber como definir uno en SQL Server

Como toda operacion DDL de creación, se realiza utilizando `CREATE`

Sintaxis:

```
CREATE [UNIQUE] [CLUSTERED | NONCLUSTERED] INDEX index_name
ON table_or_view (column1 [ASC | DESC], column2 [ASC | DESC], ...)
[WITH (index_option1, index_option2, ...)]
[ON filegroup]

```

`CREATE INDEX nombreIndice` acepta entre medio las opciones ***UNIQUE*** (cada valor/combinación de valores en las columnas del indice serán únicos) y ***CLUSTERED*** o ***NONCLUSTERED***

***CLUSTERED*** -> el orden lógico del indice especificado determinara el orden físico del archivo de los datos, solo puede existir uno por tabla

***NONCLUSTERED*** -> el indice esta separado de los datos, con punteros hacia las filas, desordenadas

`ON tabla|vista (column1 [ASC | DESC] ...)`

Esto indica en que tabla o vista se basara el indice, y se especifican en los paréntesis las columnas de la tabla/vista que componen el indice y su ordenamiento ascendente o descendente 

Casi terminando, opcionalmente se puede agregar `WITH (index_option1 ...)` que permite establecer algunas ***opciones de configuración del indice***. Las opciones mas comunes son `PAD_INDEX = ON | OFF` (permitir al indice dejar espacio entre los niveles intermedios para las inserciones rápidas)
y `FILLFACTOR = valorPorcentaje`  para indicar cuanto llenar cada pagina, dejando el resto libre para el ***PAD_INDEX***

Finalmente, puede ***especificarse un grupo de archivos al que pertenence el indice***, por defecto es PRIMARY
`ON [FILEGROUP]`

Por ejemplo:

```
CREATE UNIQUE NONCLUSTERED INDEX IX_Clientes_Email
ON Clientes (Email ASC)
WITH (FILLFACTOR = 90, PAD_INDEX = ON)
ON [PRIMARY];
```


### Vistas

Las vistas son consultas almacenadas, que pueden entenderse como una tabla virtual (no tiene datos realles) que es un subconjunto de una tabla real o fuente.

La vista posee un nombre y es almacenada en el diccionario de datos de la BD

Las vistas son ***importantes en seguridad,*** ya que brinda la capacidad de mostrar distintas perspectivas y permisos a usuarios distintos.

Crea un ***aislamiento ante cambios en la estructura de la BD,*** ya que la vista se mantendrá igual siempre y cuando la consulta contenida sea posible

***Sin embargo***, son ***costosas*** ya que el DBMS debe resolver la consulta cada vez que es accedida y presenta ***restricciones*** a la hora de actualizarlas

Hay distintos tipos con distintas formas de crear Vistas

***Vista de los alumnos de sistemas*** (Horizontal)

```
****vista horizontal

CREATE VIEW sistemas AS
SELECT *
FROM alumnos
WHERE especialidad=’K’

WITH CHECK OPTION

```

`WITH CHECK OPTION` indica que a la hora de insertar una fila desde la vista, debe cumplir la lógica de la Vista

Sin esto, se puede insertar, pero el nuevo valor no se encontrara en la Vista, lo que trae problemas y confusión.

Esto es porque la vista no tiene datos, son los datos de la tabla alumnos, la operacion de inserción implica grabar en alumnos, y recuperar estos nuevos datos desde la Vista

***Vista parcial de los datos de los clientes*** (vertical)

```
CREATE VIEW clientes AS
SELECT nroclie, nombre, apellido
FROM maesclientes
```

***Vista parcial de los datos de los clientes de Capital*** (fila/columna)

```
CREATE VIEW clientes AS
SELECT nroclie, nombre, apellido
FROM maesclientes
WHERE codpos=5000
```

***Vista de los promedios por legajo de los exámenes*** (vista agrupada)

```
CREATE VIEW promedios (legajo, promedio) AS
SELECT nroleg, AVG(nota)
FROM examenes
GROUP BY nroleg
```

***Vista de los alumnos junto a su ciudad*** (vista compuesta)

```
CREATE VIEW procedencia (apellido, nombres, localidad) AS
SELECT a.apellido, a.nombres, c.ciudad
FROM alumnos a, ciudades c
WHERE a.id_ciudad = c.id_ciudad
```

### Restricciones a la actualización de Vistas

solo es posible operar con INSERT, UPDATE, DELETE cuando se cumplen las siguientes condiciones en la consulta que forma la Vista

- **Sin `DISTINCT`**  
    No se puede usar `DISTINCT` en la vista, ya que elimina duplicados y hace que no se pueda identificar exactamente qué fila de la tabla base se está modificando.
    
- **Sin subconsultas**  
    No se permiten subconsultas (`SELECT` dentro del `SELECT`) en la definición de la vista. Esto complica el mapeo directo a las filas de la tabla base.
    
- **Sin `GROUP BY` o `HAVING`**  
    Las vistas que agrupan datos **no son actualizables**, porque ya no representan una única fila real de la tabla base, sino un resumen.
    
- **Columnas simples, sin funciones o expresiones**  
    Ejemplo de lo que **no se permite**:
	    ``SELECT nombre || ' ' || apellido AS nombre_completo``
* **Tabla única y con derechos** 
    La vista no debe ser compuesta y el usuario debe tener permisos suficientes para operar sobre esta

### Consideraciones en la eliminación de una Vista

Cuando se quiere eliminar una Vista, tenemos 2 opciones sobre como proceder si esta Vista es referenciada en otras Vistas, restricciones, transacciones, etc.

```
DROP VIEW nombre_vista CASCADE | RESTRICT;
```

Aquí se distingue: 

**CASCADE**: elimina la vista **y todo lo que dependa de ella** (otras vistas, restricciones, etc.)

**RESTRICT**: elimina la vista **solo si no tiene dependencias**; si algo depende de ella, se lanza un error.