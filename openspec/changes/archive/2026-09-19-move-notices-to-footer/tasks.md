## 1. Cabecera

- [x] 1.1 Retirar el `<span>` con "· Con feedback de la comunidad de Reddit" del párrafo del `<header>`, dejando la frase de descripción terminada tras "España" sin separador colgante
- [x] 1.2 Verificar que la frase resultante del `<header>` coincide carácter a carácter con `meta description`, `og:description` y `twitter:description`, sin tocar el `<head>`

## 2. Región superior

- [x] 2.1 Eliminar por completo el bloque `<aside aria-label="Aviso sobre la exactitud de los precios">`, conservando su texto para reutilizarlo en la tarea 3.1
- [x] 2.2 Comprobar que el `<aside aria-label="Canal de Telegram del tracker">` queda como primer elemento tras `</header>` en orden de documento
- [x] 2.3 Elevar el botón `#telegram-cta-link` a un mínimo de 44px de alto (subir el padding vertical y el tamaño de tipo), sin alterar su copy, su `href`, su `target`, su `rel` ni su `aria` de pestaña nueva
- [x] 2.4 Cambiar `pt-6` por `pt-3` en `<main>`

## 3. Pie

- [x] 3.1 Reescribir el `<footer>` para alojar el texto íntegro del aviso de exactitud de precios como primer contenido, sin acortarlo ni reformularlo
- [x] 3.2 Añadir bajo el aviso una segunda línea con "Con feedback de la comunidad de Reddit" y el botón `#cookie-settings` existente, separados por un middot
- [x] 3.3 Mantener el botón «Cookies» con subrayado y su área táctil de al menos 24×24px, sin convertirlo en un control con aspecto de botón

## 4. Verificación de accesibilidad

- [x] 4.1 Medir el contraste del aviso del pie, del crédito a Reddit y del control «Cookies» contra su fondo, y confirmar que los tres alcanzan 4.5:1
- [x] 4.2 Medir la caja del botón de Telegram y confirmar que es de al menos 44px de alto en 375px y en 1280px de ancho
- [x] 4.3 Medir la caja de todos los demás enlaces y botones y confirmar el mínimo de 24×24px
- [x] 4.4 Confirmar que todo hijo directo de `<body>` con texto sigue siendo un landmark o lleva rol explícito con nombre accesible, tras haber eliminado un `<aside>`

## 5. Verificación de layout

- [x] 5.1 Medir la distancia desde el top del viewport hasta el borde superior del iframe en 375px de ancho y confirmar que no supera los 220px
- [x] 5.2 Medir la misma distancia en 1280px de ancho y registrar el valor para el resumen del cambio
- [x] 5.3 Confirmar en un viewport de 900px de alto que el borde superior del iframe sigue visible sin hacer scroll
- [x] 5.4 Confirmar en 375×667 que cabecera y tira de Telegram se apilan sin solaparse y que el borde superior del iframe sigue en pantalla
- [x] 5.5 Confirmar que el párrafo de descripción de la cabecera no supera dos filas de texto en 375px de ancho
- [x] 5.6 Confirmar que la descripción sigue entrando en una sola línea con un ancho de contenido de cabecera de 864px o más
- [x] 5.7 Confirmar que el pie no se solapa con la sección de sugerencias ni con el banner de consentimiento en 375×667

## 6. Verificación de no regresión

- [x] 6.1 Confirmar que el texto "Con feedback de la comunidad de Reddit" aparece exactamente una vez en la página y está dentro del `<footer>`
- [x] 6.2 Confirmar que el aviso del pie es idéntico al que se mostraba sobre la tabla, sin cambios de redacción
- [x] 6.3 Confirmar que no queda ningún bloque de aviso entre `</header>` y `<main>`, y que la tira de Telegram es el único elemento accionable de esa región
- [x] 6.4 Confirmar que el flujo de consentimiento sigue funcionando: banner en primera visita, aceptar inyecta `gtag.js` una sola vez, rechazar borra cookies `_ga` y recarga
- [x] 6.5 Confirmar que «Cookies» en el pie reabre el banner y que el diálogo de política sigue abriéndose desde «Más información»
- [x] 6.6 Confirmar que el CTA `mailto:` sigue ensamblándose por JavaScript y que la dirección no aparece literal en el HTML servido
- [x] 6.7 Confirmar que el clic en Telegram sigue emitiendo `join_telegram` solo con consentimiento aceptado
