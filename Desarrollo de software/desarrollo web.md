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

las secciones, específicamente,  <header>, <footer>, <main>, <article>, <nav>, son utilizadas dentro del <body> para estructurarlo, mejorando su claridad (contrario al uso general de <div>), accesibilidad de la pagina para tecnologías especificas como lectores de pantalla, así como para mejorar la acepción por el SEO, los motores de búsqueda interpretan mejor el contenido cuando esta estructurada de forma semánticamente correcta.

![[Pasted image 20250325231636.png]]

Luego existen otras tags mas funcionales, como <form>, aquí una descripción corta:

![[Pasted image 20250326000525.png]]
![[Pasted image 20250326000630.png]]

Uno muy importante para la accesibilidad del usuario sera <label>

![[Pasted image 20250326000708.png]]