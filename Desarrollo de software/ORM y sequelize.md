Un ORM (object relational mapping) es una técnica que nos permite crear un puente entre las entidades del modelo de negocios de nuestro programa (modelos o clases que representan los objetos que usamos en el sistema) y el esquema lógico de la base de datos relación. De este modo, manejamos con objetos y funciones del lenguaje, en lugar de hacerlo usando SQL.

esto nos provee algunas ventajas:

![[Pasted image 20250422181145.png]]

Un pool de conexiones es un conjunto limitado de conexiones a la base de datos, que se mantienen abiertas durante un tiempo limitado, y que son atribuidas a diferentes hilos de ejecución de transacciones con la base de datos, por parte del sistema de bases de datos, de modo que cuando finaliza la ejecución, se usa esta misma conexión para otro hilo que ejecuta una transacción paralela diferente.
De este modo, es posible realizar múltiples transacciones con la base de datos, sin tener que abrir y cerrar una conexión cada vez que se realiza una nueva transacción.

Las migraciones de bases de datos, son el proceso de transferir los datos, metadatos y esquemas de una base de datos a otra, ya sea desde el mismo equipo o entre computadoras diferentes, en general todos los ORM traen consigo herramientas para poder realizar migraciones de bases de datos de forma sencilla

Sin embargo, usar un ORM trae algunas complicaciones 

![[Pasted image 20250422183545.png]]
### Sequelize

Sequelize es un ORM de Typescript y Node.js basado en promesas, que se puede utilizar para múltiples sistemas de bases de datos.

Da soporte para realizar transacciones a la base de datos, crear y manejar relaciones, implementar carga diferida (solo cargar en memoria un objeto si se va a utilizar) y precarga (cargar en memoria todos los objetos para tener un rápido acceso a este), y ademas permite crear la replicacion de lectura, esto es, tener múltiples servidores con replicas de la base de datos para consultas de lectura, mientras que un servidor principal se encarga de la escritura y propagación de estos datos a las demás replicas

En general, se encuentran las siguientes caracteristicas, entre las mencionadas antes:

* Basado en promesas, por lo que los accesos a la base de datos se ejecutan en un hilo secundario provisto por libuv
* Permite definir modelos que se mapean a relaciones
* Da soporte para validaciones de los datos en la aplicación, definir restricciones de los campos en la base de datos, y la creación de asociaciones del tipo one-to-one, one-to-many, many-to-many
* Permite ejecutar SQL puro si se requiere

### Instalación y estructura de carpetas 

```
npm install sqlite3
npm install sequelize
```

Se recomienda la siguiente estructura
![[Pasted image 20250422185448.png]]
### Conexión con la base de datos

especificamente para SQLite, podemos conectarnos a una base de datos en memoria (no se aplican cambios ni se guarda en disco), una base de datos en disco, o una base de datos remota.

Notar que para SQLite esto ultimo no es posible como tal, SQLite es una base de datos embedida, esto es, que toda la base de datos se guarda como un archivo, y no tiene implementado mecanismos de cliente-servidor para realizar una base de datos distribuida.

En nuestro JS dedicado a la configuración de la base de datos, usamos lo siguiente:


Conexión a base de datos en disco:

```
import { Sequelize } from 'sequelize';

const sequelize = new Sequelize({
	dialect: 'sqlite',
	storage: './datos/db.sqlite',
});

export default sequelize; // exportar la conexion creada
						  // para ser usada en la aplicacion
```

Conexión a base de datos en memoria:

```
import { Sequelize } from 'sequelize';

const sequelize = new Sequelize("sqlite::memory:");
```

Podemos ver que entonces sequelize refiere a una instancia de Sequelize, que representa una conexión especifica a una base de datos.

Para cerrar la conexión y liberar los recursos que esta utiliza, se debe utilizar el método ***.close()*** en la conexión, para crear otra sera requerido volver a crear una instancia de sequelize con los métodos anteriores explicados, no puede volverse a conectar una vez cerrada la conexión

#### Testeo de conexión. 

Para comprobar que una conexión a la base de datos es correcta, se puede utilizar el método .authenticate() provisto por la conexión creada anterior.

```
try {  
await sequelize.authenticate(); 
console.log('Connection has been established successfully.');
} catch (error) {  
console.error('Unable to connect to the database:', error);
}

```


