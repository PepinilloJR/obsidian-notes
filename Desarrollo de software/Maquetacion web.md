la maquetacion web es el proceso de estructurar y organizar el contenido de una pagina web, utilizando etiquetas HTML, mediante la division del contenido en secciones logicas y coherentes.

esto facilita la navegacion por esta por parte de usuarios y motores de busqueda.

para ello se usan etiquetas que aporten un significado semantico a la pagina, dandole porposito a cada seccion, por ejemplo mediante el uso de header, nav, main, secition, article, aside, footer, etc.

la maquetación HTML también implica la aplicación de estilos
CSS para controlar el diseño visual de la página, como el tamaño y el posicionamiento de los elementos, los
colores y las fuentes utilizadas, los márgenes y rellenos, entre otros aspectos

### Modelo de cajas

todo en CSS tiene una caja, es decir, cada elemento es tratado como una caja, con tamaños, posiciones, entre otros parámetros,
y se usa esto como base para estilar las paginas

las cajas pueden ser en bloque o en linea

Si una caja se define como un bloque, se comportará de las maneras siguientes:
* La caja fuerza un salto de línea al llegar al final de la línea.
* La caja se extenderá en la dirección de la línea para llenar todo el espacio disponible que haya en su contenedor. En la mayoría de los casos, esto significa que la caja será tan ancha como su contenedor, y llenará el 100% del espacio disponible.
* Se respetan las propiedades width y height.
* El relleno, el margen y el borde mantienen a los otros elementos alejados de la caja.

Si una caja tiene una visualización externa de tipo inline, entonces:
* La caja no fuerza ningún salto de línea al llegar al final de la línea.
* Las propiedades width y height no se aplican. Se aplican relleno, margen y bordes verticales, pero no mantienen alejadas otras cajas en línea.
* Se aplican relleno, margen y bordes horizontales, y mantienen alejadas otras cajas en línea.

### Visualización externa e interna

la visualización externa como ya vimos, trata sobre si actúa como caja bloque o en linea

pero es posible definir la visualización interna de una caja, donde se define como se disponen los elementos dentro de esta.

en general, presentan un flujo normal, es decir, los elementos internos actuaran como cajas de tipo bloque o en linea

sin embargo, podemos utilizar otros métodos de visualización interna como flex o grid, que proveen mayor flexibilidad para posicionar los elementos

las cajas presentan distintas secciones:

* El contenido de la caja (o content box): El área donde se muestra el contenido, cuyo tamaño puede cambiarse utilizando propiedades como width y height.
* El relleno de la caja (o padding box): El relleno es espacio en blanco alrededor del contenido; es posible controlar su tamaño usando la propiedad padding y otras propiedades relacionadas.
* El borde de la caja (o border box): El borde de la caja envuelve el contenido y el de relleno. Es posible controlar su tamaño y estilo utilizando la propiedad border y otras propiedades relacionadas.
* El margen de la caja (o margin box): El margen es la capa más externa. Envuelve el contenido, el relleno y el borde como espacio en blanco entre la caja y otros elementos. Es posible controlar su tamaño usando la propiedad margin y otras propiedades relacionadas.


### Bootstrap y el diseño responsive

bootstrap es un framework de desarrollo web de código abierto que proporciona una serie de estilos CSS predefinidos para facilitar la creación de sitios web, con un diseño responsivo y moderno

recordemos que el diseño responsivo permite que un sitio web se adapte a diferentes dispositivos, cambiando para cada tamaño de pantalla de modo que sea posible de usar en cualquiera de estos

lo escencial es entender como lo hace utilizando un sistema de cuadriculas flexible, a través de la cual distribuye cada elemento, siendo estas 12 columnas por cada fila que definamos para mostrar contenido en la pagina.

Bootstrap esta constituido por una serie de archivos CSS y de JavaScript, cada uno responsable de asignar características especificas de la pagina siempre que el programador lo solicite

Bootstrap puede ser instalado mediante un CDN (content delivery network) mediante el cual se provee de las herramientas mencionadas a través de un servidor. Tambien es posible usarlo descargando los archivos que lo componen, e instalarlos manualmente al proyecto.

### Componentes de bootstrap

adicionalmente con las clases, bootstrap ofrece elementos y conjuntos de elementos HTML combinados con CSS y JS que facilitan la construcción de interfaces.

![[Pasted image 20250410172515.png]]
