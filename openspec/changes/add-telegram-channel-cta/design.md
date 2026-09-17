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
- **"cuando *detectamos*"** — no se firma exhaustividad. Si el bot falla o una tienda no se cubre, la página no ha mentido.
- **"Nada más."** — el bot dispara por evento, no por calendario, así que no se puede prometer un número de mensajes. La objeción real del visitante es el miedo al spam, y se responde con la única promesa que sí se puede cumplir: cero relleno. Es más creíble que un "únicamente" adverbial.

El botón dice "Unirme en Telegram" y no "NutriChollos": el nombre del canal lo verá al llegar; aquí lo que convierte es el verbo.

### Decisión 6 — Enlace saliente puro

`target="_blank"` con `rel="noopener noreferrer"`. Sin cookies, sin consentimiento, sin datos personales. La política de cookies no requiere ninguna modificación: no hay nada nuevo que declarar.

### Decisión 7 — Medición aceptada como parcial

Se añade un evento GA4 `join_telegram` en el clic, envuelto en la comprobación de consentimiento ya existente. **Subcontará**: solo dispara para quien aceptó cookies. Se acepta a sabiendas, porque una serie temporal parcial permite ver picos por día y correlacionarlos con publicaciones o menciones, cosa que el contador de suscriptores —un simple acumulado sin fecha— no permite.

Los enlaces de canal `t.me/<nombre>` no admiten parámetros de atribución; eso solo funciona con bots (`t.me/bot?start=web`). La cifra absoluta es el contador de suscriptores del canal, pero sin fuente. Las dos señales son complementarias y ninguna es suficiente sola.

Consecuencia operativa: fijar posición y copy, y no tocarlos durante 3–4 semanas. Con una señal tan ruidosa, iterar cada dos días no enseña nada.

### Decisión 8 — Tinte sky claro, subordinado al ámbar

La tira usa `bg-sky-50` con borde inferior `sky-200` y el botón sólido en `sky-600`/`slate-900`.

Se descarta el azul saturado de marca Telegram: convierte la tira en un banner publicitario reconocible y activa la ceguera de banner que la Decisión 2 intenta evitar —arrastrando consigo al disclaimer legal contiguo.

Se descarta el neutro puro (`bg-white`/`bg-slate-100`) porque, pegado a una cabecera blanca y a una tira ámbar, se lee como continuación de la cabecera y pierde la condición de bloque independiente.

`sky-50` es del mismo registro de saturación que `amber-50`, así que ninguna de las dos domina a la otra por intensidad: la jerarquía la marca el botón, que es el único elemento con color sólido en toda la región superior. Además el azul frente al ámbar separa semánticamente "información útil" de "aviso", que es exactamente la distinción que interesa.

## Risks / Trade-offs

- **+40px sobre el pliegue.** El escenario `Spreadsheet visible on a laptop viewport` (900px de alto) es el que este cambio puede romper. Es verificable y es el criterio de aceptación principal.
- **Sube el listón de responsabilidad percibida.** La página pasa de "aquí tienes datos, verifícalos" a "yo te aviso". Mitigado convirtiendo el condicional del copy en requisito normativo, no en preferencia de estilo.
- **Telegram sigue siendo audiencia alquilada.** No se exporta la lista de suscriptores y la plataforma puede cerrar el canal. El salto valioso que sí se captura es de "que recuerde volver" a "le suena el móvil". El día que se busque audiencia realmente propia, la conversión será Telegram → email, no web → email.
- **Competencia visual con el disclaimer legal.** Mitigado por la Decisión 2, pero conviene revisarlo con ojos frescos tras implementarlo.

## Open Questions

- **Qué hacer si la conversión resulta baja tras 3–4 semanas: aplazado a propósito.** Se consideró prefijar el orden del experimento (otro copy antes que subir a tarjeta) para no racionalizar el resultado a posteriori, y se decidió no hacerlo: sin ninguna línea base, cualquier umbral sería inventado. Queda registrado el riesgo conocido de decidir con los datos ya vistos.
- Si en algún momento se busca audiencia realmente propia, la conversión será Telegram → email, no web → email. Fuera del alcance de este cambio.
