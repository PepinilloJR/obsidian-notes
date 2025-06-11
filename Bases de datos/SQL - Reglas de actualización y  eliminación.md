
Las reglas de actualización y eliminación son aquellas que se especifican al crear un relación de tablas hijo (foreign keys), y que indican que hacer con la relación cuando se modifica el padre (se actualiza su PK o es eliminada)

Estas se especifican con la sintaxis `ON DELETE` Y `ON UPDATE` en la creación de la tabla hijo.

Las reglas de actualización y eliminación tienen las siguientes posibilidades:

RESTRICT - NO ACTION -> Impide que se elimine/actualice la PK del padre mientras exista referencias a este

CASCADE -> En caso de actualizar, se actualizan las claves foráneas de todos los hijos, en caso de eliminar, se eliminan los hijos asociados al padre

SET NULL -> Da valor Null a las claves foráneas cuando cambia o se elimina el padre

SET DEFAULT -> SI se especifico antes, coloca en las claves foráneas el valor default al cambiar el padre o eliminarse este.

Ejemplo:

```
CREATE TABLE Empleado
( 
 Dni INT NOT NULL,
 DniJefe INT NOT NULL DEFAULT 1,

 CONSTRAINT PK PRIMARY KEY(Dni),

 CONSTRAINT JEFE FOREIGN KEY (DniJefe) REFERENCES EMPLEADO(Dni)
	 ON DELETE SET DEFAULT
	 ON UPDATE CASCADE

);
```



