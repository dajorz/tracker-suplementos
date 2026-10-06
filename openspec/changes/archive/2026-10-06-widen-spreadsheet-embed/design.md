## Context

`index.html` es un único fichero estático con Tailwind por CDN y sin paso de compilación. Hoy `<main>` lleva `max-w-4xl mx-auto px-4`, así que todas sus secciones, la del iframe incluida, quedan en 864 px de contenido, alineadas con el texto de la cabecera.

Medido el 2026-10-06 sobre la hoja publicada. Durante la exploración, el propietario acortó los nombres de producto (el más largo pasó de 72 a 47 caracteres), fijó «Producto» a 160 px y activó en esa columna el ajuste de línea:

```
 Tabla publicada: 1280 px (antes 1564 → 1445)
 ┌──────┬──────────┬──────┬────────┬───────┬───────┬──────────┬────────┬──────────────┐
 │Tienda│ Producto │Precio│Posición│€/100 g│Var.   │Valoración│Mínimo  │Disponibilidad│
 │  76  │ 160 wrap │  91  │  175   │  151  │  124  │   162    │  139   │     202      │
 └──────┴──────────┴──────┴────────┴───────┴───────┴──────────┴────────┴──────────────┘
 acumulado: 76 · 237 · 328 · 503 · 654 · 778 · 940 · 1079 · 1281

 móvil 360 (328 px)  ├──────┤                              Tienda + Producto + Precio
 hoy (864 px)        ├──────────────────┤                 hasta Variación
 portátil (~1218 px) ├──────────────────────────┤      hasta Mínimo registrado
 escritorio (1296 px)├───────────────────────────────┤ entera
```

Los anchos de columna los fija a mano el propietario de la hoja; el bot no los redimensiona. La versión publicada respeta el ajuste de línea (`white-space: normal`), y las filas crecen con el contenido: 24 filas de producto ocupan 2 líneas y 2 ocupan 3. El ancho de la tabla solo cambia cuando el propietario edita el formato.

En móvil, emulado a 360 y 384 px con la página propuesta, se ven Tienda, Producto y Precio. A 360 px el precio llega justo al borde del iframe (328 px).

Comportamiento medido de los parámetros de la URL `pubhtml`:

| Parámetro | Efecto observado |
|---|---|
| `chrome=false` | Desaparece la barra de título del documento (29 px) |
| `widget=true` | Mantiene la barra de pestañas inferior (26 px) y anida un segundo iframe |
| `widget=false` | Desaparece la barra de pestañas; la tabla se pinta directamente en el iframe |
| `headers=false` | Sin efecto visible hoy (los encabezados de fila miden 1 px) |
| `single=true&gid=0` | Fija la pestaña «Precios» (`gid=0`, confirmado en la URL interna) |

Fondo del documento publicado, medido con estilos computados: `body`, el contenedor de la tabla y cada celda sin relleno son `rgb(255, 255, 255)`. Quitar el relleno en la hoja no lo cambia: la celda sigue saliendo blanca.

## Goals / Non-Goals

**Goals:**
- Que en escritorios anchos se vea la tabla entera sin scroll horizontal.
- Que en un portátil se vea al menos hasta «Valoración», la columna que explica los colores.
- Quitar del embed la barra de título y la barra de pestañas.
- Que el ancho sobrante a la derecha de la tabla no se vea como una franja blanca.
- Mantener intactos el ancho de lectura y la alineación del resto de la página.

**Non-Goals:**
- Cambiar columnas, anchos o formato de la hoja de Google.
- Sustituir el iframe por una tabla propia.
- Cambiar el `loading="lazy"` del iframe.
- Rediseñar la tabla para móvil (tarjetas): por debajo de 896 px de ventana el ancho del iframe es idéntico, y la mejora en móvil se limita a quitar el doble scroll.

## Decisions

### 1. `<main>` pierde el ancho de lectura y cada bloque declara el suyo

`<main>` queda como `w-full pt-3 flex-1`, sin `max-w`, sin centrado y sin padding horizontal. Dentro:

