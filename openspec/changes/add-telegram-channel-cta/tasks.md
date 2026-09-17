## 1. Marcado de la tira

- [ ] 1.1 Insertar en `index.html` un bloque `w-full` inmediatamente **después** del disclaimer ámbar y **antes** de `<main>`, siguiendo el mismo patrón de contenedor (`max-w-4xl mx-auto w-full px-4`), con `bg-sky-50`, borde inferior `sky-200` y `text-xs`.
- [ ] 1.2 Maquetarlo como `flex` con el texto a la izquierda y el botón a la derecha en desktop, apilado en móvil, de forma que ocupe una sola línea de texto en viewports anchos.
- [ ] 1.3 Verificar que el botón es el único elemento accionable de toda la región situada por encima del iframe.

## 2. Copy

- [ ] 2.1 Redactar el texto como: «🔔 ¿No quieres entrar cada día? El canal de Telegram de este tracker te avisa cuando detectamos que un producto toca su mínimo registrado. Nada más.»
- [ ] 2.2 Etiquetar el botón «Unirme en Telegram» (no «NutriChollos»).
- [ ] 2.3 Comprobar que el copy no contiene «mínimo histórico» sin matizar, ninguna cifra de frecuencia de mensajes, ni ninguna promesa de exhaustividad.

## 3. Enlace

- [ ] 3.1 `href="https://t.me/NutriChollos"` con `target="_blank"` y `rel="noopener noreferrer"`.
- [ ] 3.2 Confirmar que el enlace no introduce cookies ni peticiones adicionales en la carga de la página.

## 4. Verificación de no regresión

- [ ] 4.1 A 900px de alto: la tira es visible sin scroll y el borde superior del iframe sigue visible.
- [ ] 4.2 A 375×667: cabecera, disclaimer y tira se apilan sin solaparse y el iframe sigue apareciendo en pantalla.
- [ ] 4.3 Con cookies rechazadas o sin decidir: cero peticiones a `googletagmanager.com`.
- [ ] 4.4 El disclaimer ámbar sigue sin control de cierre, fuera de `<details>`, y ningún script lo oculta.
- [ ] 4.5 El CTA `mailto:` inferior y el ensamblado anti-scraping de la dirección siguen funcionando (`id="suggest-product-link"` intacto).
- [ ] 4.6 El diálogo de política de cookies y el control «Cookies» siguen operativos y sin cambios de contenido.

## 5. Medición

- [ ] 5.1 Añadir un evento GA4 `join_telegram` en el clic del botón, reutilizando la comprobación de consentimiento ya existente en `consentGate()`.
- [ ] 5.2 Verificar que con consentimiento rechazado o sin responder el clic no genera ninguna petición a `googletagmanager.com`, y que el enlace sigue abriendo Telegram igualmente.
- [ ] 5.3 Anotar la fecha de puesta en producción para poder comparar el delta de suscriptores, y no modificar posición ni copy durante 3–4 semanas.