### Modelos

Un modelo es una abstracción que representara una tabla en la base de datos, para ello, se utiliza la clase Model, de la cual nuestro modelo sera una clase extendida o subclase.

El modelo definirá de la tabla su nombre, las columnas (atributos) y los tipos de datos de cada columna

los tipos de datos son provistos por Sequelize y deben usarse estos a la hora de definir el modelo, importando la clase ***DataTypes***.

![[Pasted image 20250422191700.png]]
Tambien pueden especificarse restricciones (nivel base de datos) y validaciones (nivel ORM) a los campos de los modelos, que sirven para mantener la consistencia de los datos y para asegurar que se cumplen las reglas de negocio establecidas.

Ejemplos de restricciones:
```
usuario: {
type: DataTypes.TEXT,
unique: true // valor unico
},

usuario: {
type: DataTypes.TEXT,
allowNull: false, // valor nulo 
},


```
Ejemplos de validaciones:
```
genero: {
type: DataTypes.TEXT,

// definir los posibles valores que debe tomar el campo
isIn: { 
args: [['F', 'M']],
msg: "Debe ser (F)emenino o (M)asculino"
}
},

email: {
type: DataTypes.STRING,

// Validación de formato de correo 
validate: {
isEmail: true electrónico
}
},

usuario: {
type: DataTypes.STRING,

// Validación: longitud mínima y máxima
validate: {
len: [4, 20], 
is: /^[a-zA-Z0-9_]+$/ // Validación: solo caracteres alfanuméricos y guiones
bajos
}
}


```

### Definicion de un modelo

para definir un modelo, existen dos formas.

Llamando al metodo ***sequelize.define(nombreModelo, campos, options)*** 

Creando una clase modelo que extienda de Model y llamando a su metodo  ***modelo.init(campos, options)***

Una vez que el modelo es definido, es posible acceder a este desde ***sequelize.models***

>Los campos están contenidos en un objeto raiz, donde cada campo sera un objeto, y tendrá de atributos el ***type,  y las restricciones***

***Ejemplo con sequelize.define()***

```
const User = sequelize.define(
  'User',
  {
    // Model attributes are defined here
    firstName: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    lastName: {
      type: DataTypes.STRING,
      // allowNull defaults to true
    },
  },
  {
    // Other model options go here
  }
);

```

***Ejemplo con extension de clase y metodo .init***

```
class User extends Model {}

User.init(
  {
    id: {
      type: DataTypes.INTEGER,
      autoIncrement: true,
      primaryKey: true,
    },
    nombre: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    apellido: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    usuario: {
      type: DataTypes.STRING,
      unique: true,
      allowNull: false,
    },
    fecha_alta: {
      type: DataTypes.DATE,
      defaultValue: DataTypes.NOW,
    },

  },
  // options, ejemplo
  {
    sequelize,
    modelName: 'Usuario',   // Nombre del modelo en la app
    tableName: 'usuarios',  // Nombre de la tabla en la base de datos
    timestamps: false,      // No agrega createdAt ni updatedAt
  }
);

// asociacion con foreingKey (notar que se hace afuera del init)

// Un user pertenece a un perfil
Usuario.belongsTo(Perfil, {
	as: 'perfil', // nombre que toma el atributo que contiene el objeto referido
	foreignKey: 'perfilId', // nombre del atributo que contiene el valor de la
	clave foránea
	field: 'perfil_id' // nombre del atributo en la base de datos que contiene la
	clave foránea

// Definición de la relación uno a muchos entre Perfil y Usuario
// Un perfil puede tener muchos usuarios
Perfil.hasMany(Usuario, {
	foreignKey: 'perfilId',
	field: 'perfil_id'
});


```

### Creación de tablas a partir de los modelos e insertar filas

Una vez definidos los modelos, dependiendo de si estamos conectados a una base de datos nueva o una con tablas ya creadas.

podemos utilizar el método .sync(), desde la conexión misma o de desde uno de los modelos si solo queremos sincronizar la tabla especifica de ese modelo.

este método lo que hará sera crear las tablas en base a los modelos de no existir estas en la base de datos.

```
// crear la tabla/s si no existe

await sequelize.sync();

await Usuario.sync(); 
```

A partir de un modelo, podemos insertar una fila con ***.create()***

