## Why

Un perfil de TikTok ajeno al proyecto (`@nutrichollos`, nombre visible "Nutri-Chollos🍅") se ha quedado con la marca en Google: la vista de IA asocia la consulta "nutrichollos" a ese perfil. La web no ayuda a corregirlo. "NutriChollos" no aparece en ningún texto visible ni en ningún metadato; solo está dentro de la URL de Telegram. El h1 dice "Tracker de Precios de Suplementación" y el JSON-LD solo declara un `Dataset` y una `Person` ("dajorz"). Google no tiene ninguna señal en la propia página que una la marca con este sitio y con los perfiles oficiales.

Además, la tabla no tiene leyenda. Los colores de fila y el indicador `[●·········]` del rango de precios solo se entienden si alguien te los explica. Se van a explicar en TikTok, y la web tiene que decir lo mismo.

## What Changes

- **h1 "NutriChollos"** en la cabecera, con una línea de descripción que diga qué es: un tracker independiente de precios reales de proteína y creatina en España.
- **`<title>` "Precios de creatina y proteína en España | NutriChollos"** (55 caracteres). Sigue empezando por la intención de búsqueda, como exige la spec. `og:title` y `twitter:title` llevan la misma cadena.
- **`meta description` con la marca**, junto con `og:description` y `twitter:description`. Sigue nombrando las tiendas, el mínimo registrado y el €/100 g.
- **JSON-LD**: el `@graph` actual gana un nodo `Organization` "NutriChollos" (url, logo y `sameAs` a X, Telegram y TikTok) y un nodo `WebSite` con la Organization como `publisher`. El `Dataset` añade `publisher` apuntando a la Organization y la `Person` pasa a ser su `founder`.
- **Cuentas oficiales en el pie**: una línea con enlaces de texto a X (`@nutrichollos`), Telegram (`t.me/NutriChollos`) y TikTok (`@nutrichollos_tracker`) que dice que son las únicas cuentas oficiales.
- **Leyenda de la tabla** justo debajo del iframe. Explica los cuatro estados de fila y el indicador del rango.
- **BREAKING (a nivel de spec)**: el pie deja de tener "Cookies" como único control (gana tres enlaces de texto), y los datos estructurados dejan de limitarse a `Dataset` y `Person`.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
- `pricing-tracker-landing-page`:
  - se reescriben la cabecera compacta (h1 y descripción), los metadatos SEO (title y description con marca), el pie (añade las cuentas oficiales) y los datos estructurados (añaden Organization y WebSite);
  - se añaden un requisito de cuentas oficiales y otro de leyenda de la tabla.

## Impact

- Ficheros: `index.html` (head, header, una sección nueva bajo el iframe, footer). No hay ficheros nuevos en la raíz.
- No cambia nada entre la cabecera y la tabla: la leyenda va debajo del iframe, así que el presupuesto de 220 px del preámbulo y la regla "la tabla encabeza el contenido" siguen intactos.
- Sin impacto en consentimiento, cookies ni analítica. Los enlaces nuevos no miden clics.
- Fuera de alcance:
  - `og-image-v1.png` y su `og:image:alt` siguen mostrando "Tracker de Precios de Suplementación". Rehacer la imagen sería una v2 con nombre nuevo.
  - La tabla ya incluye productos que no son proteína ni creatina (ashwagandha, vitamina D, crema de arroz, pre-entrenos). Este cambio no amplía el posicionamiento del texto.
  - Search Console ya está verificado; este cambio no lo toca.
- Riesgo: `sameAs` es una pista para Google, no un mandato. El desplazamiento del perfil ajeno en la vista de IA no está garantizado ni tiene plazo.
- Reversión: un `git revert` sobre `index.html`.
