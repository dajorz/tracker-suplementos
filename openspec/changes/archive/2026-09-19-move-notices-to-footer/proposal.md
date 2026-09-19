## Why

La página existe para que alguien vea una tabla de precios. Hoy, en un móvil de 375×667, hay **293px de preámbulo** antes del borde superior de la tabla: cabecera (101px), banda del aviso de precios (64px), tira de Telegram (104px) y el `pt-6` de `<main>` (24px). Con el banner de consentimiento en pantalla (148px más), al visitante le quedan **116px de tabla visibles en el primer pintado**. El contenido útil ocupa menos de un tercio de lo que ocupan los avisos que lo preceden.

El coste está mal repartido. Medido en el navegador, dos bandas consecutivas dicen cosas que se solapan —la cabecera afirma "Proyecto independiente" y la banda de debajo "Registro de precios no oficial y ajeno a las tiendas"— y entre las dos se llevan 165px. Mientras tanto, el botón de Telegram, que es el único elemento de conversión de la página, mide 32px de alto con texto de 12px: por debajo del mínimo recomendado para un objetivo táctil primario, y solo el 31% de la altura de su propia tira.

El crédito a Reddit, por su parte, resulta ser un falso culpable: cuesta 16px en móvil y 0px en desktop. Se mueve no por lo que ocupa, sino porque libera el presupuesto de línea que necesita la descripción para caber en dos filas en lugar de tres.

## What Changes

- Retirar la banda `<aside aria-label="Aviso sobre la exactitud de los precios">` de la región entre la cabecera y la tabla. El texto del aviso no se acorta ni se edita: se reubica íntegro en el `<footer>`.
- Retirar el crédito "· Con feedback de la comunidad de Reddit" de la línea de descripción de la cabecera y reubicarlo también en el `<footer>`.
- Reestructurar el `<footer>` para alojar, en este orden: el aviso completo de exactitud de precios, el crédito a la comunidad de Reddit y el control «Cookies» ya existente.
- La tira de Telegram pasa a situarse inmediatamente bajo la cabecera, al desaparecer la banda que la precedía. Sigue siendo el único bloque con color y el único control accionable sobre la tabla.
- Elevar el botón "Unirme en Telegram" de 32px a un mínimo de 44px de alto, aprovechando el espacio liberado. Su copy, su destino, su `rel` y su evento GA4 no cambian.
- Reducir el `pt-6` de `<main>` a `pt-3`.
- **BREAKING (a nivel de spec)**: el aviso de precios deja de ser legible antes que los precios. Pasa de banda persistente sobre la tabla a texto de pie de página. Sigue sin ser descartable, plegable ni oculto tras una interacción, pero un visitante puede consultar un precio sin haberlo leído. Es el riesgo central de este cambio y se asume de forma explícita, no como efecto colateral.
- No se toca el iframe, ni el flujo de consentimiento, ni la política de cookies, ni el CTA `mailto:` ni el ensamblado anti-scraping de la dirección. Las cadenas `meta description`, `og:description` y `twitter:description` tampoco cambian: siguen coincidiendo con la frase de descripción de la cabecera, que es exactamente la misma una vez retirado el crédito.

### Efecto medido

| | Móvil (375) | Desktop (1280) |
|---|---:|---:|
| Top de la tabla, antes | 293px | 177px |
| Top de la tabla, después | 213px | 145px |
| Reducción | **−27%** | **−18%** |
| Tabla visible en el primer pintado (móvil) | 116px → 196px | — |
| Altura del `<footer>` | 41px → 85px | 41px → 53px |

El desktop no gana nada por mover el crédito a Reddit (la línea entra completa en ambos casos); todo su ahorro procede de eliminar la banda del aviso.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
- `pricing-tracker-landing-page`: el requisito del aviso de exactitud de precios cambia su ubicación de banda sobre la tabla a texto de pie de página; el requisito de la cabecera compacta deja de incluir el crédito a la comunidad en su línea de descripción; el requisito de la CTA de Telegram pierde el escenario de precedencia del aviso legal, pasa a situarse inmediatamente bajo la cabecera y añade una altura mínima de 44px para su botón; el requisito de orden del contenido principal deja de mencionar bandas de aviso sobre la tabla; el requisito de accesibilidad de texto y controles traslada la comprobación de contraste del crédito desde la cabecera al pie; el requisito de retirada de consentimiento admite que el control «Cookies» comparta el pie con otros textos en lugar de ocupar una única línea; el requisito de metadatos SEO retira la salvedad "excluyendo el crédito a la comunidad", que queda sin objeto.

## Impact

- Ficheros afectados: `index.html` únicamente. Un bloque `<aside>` eliminado, un `<footer>` reescrito y tres ajustes de clases Tailwind.
- Siete requisitos tocados en `openspec/specs/pricing-tracker-landing-page/spec.md`. Ninguno se elimina; todos se reescriben.
- Riesgo legal y de confianza: es el punto delicado. Un registro de precios no oficial que no advierte de serlo antes de mostrar cifras depende de que el visitante llegue al pie. Se mitiga manteniendo el texto íntegro, sin acortar, y en un pie con contraste conforme a AA — pero no se elimina.
- Sin impacto en privacidad, consentimiento, cookies ni analítica. No se añaden dependencias: sigue siendo un fichero estático con el CDN de Tailwind.
- Escenarios de verificación que este cambio puede romper: "Spreadsheet visible on a laptop viewport" (900px de alto), "Mobile layout preserves the spreadsheet" (375×667) y "Controls are large enough to hit". Los tres deben volver a medirse tras la implementación.
- Alternativa descartada: fusionar el aviso dentro de la propia línea de descripción de la cabecera. Daba exactamente el mismo ahorro (213px en móvil) y conservaba el aviso sobre el fold, pero lo degradaba a cláusula tras un separador, con apariencia de copy de marketing, y obligaba a desacoplar `meta description` de la frase visible. Se prefirió la reubicación al pie por ser más honesta: el aviso deja de verse, en lugar de aparentar que se ve.
