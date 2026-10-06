## Context

La página es un único `index.html` estático, con Tailwind por CDN y sin build step, que envuelve un Google Sheet publicado. La marca del proyecto aparece en la imagen OG, en el favicon y en la cabecera de la tabla publicada. La página no la usa: todo es `slate`/`sky` por defecto.

Colores medidos sobre `og-image-v1.png`:

| Papel | Hex | Contraste |
|---|---|---|
| Navy (fondo de marca) | `#051322` | — |
| Lima (anillo, título) | `#98F13E` | 13,4:1 sobre navy · **1,4:1 sobre blanco** |

Restricción heredada de `widen-spreadsheet-embed`: el iframe se compone con `mix-blend-mode: multiply` para que el blanco opaco de Google tome el color de la página. Si la página es oscura, `multiply` vuelve oscuras las celdas blancas y el texto negro desaparece. **La zona de la tabla tiene que seguir siendo clara.**

Se valoraron tres direcciones con mockups en móvil (375×667) y escritorio (1280 px):

```
A · Banda de marca       B · Todo oscuro          C · Claro + acentos   ← elegida
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│▓ navy  cabecera ▓│     │▓ navy  cabecera ▓│     │ blanco, navy, 🤖 │
│▓ navy  Telegram ▓│     │▓ navy  Telegram ▓│     │ blanco  Telegram │
│━━━━━ lima ━━━━━━━│     │━━━━━ lima ━━━━━━━│     │━━━━━ lima ━━━━━━━│
│ claro · tabla    │     │▓ ┌────────────┐ ▓│     │ claro · tabla    │
│                  │     │▓ │ isla blanca│ ▓│     │                  │
│ tarjetas claras  │     │▓ tarjetas navy  ▓│     │ tarjetas claras  │
│▓ pie navy       ▓│     │▓ pie navy       ▓│     │ pie blanco       │
└──────────────────┘     └──────────────────┘     └──────────────────┘
```

## Goals / Non-Goals

**Goals**
- Que la página se reconozca como del mismo proyecto que el favicon, la imagen OG y la tabla.
- Que la franja de Telegram en móvil deje de ser un bloque suelto y gane margen respecto al límite de 220 px.
- Mantener intactos el contraste AA, el comportamiento del consentimiento y la composición del iframe.

**Non-Goals**
- Tema oscuro, total o parcial.
- Tipografía de marca (exigiría Google Fonts o alojar fuentes en el repo).
- Cambiar la tabla publicada, sus colores de fila o su tamaño.
- Rediseñar el banner de consentimiento o la política de cookies.
- Cambiar el texto de la cabecera, el título o los metadatos.

## Decisions

### Decisión 1 — Tema claro con acentos de marca (C)

Lo eligió el propietario tras ver los mockups. Además, es la única de las tres opciones que no obliga a tocar la composición del iframe ni a revisar el contraste de toda la página:

- **A** deja dos bandas navy (cabecera y pie) alrededor de una página clara.
- **B** obliga a quitar `multiply` y a meter la tabla en una «isla» blanca. Además hay que reescribir el contraste de las tarjetas, el diálogo y el pie.
- **C** solo cambia colores de primer plano: el fondo claro y `multiply` se quedan como están.

### Decisión 2 — El lima nunca es texto sobre fondo claro

Sobre blanco da 1,4:1. Solo se usa de tres formas:

1. Como **línea**, decorativa y sin texto: el borde inferior de la franja de Telegram.
2. Como **texto sobre navy**, con un contraste de 13,4:1: botones e indicadores de acción.
3. Como **relleno con texto navy encima**, con el mismo contraste. Ahora no se usa, pero queda permitido.

El navy hace de color «tinta» de la marca: títulos, botones y el enlace «Cookies». El texto corrido sigue en `slate`: el navy puro en párrafos largos cansa sin aportar nada.

Implementación: un `tailwind.config` en línea, justo después del script del CDN, con `brand.navy`, `brand.navy-hover` (`#16304F`) y `brand.lime`. Se descartan los valores arbitrarios (`bg-[#051322]`) repartidos por el HTML: con varios usos por color, un nombre evita erratas y deja un único sitio desde el que cambiar el tono.

### Decisión 3 — Logo reutilizando `apple-touch-icon-v1.png`

- **Por qué este fichero:** el icono para la pantalla de inicio del iPhone ya está publicado, mide 180×180 y es opaco. Recortado en círculo (`rounded-full`), muestra el robot con su anillo sobre un disco navy. No hace falta añadir imágenes, y se sigue cumpliendo que no haya en el repo imágenes cuadradas de más de 180 px. El favicon de 32 px se descarta: a 44 px se vería borroso en pantallas retina.
- **Accesibilidad:** el logo es decorativo (`alt=""`), porque el `<h1>` ya nombra el proyecto.
- **Disposición:** se usa una rejilla (*grid*) de dos columnas:
  - En **escritorio**, el logo (44 px) ocupa las dos filas, a la izquierda del título y de la descripción.
  - En **móvil**, el logo (36 px) solo acompaña al título y la descripción ocupa todo el ancho debajo. Así la descripción no pasa de 2 líneas, como exige la spec.
- **Medido en el mockup:** a 375 px el título ocupa 2 líneas y el borde superior de la tabla queda a **182 px**.

### Decisión 4 — Franja de Telegram: un solo enlace con forma de pastilla

```
Escritorio
┌────────────────────────────────────────────────────────────────────┐
│ 🔔 El bot de este tracker te avisa por Telegram si detecta…  (✈ Unirme) │
└━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┘
   toda la franja es el <a>; la pastilla es un <span>, no un control aparte

Móvil
┌───────────────────────────────┐
│ 🔔 El bot de este       ╭─────────╮
│ tracker te avisa por    │✈ Unirme │  44 px
│ Telegram si detecta…    ╰─────────╯
└━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┘
```

