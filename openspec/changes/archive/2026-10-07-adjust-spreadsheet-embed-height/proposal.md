## Why

Tras incorporar mas productos, la tabla publicada ha crecido hasta 1285 px y supera el espacio util del iframe actual de 1228 px. Ha reaparecido el scroll vertical interno; se necesita un ajuste puntual para recuperar el desplazamiento vertical de la pagina.

## What Changes

- Subir la altura fija del iframe de 1228 px a 1301 px, dejando espacio para la barra horizontal.
- Actualizar la medida de referencia en el comentario existente junto al iframe.
- Ampliar los escenarios de verificacion de altura para cubrir la tabla actual en escritorio y movil.
- Mantener sin cambios el ancho, la URL publicada, la carga diferida y la composicion visual.
- Conservar el mantenimiento manual: no se incorpora calculo automatico ni una tabla propia.

## Capabilities

### New Capabilities

Ninguna.

### Modified Capabilities

- `pricing-tracker-landing-page`: precisar la comprobacion del requisito de ausencia de scroll vertical interno tras recalibrar la altura, tanto en escritorio como con scroll horizontal en movil.

## Impact

Solo la altura y el comentario existente del iframe en `index.html`. Sin nuevas dependencias, cambios en Google Sheets, APIs ni pasos de compilacion. La publicacion sigue siendo mediante GitHub Pages.