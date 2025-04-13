diferencia entre sitio web y aplicación web

un sitio web es un conjunto de paginas estáticas que entregan información desde un navegador, una aplicación web son plataformas principalmente interactivas de contenido dinámico, una aplicación web puede ser parte de un sitio web, no al revés

si la web tiene solo frontend, es estática, ya que provee la misma información sin importar el usuario

Las aplicaciones web utilizan distintos lenguajes de programación entre sus diferentes capaz para funcionar, y se gestionan desde un CMS (**sistema de gestión de contenidos**) que permite crear y administrar de contenidos de la pagina desde bases de datos, donde este contenido luego sera mostrado dinamicamente en la aplicación

**HTML** (HyperText Markup Lenguage) es un **lenguaje estándar de marcado** que se utiliza para dar estructura a una pagina web, basado en etiquetas que definen como se organiza el contenido.

Hipertexto hace referencia a que, las distintas paginas web son conectadas entre si mediante enlaces.

Su estructura básica es la siguiente:

![[Pasted image 20250325220232.png]]

donde:

```
<html> -> contiene todo el contenido de la pagina, es el elemento raiz

<head> -> contenido no visible que es útil para configurar la pagina con metadatos y especificación de recursos
		<meta> -> metadatos, se puede especificar conjunto de caracteres, entre otros tipo 
	<title> -> titulo de la pagina
	<link> -> vincular recursos como fuentes, iconos, CSS, entre otros
	<script> -> especificacion de scripts de JavaScript, no exclusivo <head>
```

Secciones en HTML5

las secciones, específicamente,
```
  <header>, <footer>, <main>, <article>, <nav>, 
```
son utilizadas dentro del body para estructurarlo, mejorando su claridad (contrario al uso general de div), accesibilidad de la pagina para tecnologías especificas como lectores de pantalla, así como para mejorar la acepción por el SEO, los motores de búsqueda interpretan mejor el contenido cuando esta estructurada de forma semánticamente correcta.

![[Pasted image 20250325231636.png]]

Luego existen otras tags mas funcionales, como form, aquí una descripción corta:

![[Pasted image 20250326000525.png]]
![[Pasted image 20250326000630.png]]

Uno muy importante para la accesibilidad del usuario sera label

![[Pasted image 20250326000708.png]]
### CSS

CSS es un lenguaje de hojas de estilo en cascada (Cascade stylesheet o simplemente stylesheet) que da estilo a los elementos HTML

esto lo hace mediante reglas de estilo, que son un conjunto de instrucciones como se debe presentar un elemento HTML especifico, o elementos HTML con un ID especifico, o con un CLASS especifico, entre otras posibles condiciones

un ejemplo de esto es la siguiente regla de estilo:

![[Pasted image 20250410173011.png]]

### Selectores:

Como vimos, los selectores son como una condición, ya sea especificada por un elemento HTML, un ID, una clase, etc.

También es posible selectores mas avanzados, donde podemos seleccionar un elemento y aplicar la regla de estilo, bajo la condición de un estado especifico, una parte especifica del elemento HTML.

![[Pasted image 20250410173456.png]]

Podemos adicionalmente aplicar selectores jerárquicos, es decir, tomar en consideración el orden de los elementos HTML a la hora de aplicar el estilo.

![[Pasted image 20250410173504.png]]

### Escritura de reglas CSS

Las reglas de CSS pueden ser aplicadas de tres formas al HTML.

1. Escribiendo las reglas en un archivo externo e importándolas

```
<link href="styles/style.css" rel="stylesheet" type="text/css">
```

2. Dentro del Head del HTML, mediante un elemento Style

```
<head>
<style>
p { color: red; }
</style>
</head>
```

3. En linea, directamente sobre el elemento HTML, escribiendo la regla en un string.

```
<p style="color: blue; text-align: center;">
```

### Especificidad de selectores

la especificidad de selectores hace referencia a, mientras mas especifico sea el selector, tendrá mayor prioridad frente a otros menos específicos, si se esta aplicando estilos al mismo elemento (conflictos), específicamente, tendrá mas prioridad la propiedad dentro de la regla, si en la otra regla de estilo aplicada también esta siendo

```
/* Menos específico */
p {
  color: red;
}

/* Más específico */
div p {
  color: blue;
}

/* Mucho más específico */
#mi-id p {
  color: green;
}
```

es posible sobrescribir esta característica de CSS utilizando la regla 
!importante, del siguiente modo:

```
p {
  color: red !important;
}
```

### Variables o Propiedades personalizadas
es posible crear variables en CSS, de modo que se pueda referenciar valores de propiedades para usarse en diferentes lugares, ahorrando volver a definir todo.

estas se definen con el formato

```
--Nombre-variable : valor
```

luego, puede utilizarse de modo que:

![[Pasted image 20250410175245.png]]