```
const nuevo = await Usuario.create({ nombre: 'Ana', apellido: 'García', usuario:
'ana2025' });
```

Podemos utilizar métodos Bulk, Bulk mejora el rendimiento para la inserción de múltiples filas, por lo que si queremos insertar un array de objetos, debemos usar ***.bulkCreate()***.

Es útil poder deshabilitar el logging en una operacion Bulk, debido a que saldrán muchísimas operaciones de la base de datos.

`disableDbLog();` desactivar logging
`enableDbLog();` activar logging


```
await Usuario.bulkCreate([
{ nombre: 'Ana', apellido: 'García', usuario: 'ana2025', email:
'ana@mail.com', genero: 'F' },
{ nombre: 'Luis', apellido: 'Martínez', usuario: 'lmartinez', email:
'luis@mail.com', genero: 'M' },
{ nombre: 'Sofía', apellido: 'López', usuario: 'sofial', email:
'sofia@mail.com', genero: 'F' },
{ nombre: 'Julián', apellido: 'Ruiz', usuario: 'jruiz', email:
'julian@mail.com', genero: 'M' },
{ nombre: 'Marta', apellido: 'Fernández', usuario: 'mfer', email:
'marta@mail.com', genero: 'F' },
{ nombre: 'Carlos', apellido: 'Domínguez', usuario: 'cdom', email:
'carlos@mail.com', genero: 'M' },
{ nombre: 'Lucía', apellido: 'Silva', usuario: 'lucias', email:
'lucia@mail.com', genero: 'F' },
{ nombre: 'Pedro', apellido: 'Sosa', usuario: 'psosa', email:
'pedro@mail.com', genero: 'M' },
{ nombre: 'Valentina', apellido: 'Paz', usuario: 'vpaz', email:
'valentina@mail.com', genero: 'F' },
{ nombre: 'Andrés', apellido: 'Cruz', usuario: 'acruz', email:
'andres@mail.com', genero: 'M' }
]);
```

En general, nuestro Modelo sera el punto de acceso a la relación que representa, por lo que a partir de este, podremos realizar selecciones, eliminar filas, contarlas, y realizar otro tipo de acciones, sin requerir por nuestra parte realizar sentencias SQL.

Esto es lo que se denomina ***Acciones ORM*** y aquí hay algunos ejemplos:

```
const usuarios = await Usuario.findAll();

const usuario = await Usuario.findOne({ where: { id: 5 } });

const usuario = await Usuario.findByPk(5);

// notar que para la busqueda, podemos agregar la opcion include, para que el resultado traiga tambien los objetos asociadios por clave foranea

const usuario = await Usuario.findByPk(5,{
include: [{
	model: Perfil,
	as: 'perfil', // Alias definido en el modelo Usuario
	required: true, // Solo incluir usuarios con perfil asociado
}]
});

const usuarios = await Usuario.findAll({
	where: {
	[Op.and]: [
	{ apellido: { [Op.like]: '%ez' } },
	{ [Op.not]: { id: [1, 2, 3] } },
	]
},
	order: [['apellido', 'ASC']],
	include: [{
	model: Perfil,
	as: 'perfil', // Alias definido en el modelo Usuario
	required: true, // Solo incluir usuarios con perfil asociado
}]
});



const usuarios = await Usuario.findAll({
where: {
apellido: { [Op.like]: '%ez'} // para traer todos los apellidos terminados en
'ez'
}
});

const usuarios = await Usuario.findAll({
where: {
[Op.and]: [
{ apellido: { [Op.like]: '%ez' } },
{ [Op.not]: { id: [1, 2, 3] } }
]
}
});


```
(% indica cualquier cadena previa, algo así como el "\*")

Como vemos aparece el parámetro [Op], en este podemos definir que operador de búsqueda se esta utilizando, para realizar búsquedas mas complejas acordes a las posibilidades que ofrece SQL.

![[Pasted image 20250423082715.png]]
![[Pasted image 20250423082722.png]]

Uno muy importante es la ***paginación de resultados*** que consiste en solicitar solo una porción de ***n*** resultados en una búsqueda
```
const pagina = 2;
const tamanoPagina = 10;
const usuarios = await Usuario.findAll({
offset: (pagina - 1) * tamanoPagina,
limit: tamanoPagina
}, order: [['apellido', 'ASC']]); // puede pedirse que vengan ordenados los resultados por dos campos


```

