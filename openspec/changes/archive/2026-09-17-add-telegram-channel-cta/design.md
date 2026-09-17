## Context

La página es un fichero estático único que envuelve un Google Sheet publicado. Su orden vertical actual es: cabecera compacta (~64px) → tira ámbar de disclaimer (~32px) → padding (24px) → iframe (`80vh`, `min-height: 600px`).

Dos hechos condicionan cualquier decisión sobre dónde colocar un CTA:

1. **El iframe se come el scroll.** `pubhtml` tiene su propia barra de desplazamiento. Quien hace scroll con la rueda sobre la tabla desplaza la hoja, no la página. Por tanto el CTA de sugerencias que hoy vive bajo el iframe recibe una fracción pequeña de las impresiones, y cualquier CTA nuevo colocado ahí heredaría el mismo problema.
2. **La página tiene una identidad ya decidida.** `compact-spreadsheet-first-header` eliminó el hero y movió la tarjeta CTA de arriba a abajo precisamente para que la página "read as a tool wrapped around a spreadsheet". Su `design.md` declara además que el tinte ámbar del disclaimer *"is the one element allowed to draw attention away from the sheet"*.

El objetivo de negocio es convertir tráfico de paso en suscriptores del canal de Telegram, que ya existe y ya emite avisos automáticos por evento.

## Goals / Non-Goals

**Goals**
- Que todo visitante vea la existencia del canal sin hacer scroll.
- Que entienda *por qué* debería unirse, no solo que existe.
- Que entienda que el canal pertenece a este mismo proyecto.
- Coste vertical mínimo y cero impacto en el flujo de consentimiento.

**Non-Goals**
- Construir una lista de correo o cualquier captación de datos personales.
- Atribuir suscripciones a la fuente de tráfico (técnicamente imposible con enlaces de canal).
- Renombrar el sitio o el canal para unificar marca.
- Cambiar el iframe, su tamaño, el consentimiento o la política de cookies.

## Decisions

### Decisión 1 — Tira slim sobre la tabla, no tarjeta

Se consideraron cinco posiciones:

| | Coste vertical | Impresiones | "Sabe a landing" | Problema |
|---|---|---|---|---|
| **A** tira slim sobre la tabla | ~40px | 100% | bajo | toca `spreadsheet-leads` |
| **B** tarjeta sobre la tabla | ~110px | 100% | **alto** | revierte `compact-spreadsheet-first-header` |
| **C** dentro de la cabecera | 0px | 100% | muy bajo | no cabe el "por qué"; toca `compact-header` |
| **D** sticky inferior | 0px | 100% | medio | solapa con el banner de cookies |
| **E** junto al CTA de abajo | 0px | **~15%** | ninguno | no lo ve nadie |

Se elige **A**.

**Frente a B** (la propuesta original): una tarjeta con titular, subtítulo y botón centrado es literalmente el bloque que `compact-spreadsheet-first-header` eliminó. Reintroducirlo revierte esa decisión entera en lugar de matizarla. A consigue las mismas impresiones —mismo sitio, sobre el pliegue— por un tercio del coste vertical. Subir de tira a tarjeta más adelante es trivial si la conversión no convence; bajar de tarjeta a tira una vez acostumbrado a los números, no.

**Frente a C**: coste cero, pero en una barra de título solo cabe la palabra "Telegram", no el motivo para unirse. El motivo es todo el trabajo de conversión. Además obligaría a modificar `Compact header bar`, que fija "exactamente dos filas de texto en desktop".

**Frente a D y F (flotante)**: el banner de consentimiento es `fixed bottom-0 inset-x-0`. En la primera visita —justo cuando el visitante es nuevo y más interesa captarlo— ambos elementos se solaparían. Resolverlo exige coordinar estado entre dos componentes fijos: código real a cambio de un problema que A no tiene.

**Frente a E**: cumple toda la spec vigente sin tocar nada, y por el scroll interno del iframe no lo vería prácticamente nadie.

