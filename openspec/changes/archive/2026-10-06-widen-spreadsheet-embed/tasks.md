## 1. Estructura de `<main>`

- [x] 1.1 En `<main>`, sustituir `max-w-4xl mx-auto w-full px-4 pt-3 flex-1` por `w-full pt-3 flex-1`
- [x] 1.2 En la sección del iframe, sustituir `w-full mb-8` por `mx-auto w-full max-w-[83rem] px-4 mb-8`
- [x] 1.3 Envolver la sección de sugerencias y la de metodología en un `<div class="max-w-4xl mx-auto w-full px-4">`, sin tocar las clases de ninguna de las dos
- [x] 1.4 Confirmar que la sección del iframe sigue siendo la primera `<section>` hija de `<main>` y que la metodología sigue siendo la última `<section>` dentro de `<main>`

## 2. URL del iframe

- [x] 2.1 Añadir `?gid=0&single=true&widget=false&headers=false&chrome=false` al final del `src` del iframe
- [x] 2.2 Confirmar que el `src` no lleva `range` y que `width`, `style`, `loading="lazy"` y `title` no cambian

## 3. Composición del iframe

- [x] 3.1 En el iframe, sustituir `rounded-xl shadow-lg border-0` por `border-0 mix-blend-multiply`
- [x] 3.2 A 1920 px de ancho, confirmar con captura que el margen a la derecha de la tabla se ve del color de fondo de la página y no blanco, y que los estilos computados del iframe dan `border-radius: 0`, `box-shadow: none` y `mix-blend-mode: multiply`

## 4. Verificación en navegador

- [x] 4.1 A 1920×1000 y a 1920×760 (con scroll vertical dentro del iframe): el iframe mide 1296 px, se ven todas las columnas sin scroll horizontal, la barra de scroll vertical queda pegada a la última columna y no aparecen ni la barra de título ni la de pestañas
- [x] 4.2 A 1280 px de ancho: el iframe mide el ancho útil del documento menos 32 px de padding (1248 px con scrollbars superpuestas; 1233 px con la barra clásica de 15 px de Windows)
- [x] 4.3 A 1024 px de ancho o más: el texto de la cabecera, la sección de sugerencias y la de metodología comparten borde izquierdo, y ninguna sección supera los 864 px
- [x] 4.4 A 375×667: el iframe mide lo mismo que el contenido de la cabecera, se ven sin scroll horizontal las columnas Tienda, Producto y Precio actual, el preámbulo hasta el borde superior del iframe no supera 220 px, y cabecera y franja de Telegram no se solapan
- [x] 4.5 A 900 px de alto: el borde superior del iframe es visible sin scroll
- [x] 4.6 A 375, 1280 y 1920 px de ancho: `scrollWidth` del documento igual a `clientWidth`, sin barra de scroll horizontal en la página

## 5. Alto del iframe

- [x] 5.1 Sustituir `height: 80vh; min-height: 600px;` por `height: 1190px;` (tabla de 1174 px + 16 px de barra horizontal) y anotar en el comentario del iframe que el alto se actualiza al añadir productos
- [x] 5.2 A 1920×900, 1336×768 y 384×800 (móvil): el contenedor con scroll de la tabla no tiene desbordamiento vertical, y el scroll vertical sobre la tabla mueve la página
