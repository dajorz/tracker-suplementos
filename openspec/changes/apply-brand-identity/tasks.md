## 1. Paleta

- [ ] 1.1 Justo después de `<script src="https://cdn.tailwindcss.com">`, añadir un `<script>` con `tailwind.config` que extienda los colores con `brand.navy` (`#051322`), `brand.navy-hover` (`#16304F`) y `brand.lime` (`#98F13E`)
- [ ] 1.2 Al cargar la página, confirmar con captura que la primera pintura ya sale con los colores de marca, sin parpadeo de estilos por defecto

## 2. Cabecera

- [ ] 2.1 Convertir el contenedor de la cabecera en una rejilla (*grid*) de dos columnas con un `<img src="apple-touch-icon-v1.png" alt="" width="44" height="44" class="rounded-full …">`. En móvil, el logo mide 36 px y acompaña solo al título; desde `sm`, mide 44 px y ocupa las dos filas
- [ ] 2.2 En móvil, la descripción ocupa las dos columnas; desde `sm`, solo la segunda
- [ ] 2.3 Poner el `<h1>` en `text-brand-navy` y peso `font-extrabold`, sin tocar su texto ni el de la descripción

## 3. Franja de Telegram

- [ ] 3.1 Sustituir el contenido del `<aside>` por un único `<a id="telegram-cta-link" href="https://t.me/NutriChollos" target="_blank" rel="noopener noreferrer">` que ocupe todo el ancho, con `bg-white`, `hover:bg-slate-50`, `border-b-[3px] border-brand-lime` y un anillo de foco visible
- [ ] 3.2 Dentro del enlace, poner el texto «🔔 El bot de este tracker te avisa por Telegram si detecta un producto en su **mínimo registrado**. Nada más.», con el emoji en `aria-hidden`, y un `<span>` en `sr-only` con «(se abre en una ventana nueva)»
- [ ] 3.3 Añadir a la derecha del texto la pastilla `<span>`: `bg-brand-navy text-brand-lime rounded-full font-bold`, con el texto «Unirme» y una flecha en `aria-hidden`. Debe medir al menos 44 px de alto por debajo de `sm` y cambiar a `brand.navy-hover` cuando el ratón pasa sobre el enlace (`group-hover`)
- [ ] 3.4 Confirmar que el listener de `join_telegram` sigue encontrando `telegram-cta-link` y que no queda ninguna clase `sky-*` en el fichero

## 4. Botones, títulos y pie

- [ ] 4.1 «Proponer producto»: sustituir `rounded-lg bg-slate-900 text-white hover:bg-slate-700` por `rounded-full bg-brand-navy text-brand-lime hover:bg-brand-navy-hover`
- [ ] 4.2 «Cerrar», en la política de cookies: el mismo cambio que en 4.1
- [ ] 4.3 Los `<h2>` de las tarjetas «¿Echas en falta algún producto?» y «Cómo funciona este tracker»: `text-brand-navy`
- [ ] 4.4 «Cookies», en el pie: sustituir el color heredado `slate-500` por `text-brand-navy`, manteniendo el subrayado y la zona táctil de 24 px

## 5. Verificación en el navegador

- [ ] 5.1 A 375×667: el borde superior del iframe no pasa de 220 px (el mockup daba 182), la descripción ocupa como mucho 2 líneas y a todo el ancho, la pastilla queda a la derecha del texto y mide al menos 44 px, y la franja mide al menos 44 px de alto
- [ ] 5.2 A 1280×800 y a 1920×1000: la cabecera tiene exactamente dos filas de texto con el logo a su lado, la descripción ocupa una sola línea, la franja de Telegram ocupa como mucho dos líneas y la pastilla queda a la derecha del texto
- [ ] 5.3 A 900 px de alto: la franja de Telegram y el borde superior del iframe se ven sin hacer scroll
- [ ] 5.4 Revisar todo texto o icono en `#98F13E` y confirmar que su fondo es navy. Confirmar con captura que la tabla sigue legible y que el iframe mantiene `mix-blend-mode: multiply`
- [ ] 5.5 Contraste AA (4,5:1) en el texto de la franja, el `<h1>`, la descripción, los `<h2>`, el aviso del pie, el crédito de Reddit y «Cookies»
- [ ] 5.6 El enlace de la franja es el único enlace o botón por encima de la tabla, su nombre accesible incluye «se abre en una ventana nueva» y, al pulsarlo, se abre `https://t.me/NutriChollos` con `window.opener === null`
- [ ] 5.7 Sin consentimiento, pulsar la franja no hace ninguna petición a Analytics. Con consentimiento, se envía `join_telegram`
- [ ] 5.8 Comparar el navy de la fila de cabecera de la tabla publicada con `#051322` y anotar la diferencia en `design.md` si se nota a simple vista

## 6. Medición

- [ ] 6.1 Tras desplegar, anotar la fecha de despliegue en la Decisión 7 de `design.md` como nuevo punto de corte para comparar la conversión de Telegram
