Las transacciones en SQL son un conjunto de sentencias que se ejecutan como unidad, cumpliendo las propiedades ACID (atomicidad, consistencia, aislamiento, durabilidad)

La sintaxis es la siguiente:

```
BEGIN TRANSACTION nombreTransaccion;

... operaciones

	IF (condicional) 
	BEGIN 
		ROLLBACK; ***cancelar transaccion
	END

END ***solo requerido si se usan condicionales IF

COMMIT ***cargar transaccion a la base de datos


```


Se presentan 2 sentencias nuevas `COMMIT` y `ROLLBACK`

`COMMIT` presenta la finalizacion exitosa de la transacción y crea una versión de la base de datos en el LOG

`ROLLBACK` indica que la transacción no se pudo completar en todas sus sentencias, y vuelve al ultimo commit (se revierte toda la transacción) o a un  SAVEPOINT

un SAVEPOINT es un punto de recuperación parcial en una transacción que definimos usando `SAVEPOINT nombreSavepoint`, y que es posible volver a este con `ROLLBACK TO SAVEPOINT nombreSavepoint` 

```
BEGIN TRANSACTION;


INSERT INTO Compra (Id, ProductoId, Cantidad)
VALUES (1, 101, 5);

-- SAVEPOINT 1: por si algo sale mal con el stock
SAVEPOINT CompraInsertada;

-- Paso 2: Actualizamos el stock
UPDATE Producto
SET Stock = Stock - 5
WHERE Id = 101;

-- Verificamos si el stock se vuelve negativo
IF (SELECT Stock FROM Producto WHERE Id = 101) < 0
BEGIN
  -- Volvemos al SAVEPOINT, eliminamos la compra, no toda la transacción
  ROLLBACK TO SAVEPOINT CompraInsertada;
  DELETE FROM Compra WHERE Id = 1;
  PRINT 'Stock insuficiente. Se revierte solo la compra.';
END

```

Todas las transacciones se registran en el LOG que son imágenes de la base de datos a las cuales es posible volver:

![[Pasted image 20250602152714.png]]

### Transacciones concurrentes y problemas

Las transacciones concurrentes son aquellas que se realizan de forma simultanea por diferentes usuarios.

Esto puede presentar diferentes problemas debido a la condición de carrera que esto implica.

#### 1. Actualización perdida: (commit no a tiempo)

Dos transacciones hacen uso concurrente de los datos con posterior actualización (ambas), una actualización es aplicada, pero no commiteada a tiempo, por lo que la otra transacción trabaja con datos desactualizados, generando inconsistencia, donde la ultima transacción sobrescribe la primera. 

![[Pasted image 20250602153128.png]]

#### 2. Datos no confirmados: (rollback no a tiempo)

Una primera transacción actualiza los datos a tiempo, por lo que la segunda transacción empieza a trabajar con los datos correctos, pero entonces se realiza un ROLLBACK en la primera transacción que la otra transacción no conoce, por lo que trabaja con datos incorrectos o que no son validos, incluso si inicialmente lo parecían.

![[Pasted image 20250602155231.png]]
#### 3. Datos inconsistentes

Consulta de datos con actualización confirmada posterior de otro usuario

![[Pasted image 20250602155318.png]]

#### 4. Inserción fantasma

se realiza una consulta a la BD y posterior inserción que afecta sobre la visión anterior.

Es decir, se realizan dentro de una transacción múltiples consultas iguales y en una de estas aparece un registro nuevo que no esta en las otras, debido a una inserción perteneciente a una transacción de otro usuario


## Soluciones a transacciones concurrentes

1. Prohibir al usuario visualizar actualizaciones sin confirmación por parte de un commit
2. Mantener las transacciones breves, para evitar lo mas posible que sean concurrentes
3. Utilizar commit a menudo
4. ***Utilizar técnicas de cerramiento***

## Cerramiento

El cerramiento consiste en bloquear secciones de la BD (encargado de esto el DBMS) que esta siendo accedida por una transacción, y hacer esperar al resto de transacciones hasta que se presente un COMMIT o ROLLBACK

El nivel del bloqueo puede ser de la ***base de datos, una tabla, pagina, fila o dato***
El ***mas usado a nivel programa*** suele ser el de ***fila***

El tipo de cerramiento puede ser ***compartido***, cuando las transacciones son puramente de consulta, o puede ser ***exclusiva***, donde las transacciones incluyen actualizaciones en la BD

#### Parámetros del cerramiento

Cuando introducimos bloqueos al accedo de datos concurrente, podemos introducir interbloqueos como una problemática extra, por lo que existen parámetros que puede especificar el DBA para evitarlos.

1. Tamaño del cierre (tabla, base de datos, dato, fila)
2. Numero de cierres posibles simultáneos
3. Plazo (timeout) de un cierre (cuanto tiempo puede esperar a un recurso bloqueado la transacción)
4. Escala del cierre (se refiere a como ajustar el tamaño dinámico del bloqueo)

-- es decir, si hay muchos bloqueos pequeños de filas, podría escalarse a un bloqueo mayor de tabla o pagina

En SQL Server se presentan estas definiciones para evitar los interbloqueos

```
-- prioridad sobre cual interboqueo sacrificar
SET DEADLOCK_PRIORITY {NORMAL|HIGH|LOW}

-- tiempo maximo que puede esperar
SET LOCK_TIMEOUT período (período en milisegundos antes que devuelva error)

-- nivel de isolacion de la transaccion con respecto a otras
SET TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SNAPSHOT | SERIALIZABLE}
```

La insolación se presenta de la siguiente forma:

![[Pasted image 20250602161506.png]]

Ejemplo (van antes de la transacción, antes del BEGIN):

```
SET DEADLOCK_PRIORITY LOW; -- Si hay interbloqueo, se mata esta transacción lo mas probable
SET LOCK_TIMEOUT 3000; -- Espera 3 segundos por un bloqueo
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ; -- Nivel de aislamiento

BEGIN TRANSACTION;

    -- Lógica de la transacción
    UPDATE Productos
    SET Stock = Stock - 5
    WHERE ProductoID = 101;

    -- Podemos agregar SAVEPOINTS si se desea
    SAVE TRANSACTION Punto1;

    -- Otro bloque de operaciones
    UPDATE Productos
    SET Precio = Precio * 1.1
    WHERE Categoria = 'Electronica';

    -- Confirmamos los cambios
    COMMIT;
```

