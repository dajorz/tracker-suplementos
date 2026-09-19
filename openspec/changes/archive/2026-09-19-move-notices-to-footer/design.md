## Context

`index.html` apila cuatro bloques antes del iframe: `<header>`, una banda de aviso de precios, una tira de Telegram y el `padding-top` de `<main>`. Medidos en el navegador con el fichero actual:

```
                        Desktop (1280)      Móvil (375)
  <header>                  73px               101px
  aside "Aviso"             32px                64px
  aside "Telegram"          48px               104px
  main pt-6                 24px                24px
  ───────────────────────────────────────────────────
  Top del iframe           177px               293px
```

Tres restricciones condicionan cualquier solución:

1. **El iframe captura la rueda del ratón.** Decisión ya registrada en `add-telegram-channel-cta`: todo lo que queda bajo el iframe es zona muerta de facto para la conversión. No lo es para contenido legal de referencia, al que se llega por teclado, por lectores de pantalla o desplazando fuera del iframe.
2. **El párrafo de la cabecera rompe línea en móvil alrededor de los 115 caracteres.** Medido: 155 caracteres → 3 líneas (48px); 114 caracteres → 2 líneas (32px); 51 caracteres → 1 línea (16px). El crédito a Reddit son 41 caracteres, justo los que separan las tres líneas de las dos.
3. **El fichero es estático y de un solo archivo.** No hay build, ni CSS propio, ni tests automatizados: toda verificación es manual o instrumentada en el navegador.

El detonante fue una observación cualitativa ("la parte de arriba es demasiado alta") que la medición reordenó: el crédito a Reddit, sospechoso inicial, cuesta 16px en móvil y 0px en desktop.

## Goals / Non-Goals

**Goals:**

- Reducir la distancia hasta el borde superior de la tabla en al menos un 25% en móvil y un 15% en desktop.
- No degradar la conversión: el botón de Telegram debe quedar más arriba en pantalla y ser más fácil de pulsar que antes.
- Conservar íntegro el texto del aviso de exactitud de precios. Se reubica; no se acorta ni se reescribe.
- Mantener el fichero único, sin build ni dependencias nuevas.

**Non-Goals:**

- No se rediseña el copy de la tira de Telegram ni su promesa de avisos.
- No se toca el flujo de consentimiento, la política de cookies ni la integración GA4.
- No se instrumentan impresiones del CTA (`view_telegram_cta`). Se discutió como spike para poder calcular una tasa de conversión real en lugar de solo clics, pero queda fuera de alcance.
- No se altera la altura del iframe (`80vh`, mínimo 600px) ni se sustituye el embed.
- No se reordena el contenido bajo la tabla: la tarjeta "¿Echas en falta algún producto?" se queda donde está.

## Decisions

### 1. El aviso se reubica al pie, no se fusiona en la cabecera

Se evaluaron tres variantes, todas medidas en el navegador sobre el DOM real:

| Variante | Top tabla móvil | Aviso sobre el fold | Coste añadido |
|---|---:|---|---|
| Aviso al pie (elegida) | 213px | No | — |
| Aviso fusionado en la línea de descripción | 213px | Sí, como cláusula | Desacoplar `meta description` de la frase visible |
| Híbrida: coletilla arriba + texto completo al pie | ~213px | Sí, resumida | Dos avisos que mantener sincronizados |

Las tres dan el mismo ahorro, porque el cuello de botella es el salto de línea del párrafo, no el número de caracteres. La elección se decide, por tanto, en otro eje.

Se descarta la fusión porque un aviso legal incrustado tras un separador `·`, en gris de 12px bajo el título, se lee como copy de marketing y no como advertencia: conserva la presencia y pierde la función. Es peor que no tenerlo arriba, porque aparenta cumplir. Se prefiere que el aviso deje de verse de forma explícita y asumida, antes que degradarlo manteniendo la apariencia de prominencia.

Se descarta la híbrida por introducir dos copias del mismo texto legal en un fichero sin tests: la deriva entre ambas es cuestión de tiempo.

### 2. El crédito a Reddit se mueve por presupuesto de línea, no por altura

