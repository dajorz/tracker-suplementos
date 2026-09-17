## 1. Marcado de la tira

- [x] 1.1 Insertar en `index.html` un bloque `w-full` inmediatamente **después** del disclaimer ámbar y **antes** de `<main>`, siguiendo el mismo patrón de contenedor (`max-w-4xl mx-auto w-full px-4`), con `bg-sky-50`, sin `border-b` y `text-xs`.
- [x] 1.2 Maquetarlo como `flex` con el texto a la izquierda y el botón a la derecha en desktop, apilado en móvil, sin superar dos líneas de texto en desktop.
- [x] 1.3 Verificar que el botón es el único elemento accionable de toda la región situada por encima del iframe.

## 2. Copy

- [x] 2.1 Redactar el texto como: «🔔 ¿No quieres entrar cada día? El canal de Telegram de este tracker te avisa cuando el bot detecta que un producto toca su mínimo registrado. Nada más.»
- [x] 2.2 Etiquetar el botón «Unirme en Telegram» (no «NutriChollos»).
- [x] 2.3 Comprobar que el copy no contiene «mínimo histórico» sin matizar, ninguna cifra de frecuencia de mensajes, ni ninguna promesa de exhaustividad.

## 3. Enlace

- [x] 3.1 `href="https://t.me/NutriChollos"` con `target="_blank"` y `rel="noopener noreferrer"`.
- [x] 3.2 Confirmar que el enlace no introduce cookies ni peticiones adicionales en la carga de la página.

## 4. Verificación de no regresión

- [x] 4.1 A 900px de alto: la tira es visible sin scroll y el borde superior del iframe sigue visible.
- [x] 4.2 A 375×667: cabecera, disclaimer y tira se apilan sin solaparse y el iframe sigue apareciendo en pantalla.
- [x] 4.3 Con cookies rechazadas o sin decidir: cero peticiones a `googletagmanager.com`.
- [x] 4.4 El disclaimer de precios sigue sin control de cierre, fuera de `<details>`, y ningún script lo oculta.
- [x] 4.5 El CTA `mailto:` inferior y el ensamblado anti-scraping de la dirección siguen funcionando (`id="suggest-product-link"` intacto).
- [x] 4.6 El diálogo de política de cookies y el control «Cookies» siguen operativos y sin cambios de contenido.
- [x] 4.7 El aviso de precios usa tinte neutro, sin icono de aviso, y la tira de Telegram es el único bloque con color entre la cabecera y la tabla.
- [x] 4.8 Sobre la tabla queda una única regla horizontal: el `border-b` de la cabecera.
- [x] 4.9 El botón alcanza 4.5:1 de contraste (`bg-sky-700`, medido 5.93:1) y 32px de alto, por encima del mínimo de 24px de WCAG 2.2 SC 2.5.8.

## 5. Medición

- [x] 5.1 Añadir un evento GA4 `join_telegram` en el clic del botón, reutilizando la comprobación de consentimiento ya existente en `consentGate()`.
- [x] 5.2 Verificar que con consentimiento rechazado o sin responder el clic no genera ninguna petición a `googletagmanager.com`, y que el enlace sigue abriendo Telegram igualmente.
- [x] 5.3 Anotar la fecha de puesta en producción para poder comparar el delta de suscriptores, y no modificar posición ni copy durante 3–4 semanas. → **2026-09-17** (ver Decisión 7).

## 6. Accesibilidad

- [x] 6.1 Subir el crédito de Reddit y el control «Cookies» de `slate-400` a `slate-500` (2.56:1 → 4.76:1).
- [x] 6.2 Dar área táctil de al menos 24×24px a «Cookies» (42×24) y «Más información» (103×28).
- [x] 6.3 Sustituir los `div` huérfanos por landmarks: `<aside>` en las dos tiras, `<footer>` en la línea de cookies y `role="region"` con nombre accesible en el banner de consentimiento.
- [x] 6.4 Anunciar la apertura en pestaña nueva del enlace a Telegram con un `<span class="sr-only">`.
- [x] 6.5 Reverificar el flujo de consentimiento tras los cambios de marcado: banner, diálogo de política, aceptar, evento y reapertura desde «Cookies».