```
<main w-full>
  <section mx-auto max-w-[83rem] px-4>    ← iframe, hasta 1296 px
  <div mx-auto max-w-4xl px-4>            ← 864 px, alineado con la cabecera
    <section> sugerencias
    <section> metodología
```

La sección del iframe sigue siendo la primera `<section>` hija de `<main>`, y la metodología sigue siendo la última `<section>` dentro de `<main>`. Ningún requisito de orden se ve afectado.

*Alternativas descartadas:*
- **Breakout a sangre completa** con `w-screen` y márgenes negativos: en Windows `100vw` incluye la barra de scroll vertical y provoca desbordamiento horizontal de la página.
- **Sacar el iframe de `<main>`**: rompe el requisito de que la tabla sea la primera sección de `<main>` y la deja fuera del landmark principal.
- **`max-w-4xl mx-auto` en cada sección de texto, sin envoltorio**: daría 896 px de ancho en vez de 864, y las tarjetas se desalinearían 16 px por lado respecto al texto de la cabecera.

### 2. Tope ajustado a la tabla: iframe de 1296 px (`max-w-[83rem]`)

`max-w-[83rem]` son 1328 px; con `px-4` dejan un iframe de 1296 px como máximo, que son los 1280 px de la tabla publicada más 16 px de la barra de scroll vertical interna. Así la barra queda pegada a la última columna y el bloque de la tabla aparece centrado en la página. Con scrollbars superpuestas (macOS, móvil) sobran solo 16 px, invisibles gracias a la decisión 4.

Esto es posible porque los anchos de columna los fija a mano el propietario de la hoja: el ancho de la tabla no cambia solo. Si el propietario ensancha columnas, debe actualizar este tope en el mismo momento.

*Alternativas descartadas:*
- **Sin tope**: en pantallas ultrapanorámicas la tabla queda alineada a la izquierda dentro de un iframe enorme, con la barra de scroll a más de 1000 px de la última columna.
- **`max-w-screen-2xl`** (iframe de 1504 px), elegido cuando la tabla medía 1445 px: con la tabla actual deja 208 px vacíos entre la última columna y la barra de scroll. El color se funde con la página, pero la barra suelta delata el hueco.
- **`max-w-[102rem]`** (iframe de 1600 px), calculado para la tabla original de 1564 px: hueco aún mayor.

### 3. URL del embed: `?gid=0&single=true&widget=false&headers=false&chrome=false`

En la exploración se planteó `widget=true`. La medición lo contradice: `widget=true` conserva la barra de pestañas, y `widget=false` es lo que la quita. `headers=false` no cambia nada hoy, pero se mantiene para que activar los encabezados en la hoja no los traiga al embed. `single=true&gid=0` asegura que se muestra «Precios» aunque se añadan o reordenen pestañas.

*Alternativa descartada:* **`range=`**. Fija un número de filas y dejaría fuera productos nuevos sin aviso. Choca con la regla del proyecto de no escribir a mano valores que cambian solos.

### 4. Sin tarjeta y con `mix-blend-multiply`: el blanco de Google toma el color de la página

