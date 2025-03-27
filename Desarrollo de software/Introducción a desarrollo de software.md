Comandos utiles de git

```
// ver diferencias entre los archivos de la ultima version y las modificaciones no staged
git diff
git diff {archivo}

// revertir cambios no agregados a un commit (staged o modificados)
git restore --staged archivo // saca de staged el archivo
git restore archivo // revierte todas las modificaciones

// solo por los loles
// Crear una rama es simple:
git branch [nueva_rama]

// moverse luego a esa rama
git checkout [nueva_rama]

// eliminar otra rama (no estando ubicados en esta)
git branch -d [nombre_de_ rama]

// realizar un merge desde una rama trayendo los cambios de otra rama
git merge [rama desde donde traer cambios]
```