- **Toda la franja es el enlace** (`<a id="telegram-cta-link">`), en todos los tamaños. Se descartó tener dos enlaces que se alternan según el ancho (T2 en móvil, T3 en escritorio): duplica marcado y crea dos sitios donde medir el clic, sin ninguna diferencia visual. Con un único enlace, la zona pulsable en móvil pasa de 160×44 a unos 360×71 px.
- **La pastilla «✈ Unirme» es un `<span>`**, no un botón dentro del enlace: meter un control interactivo dentro de otro no es HTML válido. Es navy con texto lima. En móvil mide al menos 44 px de alto, para que se lea como botón aunque toda la franja responda al toque. El icono va con `aria-hidden`. El aviso «(se abre en una ventana nueva)» sigue en `sr-only`, como pide el requisito de accesibilidad.
- **Icono de avión de papel en lugar de flecha:** es la pastilla del mockup T2, que el propietario prefirió tras ver la implementación con «Unirme →». El avión dice «Telegram» antes de leer el texto. Es un glifo SVG en línea, dibujado aquí, y no el logotipo oficial de Telegram (avión dentro de un círculo azul): no se copia ningún recurso de marca ajeno, y hereda el lima de la pastilla con `currentColor`.
- **Fondo blanco con una línea lima de 3 px debajo.** Es la misma firma visual de la cabecera de la tabla (navy con línea lima), así que la franja sirve de puente entre la página y la hoja. Al pasar el ratón, el fondo pasa a `slate-50` y la pastilla a `brand.navy-hover`. Lleva un anillo de foco visible para la navegación con teclado.

### Decisión 5 — El texto de Telegram se acorta, sin perder garantías

> 🔔 El bot de este tracker te avisa por Telegram si detecta un producto en su **mínimo registrado**. Nada más.

Frente a las cuatro restricciones de la Decisión 5 de `add-telegram-channel-cta`:

| Restricción | Antes | Ahora |
|---|---|---|
| El canal es de este tracker | «El canal de Telegram de este tracker» | «El bot de este tracker … por Telegram» |
| Mínimo registrado, no histórico | ✓ | ✓ |
| No se promete exhaustividad | «cuando el bot detecta» | «si detecta» (condicional explícito) |
| Sin frecuencia, respuesta al miedo al spam | «Nada más.» | «Nada más.» (medido: 0 px extra en 375 y 1280) |

Se elimina el gancho «¿No quieres entrar cada día?». Con la pastilla al lado, cada palabra cuesta ancho en móvil. Y cuando toda la franja es un enlace, una pregunta retórica como comienzo del nombre accesible estorba a quien navega con lector de pantalla.

### Decisión 6 — Botones, títulos y pie

| Elemento | Antes | Después |
|---|---|---|
| «Proponer producto» | `slate-900`, `rounded-lg`, texto blanco | `brand.navy`, `rounded-full`, texto lima |
| «Cerrar» (política de cookies) | `slate-900`, texto blanco | `brand.navy`, texto lima |
| `<h1>` y `<h2>` de las tarjetas | `slate-900` | `brand.navy` |
| `<h3>` y texto corrido | `slate-800` / `slate-600` | sin cambios |
| «Cookies» en el pie | `slate-500` con subrayado | `brand.navy` con subrayado |

La forma de pastilla (`rounded-full`) es la misma para todos los botones de marca, para que los que llevan a una acción se reconozcan como una sola familia.

### Decisión 7 — Medición

`join_telegram` sigue en el mismo `id` y detrás del mismo control de consentimiento. Como cambian el texto, la forma y la zona pulsable, la fecha de despliegue se anota en este `design.md` como **nuevo punto de corte**. Las cifras de antes y después de esa fecha no se pueden comparar directamente.

## Risks / Trade-offs

- **[Toques accidentales]** Una franja entera pulsable recoge toques de quien solo intenta hacer scroll. → Riesgo bajo: la franja mide ~71 px y está por encima de la tabla, no encima. Si `join_telegram` se dispara de forma anómala, se puede limitar la zona pulsable a la pastilla sin cambiar el aspecto.
- **[Configuración del CDN y parpadeo]** El `tailwind.config` en línea debe ir justo después del script del CDN. Si no, la primera pintura sale sin los colores de marca. → Una tarea específica comprueba el orden.
- **[El navy de la tabla no es exactamente `#051322`]** La fila de cabecera la colorea el propietario en Sheets. → Se comprueba el color al verificar. Si difiere poco, se ignora; si se nota, se ajusta en la hoja, no en la página.
  **Medido el 2026-10-06** en el HTML publicado: la cabecera de la tabla usa `#0A192F` (marca: `#051322`) y su línea inferior `#8EE339` (marca: `#98F13E`). Ambos están en el mismo tono y solo varía ligeramente la luminosidad; a simple vista no se distinguen de la franja de la página. No se toca la página. Si se quiere la coincidencia exacta, se cambian los dos colores en la hoja.
- **[Se pierde el gancho]** Sin «¿No quieres entrar cada día?», la franja engancha menos. → Se acepta. Con el nuevo punto de corte de la Decisión 7, una caída de conversión se puede atribuir a este cambio.

## Open Questions

- El banner de consentimiento queda fuera del alcance, pero hay un desequilibrio previo: «Aceptar» es blanco y destaca más que «Rechazar» (`slate-700`). Las guías europeas sobre patrones engañosos piden que ambas opciones tengan el mismo peso visual. Conviene tratarlo en un cambio aparte, no mezclado con el de marca.