El iframe pierde `rounded-xl` y `shadow-lg` y gana `mix-blend-multiply`. Al multiplicar, el blanco por el fondo da el fondo: el blanco del documento, incluido el margen a la derecha de la tabla, se ve del color de la página (`slate-50`). La cabecera azul marino de la tabla, el texto y los colores de fila apenas cambian, porque `slate-50` (#f8fafc) los atenúa menos de un 3 %. Probado en Chrome con un fondo azul de prueba: el blanco y el margen se volvieron azules.

Sin sombra ni esquinas, la tabla queda directamente sobre la página. Su borde lo marcan las propias celdas y la fila de cabecera oscura, y el margen sobrante deja de existir visualmente.

*Alternativas descartadas:*
- **Dejar margen de ancho y confiar solo en `multiply` para ocultarlo**: el color se funde, pero la barra de scroll suelta delata el hueco. Por eso la decisión 2 ajusta el tope a la tabla.
- **`mix-blend-multiply` conservando la tarjeta**: con `slate-50` el cambio de color es casi imperceptible, y la sombra seguiría enmarcando una zona más ancha que la tabla.
- **Fondo transparente real**: imposible. El CSS de la página no alcanza un documento de otro origen, y Google pinta de blanco también las celdas sin relleno.

### 5. Alto fijo igual a la tabla: sin scroll vertical interno

Con `80vh` el iframe tenía su propio scroll vertical, y en móvil había que alternar entre desplazar la página y desplazar la tabla. El alto de la tabla publicada no depende del ancho de la ventana, porque las columnas tienen ancho fijo y el ajuste de línea parte siempre igual: mide 1174 px en móvil y en escritorio. El iframe pasa a `height: 1190px`, que son esos 1174 px más 16 de la barra de scroll horizontal cuando la ventana es más estrecha que la tabla.

Probado: sin desbordamiento vertical interno, el scroll vertical sobre la tabla mueve la página, y el scroll horizontal sigue moviendo la tabla.

El alto no se puede leer desde la página (documento de otro origen), así que es un valor mantenido a mano, igual que el tope de ancho. Cada producto nuevo suma unos 37 px (nombre en 2 líneas).

*Alternativas descartadas:*
- **Calcularlo con JavaScript desde el CSV publicado** (contar productos y líneas por nombre): sin mantenimiento, pero depende de la fuente, del ancho de «Producto» y del orden de las columnas, y se desajustaría en silencio si cambian.
- **Mantener `80vh`**: conserva el doble scroll en móvil.

## Risks / Trade-offs

- [Si el propietario ensancha columnas en la hoja por encima de 1280 px en total, vuelve el scroll horizontal en pantallas grandes] → El tope está anotado en un comentario junto a la sección del iframe en `index.html`. Ensanchar columnas y subir el tope van juntos; el peor caso es un scroll horizontal corto.
- [En un portátil de ~1250 px «Disponibilidad» queda cortada unos 60 px] → Aceptado. Todo lo demás, «Mínimo registrado» incluido, se ve entero.
- [La legibilidad en móvil depende de que «Producto» siga a 160 px con ajuste de línea en la hoja] → Es formato manual que el bot no toca. Si se ensancha la columna, en móvil dejaría de verse el precio.
- [Los parámetros `chrome`, `widget` y `single` no forman parte de un contrato documentado de Google] → Si dejan de funcionar, el embed vuelve a mostrar título y pestañas: el aspecto actual, sin pérdida de datos.
- [La tabla es más ancha que la cabecera y el texto, así que los bordes no coinciden en escritorio] → Aceptado. Es el patrón habitual para tablas de datos, y el texto conserva una línea de lectura cómoda.
- [Un navegador que no componga iframes con `mix-blend-mode` mostraría el blanco de Google] → Se degrada al aspecto actual sin tarjeta: tabla blanca sobre fondo casi blanco, sin pérdida de datos.
- [Si se añaden productos sin subir el alto, las últimas filas vuelven a quedar tras un scroll interno] → Degrada al comportamiento anterior, sin pérdida de datos. El comentario junto al iframe en `index.html` recuerda actualizar el alto.
- [Si se quitan productos sin bajar el alto, queda espacio vacío bajo la tabla] → Invisible gracias a `multiply`; solo aleja un poco la sección de sugerencias.
- [Al bajar por la página, la fila de títulos de columna desaparece de la vista] → Igual que antes dentro del iframe: la vista publicada no mantiene filas fijas.
- [Si en el futuro el fondo de la página pasa a un color saturado, `multiply` teñirá los colores de fila] → Revisar este punto en cualquier cambio de paleta.

## Migration Plan

Un único cambio en `index.html`, publicado con GitHub Pages. La vuelta atrás consiste en revertir el commit.

## Open Questions

Ninguna para este cambio. Queda fuera, como posible cambio posterior, una tabla propia generada desde el CSV publicado, con tarjetas en móvil, si el tráfico móvil lo justifica.