Consecuencia directa de la restricción 2. Con la descripción actual (114 caracteres sin el crédito) el párrafo ocupa dos líneas; con el crédito, tres. Como el crédito no aporta nada en desktop y 16px en móvil, mantenerlo obligaría a conservar la tercera línea para una prueba social que un visitante procedente de Reddit ya conoce.

Efecto secundario favorable: la frase de descripción de la cabecera pasa a coincidir *exactamente* con `meta description`, `og:description` y `twitter:description`. La salvedad "excluyendo el crédito a la comunidad" del requisito de metadatos queda sin objeto y se retira. Las cadenas del `<head>` no se tocan.

### 3. El aviso se aloja en el `<footer>` existente, no en un bloque nuevo bajo la tabla

Ubicarlo inmediatamente bajo el iframe lo dejaría tras 600px de tabla con scroll propio: invisible en la práctica y, además, huérfano de landmark. El `<footer>` ya es un landmark, ya aloja el control «Cookies» y es la convención establecida para texto legal. Se reutiliza.

Orden dentro del pie: aviso completo primero, después crédito y «Cookies» en una segunda línea. El aviso encabeza porque es el único contenido con función legal.

### 4. El espacio liberado se reparte entre tabla y botón

De los 80px ganados en móvil, unos 12 se devuelven al botón de Telegram (32px → 44px) y el resto va a la tabla. El botón es el único elemento de conversión de la página y estaba por debajo del mínimo recomendado para un objetivo táctil primario, pese a que su tira ocupaba 104px: solo el 31% correspondía al control, el resto era copy y espaciado. Subirlo no compite con el objetivo de reducir altura porque la tira pierde su banda precedente.

La alternativa de recortar el copy de la tira para ganar más altura se descarta: el texto actual está redactado para acotar la promesa de avisos ("mínimo registrado", detección atribuida al bot, sin frecuencia) y recortarlo arriesga esa precisión por unos pocos píxeles.

### 5. `pt-6` → `pt-3` en `<main>`

12px gratis en ambos viewports. Con la banda de aviso eliminada, la tira de Telegram y la tabla ya no necesitan tanta separación para leerse como bloques distintos: el borde inferior de la tira y el `shadow` del iframe bastan.

## Risks / Trade-offs

**[El visitante puede consultar precios sin haber leído nunca el aviso de no oficialidad]** → Es el riesgo central y no se elimina, solo se acota. Mitigación: el texto se conserva íntegro, sin acortar, en un landmark con contraste conforme a AA y sin ninguna interacción que lo oculte. La cabecera sigue diciendo "Proyecto independiente", que es el mismo mensaje en versión débil. Si el proyecto crece en tráfico o añade enlaces de afiliación, esta decisión debe revisarse: el listón de responsabilidad sube con ambas cosas.

**[Un aviso en el pie puede interpretarse como menos prominente de lo exigible]** → Aceptado conscientemente. No hay obligación normativa de banda fija para un registro de precios no comercial, pero la decisión se documenta aquí para que una revisión futura sepa que fue deliberada y no un descuido.

**[El `<footer>` crece de 41px a 85px en móvil]** → Sin impacto: queda bajo el fold y bajo la tarjeta de sugerencias. Se registra para que no sorprenda en la verificación.

**[Regresión en escenarios de viewport ya verificados]** → Tres escenarios existentes miden alturas concretas: "Spreadsheet visible on a laptop viewport" (900px), "Mobile layout preserves the spreadsheet" (375×667) y "Controls are large enough to hit". El cambio los mejora en teoría, pero el botón a 44px y el pie reestructurado obligan a volver a medirlos. Mitigación: son tareas explícitas de verificación, no comprobaciones a ojo.

**[Sin datos de conversión para validar el reparto de espacio]** → La página emite `join_telegram` pero no mide impresiones del CTA, así que no existe una tasa de conversión con la que comparar antes y después. La decisión 4 descansa en heurística de tamaño de objetivo táctil, no en evidencia propia. Mitigación posible fuera de este cambio: instrumentar impresiones con `IntersectionObserver` reutilizando la puerta de consentimiento existente.

**[Reversión]** → El cambio afecta a un solo fichero estático servido por GitHub Pages. Revertir es un `git revert` y un push; no hay migración de datos, ni estado persistido, ni caché que invalidar más allá de la del navegador.
