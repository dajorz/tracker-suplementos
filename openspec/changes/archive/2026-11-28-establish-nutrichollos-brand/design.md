## Context

La marca del proyecto en redes es "NutriChollos". La web se presentaba con otro nombre ("Tracker de Precios de Suplementación"), una decisión tomada en `add-telegram-channel-cta` para separar el tono sobrio de la web del de "chollos". Desde entonces ha aparecido un perfil de TikTok ajeno, `@nutrichollos`, que Google ya asocia con la marca. Mantener los dos nombres separados ahora juega en contra: el único sitio que puede afirmar con autoridad cuáles son las cuentas oficiales es esta web.

Estado de la hoja publicada, verificado el 2026-11-28:

| Estado de fila | Estilo en la hoja | Valoración asociada |
|---|---|---|
| Verde oscuro | fondo `#93c47d` | "En su mínimo registrado" |
| Verde claro | fondo `#d9ead3` | "Barato para su histórico" |
| Blanco | sin fondo | "En su precio habitual", "Sin histórico suficiente", "Sin cambios de precio" |
| Tachado | fondo `#f3f4f6`, texto `#888`, `line-through` | No disponible; prevalece sobre la valoración |

El rango se muestra como `mínimo [··●·······] máximo`, con 10 posiciones. El punto marca dónde cae el precio actual entre el mínimo y el máximo registrados. Ejemplo: 10,99 € mínimo, 24,90 € máximo y 12,45 € actual dan la segunda posición. "Punto a la izquierda = cerca de su mínimo registrado" es una lectura correcta.

## Goals / Non-Goals

**Goals:**
- Que la página afirme, en texto visible y en marcado, que NutriChollos es este sitio y cuáles son sus únicas cuentas oficiales.
- Que cualquier visitante pueda leer los colores y el indicador del rango sin explicación externa.

**Non-Goals:**
- Rediseñar `og-image`.
- Ampliar el texto de posicionamiento a otras categorías de producto.
- Mencionar o enlazar el perfil ajeno. Nombrarlo le daría una señal más.

## Decisions

- **h1 "NutriChollos", descripción reescrita.** La descripción pasa a "Tracker independiente de precios reales de proteína y creatina en España". Sigue cabiendo en una línea a 864 px y en dos a 375 px, como exige la spec. Se pierde la mención a "disponibilidad de stock", que ahora explica la leyenda (fila tachada).
- **Title: intención primero, marca al final.** Se conserva la regla "no empieza por el nombre del proyecto" y se sustituye "Tracker diario" por la marca. La spec gana la exigencia de que el title termine en "| NutriChollos".
- **Description:** "NutriChollos: precios diarios de creatina y proteína en HSN, MyProtein, Prozis y Zumub, con mínimo registrado y €/100 g de proteína." (≤160 caracteres; sin cifras volátiles).
- **JSON-LD: `Organization` + `WebSite` sin `SearchAction`.**
  - `Organization`: `@id` `…/#org`, `name` "NutriChollos", `url` canónica, `logo` `apple-touch-icon-v1.png` en URL absoluta (180×180, por encima del mínimo de 112×112 de Google).
  - `sameAs`, con URLs exactas:
    - `https://x.com/nutrichollos`
    - `https://t.me/NutriChollos`
    - `https://www.tiktok.com/@nutrichollos_tracker`
  - `founder` → `Person`.
  - `WebSite`: `name` "NutriChollos", `url`, `inLanguage`, `publisher` → `Organization`.
  - `Dataset`: se le añade `publisher` → `Organization` y conserva `creator` → `Person`.
  - Sigue siendo un único script `application/ld+json`.
- **Cuentas oficiales en el pie, no en la cabecera.** La spec reserva la zona sobre la tabla para el enlace de Telegram. En el pie van como enlaces de texto en línea, sin aspecto de botón, para no competir con la franja de Telegram.
  - Copy: "Cuentas oficiales de NutriChollos: X · Telegram · TikTok. No hay otras."
  - Cada enlace lleva `rel="me noopener noreferrer"` y `target="_blank"`; `rel="me"` declara la identidad compartida.
  - Los nombres accesibles incluyen el handle y "se abre en una ventana nueva".
- **Leyenda inmediatamente después del iframe**, dentro de la misma sección ancha y antes de la tarjeta de sugerencias.
  - Muestras de color con los mismos valores que la hoja, cada una acompañada de su texto, para no depender solo del color.
  - Una línea para el rango con un ejemplo literal `[●·········]`.
  - Matiz para el rango: es relativo al propio histórico, y con poco histórico o un rango estrecho dice poco.
  - Se descarta ponerla dentro de la sección de metodología: queda demasiado lejos de la tabla.
- **El texto de la leyenda describe la hoja tal como es hoy.** Si los colores de la hoja cambian, hay que cambiar la leyenda; la spec lo deja escrito.

## Risks / Trade-offs

- El `<h1>` cambia de texto: la pestaña y los resultados mostrarán "NutriChollos" donde antes se leía el nombre descriptivo. Lo mitiga la línea de descripción, que dice qué es.
- La leyenda queda acoplada a la paleta de la hoja, que vive fuera del repo. Si alguien cambia un color en la hoja sin tocar `index.html`, la leyenda queda desfasada.
- La lista de cuentas en el pie es una promesa ("no hay otras"). Si se abre una cuenta nueva, hay que añadirla en el pie y en `sameAs` a la vez.
