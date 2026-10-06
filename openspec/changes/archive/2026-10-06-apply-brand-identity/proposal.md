## Why

El proyecto ya tiene una identidad visual: un robot dentro de un anillo, en navy `#051322` y lima `#98F13E`. La usan la imagen OG, el favicon y, desde hace poco, la cabecera de la tabla publicada (fila navy con una línea lima debajo). La página que envuelve la tabla, en cambio, usa los grises y azules por defecto de Tailwind (`slate`, `sky`). En el móvil se ven tres estilos apilados que no tienen nada que ver entre sí: una cabecera blanca genérica, una franja azul cielo con un botón azul, y debajo la marca, que aparece por primera vez en la tabla.

El botón de Telegram es lo que peor queda. En el móvil es un bloque de 160×44 px pegado a la izquierda y solo en su línea, con un texto más grande que el de la franja que lo contiene. La franja deja el borde superior de la tabla a 213 px, a solo 7 px del límite de 220 px que fija la spec.

## What Changes

- **La cabecera incorpora el logo.** Es el robot circular, reutilizando el `apple-touch-icon-v1.png` que ya existe, sin añadir imágenes nuevas. El título pasa a navy.
- **La franja de Telegram se rediseña:**
  - En todos los tamaños, la franja entera es un único enlace.
  - El botón azul se sustituye por una pastilla navy con texto lima. Lleva un icono de avión de papel y el texto «Unirme». En móvil mide 44 px de alto y se coloca al lado del texto, no debajo.
  - El fondo deja de ser azul cielo y pasa a blanco. Debajo lleva una línea lima, la misma que separa la cabecera de la tabla en la hoja publicada.
  - El texto se acorta a una frase. Se mantienen las mismas garantías: que el canal es de este tracker, que avisa del mínimo registrado, que la detección la hace el bot, el «Nada más.» y que no promete ninguna frecuencia. Se elimina la pregunta inicial «¿No quieres entrar cada día?».
- **La paleta de la página pasa a la de la marca, manteniendo el fondo claro:**
  - Navy para los títulos.
  - Navy con texto lima para los botones («Proponer producto», «Cerrar» de la política de cookies).
  - El lima solo se usa como relleno o como línea, nunca como texto sobre blanco, porque daría un contraste de 1,4:1.
- **La zona de la tabla no cambia.** Sigue clara y con `mix-blend-multiply`. Un fondo oscuro la haría ilegible.
- El banner de consentimiento, la política de cookies, la medición `join_telegram` y la tabla publicada no cambian de comportamiento.

## Capabilities

### New Capabilities
(ninguna)

### Modified Capabilities
- `pricing-tracker-landing-page`:
  - **«Compact header bar»** pasa a exigir el logo del proyecto junto al título, sin que añada filas de texto en escritorio.
  - **«Telegram channel call to action»** pasa a exigir que toda la franja sea un único enlace, con un indicador de acción visible en forma de pastilla a la derecha del texto en todos los tamaños. Cambia el texto y se ajustan los escenarios del objetivo táctil mínimo.
  - Se añade un requisito de **paleta de marca**: qué colores se usan, dónde, y que el lima nunca se use como color de texto sobre fondo claro.

## Impact

- **Fichero afectado:** `index.html` (clases de la cabecera, la franja, los botones, los títulos y el pie, y el marcado de la franja de Telegram).
- **Sin cambios en dependencias ni en el comportamiento del JavaScript.** El listener de `join_telegram` sigue colgado del mismo `id`, que pasa a estar en la franja completa. Sin build step: los colores de marca se escriben como valores arbitrarios de Tailwind o con una configuración en línea del CDN.
- **No se añaden imágenes.** El logo es un fichero ya publicado de 180×180, así que se sigue cumpliendo la regla de no subir imágenes cuadradas de más de 180 px.
- **Sin fuentes externas:** se sigue usando la tipografía del sistema, así que la política de cookies no cambia.
- **El tráfico hacia Telegram puede variar.** El cambio de texto y de forma altera la medición de conversión que se puso en marcha con la línea base del 2026-09-17. Hay que anotar la fecha de despliegue como un nuevo punto de corte.