**Medido tras implementar:** la tira ocupa 48px y deja el borde superior del iframe a 178px en un viewport de 900px. La copy acordada necesita 810px y solo hay 714px junto al botón, así que ocupa dos líneas en desktop. Se acepta: las tres variantes que caben en una línea sacrificaban o el gancho inicial o la promesa «Nada más.», y ambos son carga útil (Decisión 5). "Tira slim" pasa a significar **como máximo dos líneas de texto**, no exactamente una.

### Decisión 2 — Distinguir por afordancia, no por color

Dos tiras de color apiladas invitan a la ceguera de banner, y la que perdería sería la legal. La distinción no se delega en el tinte: **la tira de Telegram lleva el único control accionable de toda la región superior**. El ojo separa "leer" de "pulsar" mucho antes que separa ámbar de índigo.

### Decisión 3 — El disclaimer legal va primero

Orden: cabecera → disclaimer ámbar → tira de Telegram → tabla.

Ambos órdenes satisfarían literalmente el requisito vigente ("entre la cabecera y la hoja"), pero se elige este por dos razones: precedencia del aviso legal, y porque deja el CTA pegado a la tabla, en el último punto del recorrido de lectura antes del dato.

```
┌────────────────────────────────────────────────┐
│ HEADER   Tracker de Precios de Suplementación  │  ~64px
├────────────────────────────────────────────────┤
│ ⚠  Registro no oficial y ajeno a las tiendas…  │  ~32px  ámbar · AVISO (pasivo)
├────────────────────────────────────────────────┤
│ 🔔  …te avisa cuando…        [ Unirme ➔ ]      │  ~40px  índigo · ACCIÓN (botón)
├────────────────────────────────────────────────┤
│                    TABLA                       │
└────────────────────────────────────────────────┘
```

### Decisión 4 — La prohibición de alertas caducó, no se revoca por capricho

`Product suggestion call to action` dice *"SHALL NOT offer price-drop alerts"*. Su razón está registrada: *"alerts would be a promise the page cannot keep without a mailing list or automation"*. La automatización ya existe, luego la condición que sostenía la prohibición dejó de cumplirse.

La prohibición **no se elimina**: se acota a su propia sección. La CTA de sugerencias sigue sin formulario, sin campo de email y sin ofrecer alertas — ese bloque sigue siendo exclusivamente para feedback de cobertura.

### Decisión 5 — El copy asume cuatro restricciones, cada una por un motivo concreto

> 🔔 **¿No quieres entrar cada día?** El canal de Telegram de este tracker te avisa cuando detectamos que un producto toca su mínimo registrado. Nada más. → **[ Unirme en Telegram ]**

- **"el canal de Telegram *de este tracker*"** — la web se llama "Tracker de Precios de Suplementación" y el canal "NutriChollos". Son dos registros distintos, y "chollos" tiene olor a caza de ofertas frente al tono sobrio del disclaimer. Se cosen en el copy en lugar de renombrar: más barato y reversible.
- **"mínimo *registrado*"** — el histórico tiene menos de 6 meses. "Mínimo histórico" sería una promesa que hoy no se sostiene. El matiz refuerza la voz de honestidad que ya establece el disclaimer.
- **"cuando *el bot* detecta"** — no se firma exhaustividad: un bot es explícitamente un mecanismo falible, así que si falla o una tienda no se cubre, la página no ha mentido. Se descartó "cuando detectamos" por dos motivos. Era el único "nosotros" de toda la página —el aviso y la cabecera son impersonales y el CTA de sugerencias habla en primera persona del singular ("Escríbeme y lo añado")—, y un proyecto de una persona que dice "detectamos" suena a plural corporativo inventado. Además nombrar al bot refuerza "Nada más.": si quien decide es un proceso por evento, no hay nadie seleccionando qué publicar. De paso es más preciso: el bot detecta, el canal publica.
- **"Nada más."** — el bot dispara por evento, no por calendario, así que no se puede prometer un número de mensajes. La objeción real del visitante es el miedo al spam, y se responde con la única promesa que sí se puede cumplir: cero relleno. Es más creíble que un "únicamente" adverbial.

El botón dice "Unirme en Telegram" y no "NutriChollos": el nombre del canal lo verá al llegar; aquí lo que convierte es el verbo.