> Notar que de forma practica, dividimos los resultados en paginas, esto no implica que necesariamente la base de datos divida en paginas, es una forma de organizar los resultados:
> digamos que una pagina tiene 10 elementos, y yo quiero los elementos de la pagina 2, entonces offset (comienzo de la busqueda) empieza en la posicion 2 * 10 y limit (cantidad de elementos a recorrer) es de 10 registros

podemos aplicar a su vez ***funciones de agregacion*** como .count(), .max(), .sum(), avg() y demas tipicos de SQL:

```
const totalUsuarios = await Usuario.count();
const maxEdad = await Usuario.max('edad');
const sumSalarios = await Usuario.sum('salario');
```

y ***son compatibles con [Op]***

### Consultas RAW

Sequelize permite ejecutar consultas SQL directas, estas se denominan ***consultas RAW***.

para ello se utiliza el método .***query*** que toma como parámetro un string contenedor de la consulta, y un objeto con indicaciones del tipo de consulta:

```
const resultado = await sequelize.query(
"SELECT * FROM usuarios WHERE edad > 30",
{ type: QueryTypes.SELECT }
);
```


### Transacciones 

Sequelize permite definir transacciones como un conjunto de operaciones sobre la base de datos para que sean tratadas como una transacción atómica.

para ello, se debe definir una transacción con ***sequelize.transaction()*** (notar que este método es asíncrono y debe esperarse a que sequelize prepare la transacción) 

luego, para todas las operaciones sobre un modelo que queremos que forme parte de esa transacción, se le debe pasar un objeto indicando la transacción a la que pertenece.

para indicar cuando finalizar la transacción, se utiliza el método .***commit******(),*** o si se detecto errores, podemos cancelarla con .***rollback******()***

```
const t = await sequelize.transaction();
try {
	const usuario = await Usuario.create({
	nombre: 'Nicolás',
	apellido: 'Juárez',
	usuario: 'njuarez',
	email: 'nico@mail.com',
	genero: 'M',
	perfilId: 2
}, { transaction: t });
	await LogActividad.create({
	mensaje: `Alta de usuario ${usuario.usuario}`,
	usuarioId: usuario.id
}, { transaction: t });
	await t.commit();
} catch (error) {
	await t.rollback();
}
```

Podemos utilizar un ***wrapper*** en forma de callback pasado a .transaction() para que tanto el .***commit***() como el .***rollback***() se realizan de forma automática.

### Patrón repositorio

El patrón repositorio es una estrategia que permite separar la lógica del acceso a datos del resto de la aplicación, encapsulando las interacciones con la base de datos en una interfaz orientada a objetos (interfaz -> repositorio)

De esta forma toda la lógica de acceso a datos no afecta al resto de la aplicación, pudiendo modificar esta sin afectar al resto de capas.

Para cada modelo se define un repositorio, con métodos para acceder y realizar operaciones, o podemos realizar un repositorioBase genérico que puede usarse para cualquier modelo, y agregarle mas métodos de ser necesario, o usarla de extensión para un repositorio mas especifico.

```
export default class RepositorioBase {

constructor(modelo) {
this.modelo = modelo;
}

async obtenerTodos({ pagina = 1, limite = 10 } = {}) {
const offset = (pagina - 1) * limite;
return this.modelo.findAll({ limit: limite, offset: offset });
}

async obtenerPorId(id) {
return this.modelo.findByPk(id);
}

async crear(datos) {
return this.modelo.create(datos);
}

async actualizar(id, datos) {
const instancia = await this.modelo.findByPk(id);
if (!instancia) throw new Error(`Error: Instancia con id: ${id} no
encontrada`);
// Actualiza los datos de la instancia
// y guarda los cambios en la base de datos
// Se puede usar Object.assign o el método set para asignar los datos en lote
// Object.assign(instancia, datos);
// instancia.set(datos);
// y luego guardar los cambios en la base de datos
// return instancia.save();
// O simplemente usar el método update
// y pasarle los datos a actualizar
return instancia.update(datos);
}
async eliminar(id) {
const instancia = await this.modelo.findByPk(id);
if (!instancia) return 0;
await instancia.destroy();
return 1;
}
}
```