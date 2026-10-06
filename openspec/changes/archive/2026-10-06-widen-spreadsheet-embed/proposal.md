## Why

La tabla que publica Google mide hoy 1280 px de ancho, ya con los nombres de producto acortados y la columna «Producto» a 160 px con ajuste de línea. El iframe vive dentro de `max-w-4xl`, que deja 864 px: «Valoración», «Mínimo registrado» y «Disponibilidad» solo aparecen con scroll horizontal. Es justo la columna «Valoración» la que da sentido a los colores de las filas, y la sección de metodología explica el «mínimo registrado» y el «€/100 g», dos columnas que el visitante no llega a ver.

Además, el embed gasta espacio en elementos de Google que no aportan nada aquí: una barra de título de 29 px dentro del iframe y una barra de pestañas de 26 px abajo, con una sola pestaña.

Por último, Google pinta su documento de blanco opaco y alinea la tabla a la izquierda. Cuando el iframe es más ancho que la tabla, el sobrante aparece como una franja blanca enmarcada por la sombra de la tarjeta.

## What Changes

- La sección del iframe sale del ancho de lectura de `max-w-4xl` y pasa a un contenedor propio, más ancho, centrado y con tope. En escritorios anchos la tabla cabe entera; en un portátil de unos 1250 px útiles se ve hasta «Mínimo registrado» inclusive.
- La cabecera, la franja de Telegram, la sección de sugerencias, la de metodología y el pie mantienen exactamente su ancho y alineación actuales.
- La URL del iframe añade los parámetros de publicación que retiran la barra de título y la barra de pestañas, y que fijan la pestaña «Precios» por su `gid`.
- El iframe deja de ser una tarjeta (sin sombra ni esquinas redondeadas) y se compone con `mix-blend-mode: multiply`, de modo que el blanco de Google toma el color de fondo de la página. La tabla queda directamente sobre la página y el ancho sobrante no se ve.
- El iframe pasa de `80vh` a un alto fijo igual al de la tabla, así que no tiene scroll vertical propio. En móvil solo se desplaza la página; la tabla solo se desplaza en horizontal. El alto se actualiza a mano al añadir productos.
- En móvil el ancho no cambia: por debajo de 896 px de ventana el iframe mide lo mismo que hoy. Sí se aplican la nueva URL y la composición sin tarjeta.
- No se toca la hoja de Google: ni columnas, ni formato, ni contenido.

## Capabilities

### New Capabilities
(ninguna)

### Modified Capabilities
- `pricing-tracker-landing-page`: el requisito «Embedded Google Sheet iframe» pasa a exigir un contenedor más ancho que el del texto, con tope, y una URL de embed sin barra de título ni barra de pestañas, fijada a la pestaña de precios. Deja de exigir esquinas redondeadas y sombra, exige que el fondo del embed se funda con el de la página, y sustituye el alto adaptativo (`80vh`, mínimo 600 px) por un alto fijo igual al de la tabla, sin scroll vertical interno.

## Impact

- Fichero afectado: `index.html`. Cambian las clases de `<main>`, de la sección del iframe y del propio iframe; se añade un contenedor para las dos secciones de texto y cambia el `src` del iframe.
- Sin JavaScript nuevo, sin dependencias y sin build step.
- El ancho máximo y el alto del iframe se ajustan a la tabla publicada y se mantienen a mano: el ancho, cuando el propietario cambia columnas; el alto, cuando añade o quita productos. Si no se actualizan, vuelve un scroll interno corto, sin pérdida de datos.
- La política de cookies no cambia: el contenido incrustado sigue siendo el mismo documento publicado de Google Sheets.
