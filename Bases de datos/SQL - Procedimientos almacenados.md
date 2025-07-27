Los procedimientos almacenados son un conjunto de sentencias SQL que se almacenan en la BD, similar a las tablas, triggers y vistas, y que toman parámetros de entrada y pueden devolver valores.

Similar a las vistas, un usuario puede tener permiso de ejecutar una SP, pero no tiene derechos sobre los objetos implicados en el procedimiento.

Sintaxis:

```
CREATE PROC nombre @Variable [tipo] ... 
AS
BEGIN 
 ....
END


EXECUTE nombre VALOR1, VALOR2 ...
```

SQL server

```
CREATE PROCEDURE nombre @Variable [tipo] ... 
AS
BEGIN 
 ....
END


EXECnombre VALOR1, VALOR2 ...
```

Ejemplo:

```
CREATE PROC AgregarPais @Cod varchar(3), @Nom varchar(50)
AS
BEGIN
INSERT INTO Paises (Codigo, Nombre)
VALUES (@Cod, @Nom)
END

********Ejecución

EXECUTE AgregarPais 'AR', 'Argentina'

```




