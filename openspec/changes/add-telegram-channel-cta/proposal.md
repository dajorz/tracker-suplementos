## Why

La web es hoy un destino de un solo uso: alguien entra, comprueba un precio y se va. No existe ningún puente hacia un canal que permita volver a alcanzarle, así que cada visita depende de que el visitante recuerde la URL por su cuenta. El canal de Telegram del proyecto (`https://t.me/NutriChollos`) ya existe y ya emite avisos automáticos, pero la página no lo menciona en ningún sitio: la única forma de descubrirlo es conocerlo de antemano.

El sitio omitía deliberadamente las alertas de bajada de precio. El `design.md` de `add-pricing-tracker-landing-page` lo justificaba así: *"alerts would be a promise the page cannot keep without a mailing list or automation"*. Esa premisa ha caducado — la automatización existe. Lo que sigue vigente es la otra decisión, la de `compact-spreadsheet-first-header`: la página debe leerse como una herramienta envuelta alrededor de una hoja de cálculo, no como una landing que contiene una.

Hay además una restricción física que condiciona la solución: el iframe de Google Sheets captura la rueda del ratón, de modo que un visitante que hace scroll dentro de la tabla nunca desplaza la página. Todo lo que hay bajo el iframe —incluido el CTA de sugerencias— es zona muerta de facto. Un CTA que deba convertir tiene que estar por encima de la tabla.

## What Changes

- Añadir una tira de ancho completo, de una sola línea, entre el disclaimer de precios y el iframe, con enlace al canal de Telegram del proyecto.
- La tira lleva el único control accionable de toda la región superior (un botón), lo que la distingue por afordancia —no solo por color— del disclaimer ámbar, que sigue siendo texto pasivo de aviso.
- El disclaimer de precios conserva la precedencia: se mantiene inmediatamente bajo la cabecera, por encima de la tira de Telegram.
- Relajar el requisito `Spreadsheet leads the main content` para admitir esa única tira de una línea, manteniendo la prohibición de tarjetas o secciones CTA antes de la tabla.
- Acotar la prohibición de alertas del requisito `Product suggestion call to action` a esa sección concreta, en lugar de a toda la página. La sección de sugerencias sigue sin formulario, sin campo de email y sin ofrecer alertas.
- El copy cose las dos marcas ("el canal de Telegram de este tracker") en lugar de renombrar nada, habla de "mínimo registrado" en lugar de "mínimo histórico", redacta la detección en condicional ("cuando detectamos") y no promete ninguna frecuencia de mensajes.
- Añadir un evento GA4 `join_telegram` en el clic del botón, envuelto en la comprobación de consentimiento ya existente, de modo que no dispara para quien rechazó o no ha respondido.
- No se toca el iframe, ni el flujo de consentimiento, ni la política de cookies, ni el CTA `mailto:` ni el ensamblado anti-scraping de la dirección.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
- `pricing-tracker-landing-page`: se añade un requisito de llamada a la acción hacia el canal de Telegram; el requisito de orden del contenido principal pasa a admitir una única tira de una línea sobre la tabla; el requisito de la CTA de sugerencias acota su prohibición de alertas a su propia sección.

## Impact

- Ficheros afectados: `index.html` únicamente (un bloque nuevo entre el disclaimer y `<main>`).
- Coste vertical: aproximadamente +40px por encima del pliegue. El borde superior de la tabla debe seguir siendo visible en un viewport de 900px de alto; es el escenario que este cambio puede romper y el que hay que verificar.
- Sin impacto en privacidad: es un enlace saliente. No instala cookies, no requiere consentimiento y no envía datos personales. La política de cookies no cambia.
- Sin dependencias nuevas: sigue siendo un único fichero estático con el CDN de Tailwind.
- Limitación de medición asumida: los enlaces de canal de Telegram no admiten parámetros de atribución (eso solo funciona con bots, `t.me/bot?start=web`). El evento `join_telegram` de GA4 solo dispara para quien aceptó cookies, por lo que subcontará; el contador de suscriptores del canal es la única cifra absoluta, pero sin fuente.
- Riesgo de postura: al prometer avisos, la página sube su propio listón de responsabilidad percibida frente al actual "las cifras pueden estar desactualizadas". Por eso el copy es condicional por requisito, no por estilo.
