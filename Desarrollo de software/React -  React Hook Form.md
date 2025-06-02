React Hook Form simplifica la gestión de las entradas en formularios de React (su validación, manejo de errores y rendimiento)

### Componentes de React Hook Form

```
import React from 'react';
import { useForm } from 'react-hook-form';

function MiFormulario() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = (data) => {
    console.log('Datos del formulario:', data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label htmlFor="name">Nombre:</label>
      <input
        id="name"
        type="text"
        {...register('name', { required: 'El campo nombre es requerido' })}
      />
      {errors.name && <p>{errors.name.message}</p>}
      <br />

      <label htmlFor="email">Correo electrónico:</label>
      <input
        id="email"
        type="email"
        {...register('email', {
          required: 'El campo correo electrónico es requerido',
        })}
      />
      {errors.email && <p>{errors.email.message}</p>}
      <br />

      <button type="submit">Enviar</button>
    </form>
  );
}

export default MiFormulario;
```

Inicializacion con useForm

```
const { register, handleSubmit, formState: { errors } } = useForm();
```


***useForm*** permite parsear los datos de un formulario rapidamente, para ello cada campo del formulario debe tener el atributo register con el nombre del campo que se va a enviar al servidor

Luego ***HandleSubmit*** maneja el parseo de los datos en submit y le pasa los datos a un callback, en este caso la funcion ***onSubmit***, 
***formState*** es un objeto que contiene el estado del formulario, valores etc, como los errores de validación
***errors*** es un objeto que contiene los errores de validación de cada campo que existe en formState, estas validaciones se agregan en el register, con un objeto options

Ahora, para agregar el atributo register a los campos del formulario que queremos formen parte de los datos recuperados, se usa la siguiente sintaxis

```
<input 
{...register("nombre", {required: true})} 
id="nombre" className="form-control" type="text" placeholder="Nombre"/>

{errors.nombre && errors.barrio.type === "required" && <span className="text-danger">Este campo es obligatorio</span>}
```

`{...register` esta usando una sintaxis de propagación para agregar al elemento input todos los objetos contenidos en la devolución de register()

para dejar claro esto, veamos este mini ejemplo:

```
const props = {
  id: "nombre",
  type: "text",
  placeholder: "Tu nombre",
};

//Luego

<input {...props} />

// es lo mismo que escribir esto

<input id="nombre" type="text" placeholder="Tu nombre" />

```

luego, a register se le pasa el nombre del campo que se utilizara para el campo en el objeto de datos.

Finalmente, se le pasa a register un segundo parámetro, que es un objeto que contendrá las validaciones

Estas pueden ser:

```
required: true

minLength: 8

maxLength: 20

validate: (value) => value === algo  

// este ultimo podria ser util para hacer un regex que detecte si es un formato de mail

pattern: {
value: /^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z]{2,4}$/i
}
```

Luego, si se incumplen algunas de estas validaciones, sera posible acceder a estas mediante ***formState.errors*** o ***errors*** en este caso al descomponer formState

### formState

Existe los siguientes propiedades en formState:

```
isDirty: Indica si el usuario ha interactuado con alguno de los campos del formulario (es decir, si se han modificado los valores iniciales de los campos).

isValid: Indica si todos los campos del formulario cumplen con las reglas de validación.

isSubmitting: Indica si el formulario está actualmente en proceso de envío (es decir, si se ha llamado a la función handleSubmit y está en ejecución).

errors: ya lo explicamos nene
```

Estas propiedades pueden ser útiles para mejorar la experiencia del usuario y entregarle feedback sobre su interacción con el formulario