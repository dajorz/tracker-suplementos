## Why

Tras anadir mas productos a la hoja, la tabla publicada mide 1366 px en el navegador integrado de VS Code en Windows (1307 px en Chromium sin cabeza en Linux) y supera el espacio util del iframe actual de 1301 px. Ha reaparecido el scroll vertical interno; hay que recalibrar la altura manual.

## What Changes

- Subir la altura fija del iframe de 1301 px a 1382 px (1366 px de tabla + 16 px de la barra horizontal).
- Actualizar la medida de referencia del comentario existente junto al iframe (de 1285 a 1366 px).
- Anadir un escenario de verificacion que exige que la altura se recalcule contra el `scrollHeight` del contenedor interno cada vez que cambia el numero de productos.
- Mantener sin cambios el ancho, la URL publicada, la carga diferida y la composicion visual.
- Conservar el mantenimiento manual: no se incorpora calculo automatico ni una tabla propia.

## Capabilities

### New Capabilities

Ninguna.

### Modified Capabilities

- `pricing-tracker-landing-page`: precisar que la altura del iframe se deriva de la altura renderizada del contenedor interno mas el grosor de la barra horizontal, y que se recalibra al cambiar los productos.

## Impact

Solo la altura y el comentario existente del iframe en `index.html`. Sin nuevas dependencias, cambios en Google Sheets, APIs ni pasos de compilacion. La publicacion sigue siendo mediante GitHub Pages.
