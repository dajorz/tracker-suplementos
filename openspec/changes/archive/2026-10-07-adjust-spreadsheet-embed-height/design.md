## Context

La pagina estatica contiene un iframe de Google Sheets con altura manual de 1228 px. Tras anadir productos, la tabla publicada se midio de nuevo el 2026-10-07: 1285 px de alto. Con la barra horizontal de 16 px, el contenedor interno solo disponia de 1212 px en movil y presentaba desbordamiento vertical; en escritorio tambien excedia el alto disponible.

## Goals / Non-Goals

**Goals:**
- Recuperar la ausencia de scroll vertical interno para la tabla actual en escritorio y movil.
- Mantener el desplazamiento horizontal de la tabla en pantallas estrechas.
- Actualizar el comentario de referencia junto al iframe.

**Non-Goals:**
- Adaptacion automatica de altura, lectura de CSV o sustitucion del iframe.
- Cambios en columnas, formato, ancho del embed, URL, consentimiento o marca.
- Modificar la hoja publicada.

## Decisions

### 1. Ajustar la altura a 1301 px

Se suma a la altura medida (1285 px) el espacio de 16 px para la barra horizontal. Asi, el iframe de 1301 px deja 1285 px utiles en movil y conserva espacio libre en escritorio. Se conserva el estilo en linea existente y se actualiza la medida del comentario existente.

### 2. Comprobar el contenedor interno, no solo el iframe

En viewports de 1920 px y 375 px se comprobara que `#sheets-viewport` tiene `scrollHeight <= clientHeight` tras cargar la tabla, y que una rueda vertical sobre ella desplaza la pagina sin mover verticalmente el contenedor interno. En movil se comprobara tambien que el desplazamiento horizontal permite alcanzar las ultimas columnas y que la pagina no desborda horizontalmente.

La herramienta de navegador puede inspeccionar el documento publicado; el JavaScript de la pagina no tiene ese acceso por la proteccion entre origenes. No se introduce codigo de medicion en la pagina.

## Risks / Trade-offs

- [Nuevos productos o cambios de formato pueden superar de nuevo la altura] -> Se conserva el aviso de mantenimiento manual junto al iframe; este arreglo solo cubre el contenido actual.
- [La hoja puede cambiar entre la propuesta y la implementacion] -> Volver a medir antes de aplicar; si 1301 px deja de ser suficiente, revisar la cifra en los artefactos antes de implementar.

## Migration Plan

Modificar exclusivamente la altura y su comentario en `index.html`, verificar en el navegador y publicar mediante el flujo habitual de GitHub Pages. La vuelta atras restaura el valor y comentario anteriores (1228 px y medida de 1211 px).

## Open Questions

Ninguna para el ajuste puntual acordado.