### Decisión 6 — Enlace saliente puro

`target="_blank"` con `rel="noopener noreferrer"`. Sin cookies, sin consentimiento, sin datos personales. La política de cookies no requiere ninguna modificación: no hay nada nuevo que declarar.

### Decisión 7 — Medición aceptada como parcial

Se añade un evento GA4 `join_telegram` en el clic, envuelto en la comprobación de consentimiento ya existente. **Subcontará**: solo dispara para quien aceptó cookies. Se acepta a sabiendas, porque una serie temporal parcial permite ver picos por día y correlacionarlos con publicaciones o menciones, cosa que el contador de suscriptores —un simple acumulado sin fecha— no permite.

Los enlaces de canal `t.me/<nombre>` no admiten parámetros de atribución; eso solo funciona con bots (`t.me/bot?start=web`). La cifra absoluta es el contador de suscriptores del canal, pero sin fuente. Las dos señales son complementarias y ninguna es suficiente sola.

Consecuencia operativa: fijar posición y copy, y no tocarlos durante 3–4 semanas. Con una señal tan ruidosa, iterar cada dos días no enseña nada.

**Línea base: 2026-09-17.** Fecha de referencia para comparar el delta de suscriptores del canal. Antes del 2026-10-15 no hay datos suficientes para concluir nada sobre la conversión.

### Decisión 8 — Tinte sky claro, subordinado al ámbar

La tira usa `bg-sky-50` y el botón sólido en `sky-600`. Sin `border-b`: el cambio de tono contra el aviso (`slate-50`) y contra el área de contenido ya marca los bordes, y una regla más solo añade ruido a una zona que acumula tres bandas en 177px.

Se descarta el azul saturado de marca Telegram: convierte la tira en un banner publicitario reconocible y activa la ceguera de banner que la Decisión 2 intenta evitar —arrastrando consigo al disclaimer legal contiguo.

Se descarta el neutro puro (`bg-white`/`bg-slate-100`) porque, pegado a una cabecera blanca y a una tira ámbar, se lee como continuación de la cabecera y pierde la condición de bloque independiente.

`sky-50` es del mismo registro de saturación que `amber-50`, así que ninguna de las dos domina a la otra por intensidad: la jerarquía la marca el botón, que es el único elemento con color sólido en toda la región superior. Además el azul frente al ámbar separa semánticamente "información útil" de "aviso", que es exactamente la distinción que interesa.

### Decisión 9 — El aviso de precios se neutraliza a `slate`

Pasa de `amber-50`/`amber-200`/`amber-900` con icono ⚠ a `slate-50`/`slate-200`/`slate-600` sin icono. El texto, la posición y la imposibilidad de ocultarlo no cambian.

El `design.md` de `compact-spreadsheet-first-header` justificaba el ámbar así: *"it is the one element allowed to draw attention away from the sheet"*. Esa premisa la invalida este mismo cambio al introducir una segunda banda de color. Es el mismo patrón que la prohibición de alertas: la decisión no era incorrecta, la condición que la sostenía dejó de cumplirse.

El ⚠ es el peor infractor: convierte una nota de letra pequeña en un estado de error, y la página no está en error. Sin ámbar y sin triángulo, el aviso se lee como lo que es —letra pequeña de la cabecera— y la tira de Telegram queda como el único punto de color sobre la tabla, que es el objetivo.

Esto **refuerza** la Decisión 2 en lugar de contradecirla: si solo hay una banda de color y un solo control accionable, la distinción por afordancia ya no tiene que competir con nada. Deja obsoleto el argumento de la Decisión 8 sobre "mismo registro de saturación que `amber-50`", pero no la elección de `sky`: sigue siendo preferible al azul saturado de marca por el mismo motivo de ceguera de banner.

**No se toca el texto legal.** Ya cabe en una línea a 1280px, así que acortarlo no ahorra altura, y reescribir una cláusula de exención sin ganancia visual es riesgo sin contrapartida.

