
El catalogo del sistema define la estructura entera de la BD, son lo que denominamos los ***metadatos***.

Contiene entonce la informacion de todos los objetos DEFINIDOS (no los datos en si) con DDL.

•Tablas
•Columnas
•Restricciones
•Usuarios
•Vistas
•Índices
•Procedimientos
•Triggers
•Privilegios

***Puede verse como una colección de tablas en la BD***, que contienen informacion sobre los objetos de la base de datos

***Es creado y mantenido por el DBMS y provee de valiosa informacion para el DBA***

El acceso al catalogo se realiza a través de consultas y es un acceso de solo lectura para los usuarios.

### Acceso al catalogo en SQL Server

En SQL Server, es posible acceder al catalogo mediante las vistas del catalogo del sistema (System catalog views)

Se acceden mediante el objeto ***sys***, que contendrá vistas del catalogo para el acceso del usuario

Algunas de estas vistas definidas por el DBMS son las siguientes:

```
SELECT * FROM sys.tables;
SELECT * FROM sys.columns;
SELECT * FROM sys.indexes;
SELECT * FROM sys.objects;
SELECT * FROM sys.schemas;
SELECT * FROM sys.triggers;
```

