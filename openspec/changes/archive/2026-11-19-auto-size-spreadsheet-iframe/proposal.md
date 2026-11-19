# Proposal

## Why

Al añadir productos, la hoja publicada vuelve a superar la altura fija del iframe y reaparece el scroll vertical interno. La altura necesaria también varía según el navegador y las fuentes, por lo que reajustarla a mano no es una solución duradera.

## What Changes

- Ajustar automáticamente la altura del iframe original de Google Sheets a la tabla publicada, sin modificar la hoja ni sustituir su presentación.
- Recalcular cuando cambie el ancho disponible o la escala de pantalla y adaptar la altura a los productos presentes al cargar la página, sin actualizar el código por cada alta o baja.
- Mantener el scroll horizontal dentro del iframe en móvil y el desplazamiento vertical de la página cuando la medición funciona.
- Conservar un iframe utilizable con scroll interno y registrar el error si no es posible medir la publicación, en lugar de ocultar el contenido.
- Documentar las dependencias de Google y verificar la solución en diferentes navegadores, tamaños y estados de fallo.

## Capabilities

### New Capabilities

Ninguna.

### Modified Capabilities

- `pricing-tracker-landing-page`: sustituir la altura mantenida a mano por un ajuste automático del iframe y especificar su comportamiento ante fallos.

## Impact

- `index.html`: medición y ajuste en el cliente, manteniendo el despliegue estático y el iframe original.
- `README.md`: actualizar las instrucciones y limitaciones de mantenimiento de la tabla.
- Google Sheets: lectura adicional de la publicación para medirla; la solución depende de que Google permita esa lectura y mantenga una estructura compatible.
- No se requieren cambios en la hoja, un servidor propio ni un paso de compilación.