**Reglas horizontales.** Con tres bandas apiladas, los `border-b` del aviso y de la tira de Telegram eran redundantes: el cambio de tono ya separa. Se retiran ambos y queda una sola regla sobre la tabla, la de la cabecera, que el requisito `Compact header bar` exige expresamente.

**Postura legal.** Lo que sostiene el aviso es que sea persistente, esté sobre los datos, sea legible y no se pueda ocultar — todo eso se mantiene. `slate-600` sobre `slate-50` da ~7:1 de contraste, holgadamente por encima del 4.5:1 que exige WCAG AA a este tamaño. Lo arriesgado sería moverlo al pie o meterlo tras un desplegable, y eso la spec ya lo prohíbe.

### Decisión 10 — Se corrigen los defectos de accesibilidad heredados

Auditar la maqueta destapó fallos de WCAG AA, uno introducido por este cambio y el resto preexistentes. Se arreglan todos aquí, aunque ensanche el alcance, porque el coste es de minutos y dejarlos documentados sin corregir solo garantiza que nadie vuelva.

| | Antes | Después |
|---|---|---|
| Botón de Telegram (`sky-600`) | 4.1:1 | **5.93:1** (`sky-700`) |
| Crédito de Reddit (`slate-400`) | 2.56:1 | **4.76:1** (`slate-500`) |
| Control «Cookies» (`slate-400`) | 2.56:1 | **4.76:1** (`slate-500`) |
| Área táctil de «Cookies» | 42×16 | **42×24** |
| Área táctil de «Más información» | 103×20 | **103×28** |
| Bloques fuera de landmark | 4 | **0** |

El del botón era un defecto propio: `sky-600` con texto blanco no llega a 4.5:1 a 12px. `sky-700` sí.

Los dos de `slate-400` importan más de lo que parece porque uno de ellos es **el control de retirada de consentimiento**. Un control de privacidad que la gente no puede leer es un control que no existe. Eso obliga a modificar el requisito `Consent can be changed or withdrawn`, que pedía literalmente *"small, low-contrast text"*: pasa a *"muted"* con umbral AA explícito. Discreto y ilegible no son sinónimos, y la spec los estaba confundiendo.

Los landmarks se resuelven con elementos semánticos en vez de `div`: `<aside>` para las dos tiras, `<footer>` para la línea de cookies y `role="region"` con nombre accesible para el banner. Recuperar `<footer>` no revierte nada: `compact-spreadsheet-first-header` eliminó el *contenido* del pie (crédito de autor, enlace a GitHub), no prohibió el elemento.

Se añade además un requisito `Accessible text and controls` en lugar de arreglar y olvidar, para que la auditoría quede como criterio verificable y no como anécdota de una conversación.

## Risks / Trade-offs

- **+48px sobre el pliegue.** El escenario `Spreadsheet visible on a laptop viewport` (900px de alto) es el que este cambio puede romper. Es verificable y es el criterio de aceptación principal.
- **Sube el listón de responsabilidad percibida.** La página pasa de "aquí tienes datos, verifícalos" a "yo te aviso". Mitigado convirtiendo el condicional del copy en requisito normativo, no en preferencia de estilo.
- **Telegram sigue siendo audiencia alquilada.** No se exporta la lista de suscriptores y la plataforma puede cerrar el canal. El salto valioso que sí se captura es de "que recuerde volver" a "le suena el móvil". El día que se busque audiencia realmente propia, la conversión será Telegram → email, no web → email.
- **Competencia visual con el disclaimer legal.** Detectada al revisar la maqueta y resuelta en la Decisión 9: el aviso pasa a tinte neutro y la tira de Telegram queda como único bloque con color.

## Open Questions

- **Qué hacer si la conversión resulta baja tras 3–4 semanas: aplazado a propósito.** Se consideró prefijar el orden del experimento (otro copy antes que subir a tarjeta) para no racionalizar el resultado a posteriori, y se decidió no hacerlo: sin ninguna línea base, cualquier umbral sería inventado. Queda registrado el riesgo conocido de decidir con los datos ya vistos.
- Si en algún momento se busca audiencia realmente propia, la conversión será Telegram → email, no web → email. Fuera del alcance de este cambio.
