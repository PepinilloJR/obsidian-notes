Los triggers son un caso especial de procedimiento almacenado, que se ejecutan automáticamente ante ejecuciones DML.

Esto reduce la complejidad de los programas, los triggers al ser procedimientos almacenados quedan guardados en la BD, y se fuerza su ejecución tras actualizaciones DML.

Sintaxis: 

```
CREATE TRIGGER nombreTrigger
ON tabla

FOR [operacion DML] AS 

BEGIN 
	....
END
```

Ejemplo:

```
CREATE TRIGGER t_AltaEmpleados
ON Empleados

FOR INSERT AS

BEGIN
INSERT INTO Auditoria (Tabla, Operacion, Usuario, Detalle)

SELECT 'Empleados','Alta',SESSION_USER, 'Alta del Empleado‘ +UPPER(Apellido)+', '+Nombre FROM inserted

END
```