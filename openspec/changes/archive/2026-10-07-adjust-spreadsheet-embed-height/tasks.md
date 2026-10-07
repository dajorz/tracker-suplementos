## 1. Ajuste puntual

- [x] 1.1 Volver a medir la tabla publicada y confirmar que 1301 px cubren su altura de 1285 px mas 16 px para la barra horizontal.
- [x] 1.2 Cambiar la altura del iframe de 1228 px a 1301 px en `index.html` y actualizar el comentario existente con la medida actual, conservando el aviso de mantenimiento manual y sin modificar ancho, URL, atributos ni clases.

## 2. Verificacion

- [x] 2.1 Comprobar a 1920 px y 375 px que, tras cargar la hoja, `#sheets-viewport` cumple `scrollHeight <= clientHeight` y que el desplazamiento vertical sobre la tabla mueve la pagina sin mover verticalmente el contenedor interno.
- [x] 2.2 Comprobar a 375 px que las ultimas columnas siguen siendo accesibles mediante scroll horizontal interno y que no hay desbordamiento horizontal de la pagina; revisar visualmente la ultima fila y la separacion del contenido posterior en ambos viewports.
- [x] 2.3 Revisar el diff para confirmar que los unicos cambios de implementacion son la altura y el comentario existentes del iframe.