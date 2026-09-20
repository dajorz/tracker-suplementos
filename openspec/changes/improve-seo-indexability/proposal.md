## Why

Todo el contenido con valor de esta página vive dentro de un `<iframe>` de `docs.google.com`. Los 35 productos, las cuatro tiendas, el mínimo registrado y el €/100 g de proteína pertenecen, para cualquier rastreador, a otro dominio. Lo que se indexa bajo `dajorz.github.io/tracker-suplementos/` son unas 400 palabras, de las cuales alrededor del 60% es el texto del `<dialog>` de política de cookies.

Contado sobre el HTML servido, las cadenas `HSN`, `MyProtein`, `Prozis`, `Zumub`, `creatina`, `monohidrato`, `Creapure`, `whey`, `isolate` y `€/100 g` aparecen **cero veces** fuera del iframe. La única consulta para la que la página está posicionada es su propio nombre, que nadie busca.

El efecto es doble. Google no tiene sobre qué rankear. Y los rastreadores que ni ejecutan JavaScript ni siguen iframes —`BingBot`, `GPTBot`, `PerplexityBot`— ven una página que no menciona ni un solo producto, justo cuando "¿dónde está más barata la creatina?" empieza a resolverse en esas superficies.

A esto se suma una fuga por el otro extremo: la página no declara `og:image`. Su distribución real es Telegram y Reddit, sitios donde la tarjeta de enlace *es* el anuncio. Hoy cada vez que alguien comparte el enlace se renderiza una caja gris.

## What Changes

- **Añadir una sección de metodología y procedencia** al final de `<main>`, bajo la tabla y bajo el CTA de sugerencias, encabezada por un `<h2>` "Cómo funciona este tracker". Explica en prosa qué tiendas se siguen, qué categorías de producto se cubren, qué significa el €/100 g de proteína, qué es el «mínimo registrado», con qué frecuencia se actualiza y quién está detrás, firmando como `dajorz`. Es el único elemento de este cambio que añade texto indexable, y por tanto el único que puede mover tráfico orgánico.
- **Declarar la relación comercial en dos afirmaciones separadas**: una de estado presente y revocable ("hoy no hay enlaces de afiliación"), que cubre tanto la web como el canal de Telegram al que esta página enlaza; y otra estructural y sostenible indefinidamente (el orden de la tabla y el mínimo registrado salen solo de precios observados y no los influye ningún acuerdo comercial). Se descarta expresamente cualquier promesa perpetua de ausencia de afiliación: el modelo de sostenimiento a largo plazo del proyecto no está cerrado, y una promesa absoluta que luego hubiera que revocar destruiría justo el activo que da valor al dato.
- **Desacoplar `meta description`, `og:description` y `twitter:description` de la frase de descripción de la cabecera.** Pasan a ser copy de SERP orientado al clic, no la frase de posicionamiento del header. Las tres siguen siendo idénticas entre sí. La frase visible de la cabecera no se toca.
- **Reescribir el `<title>`** de `Tracker de Precios de Suplementación` a `Precios de creatina y proteína en España | Tracker diario`, y alinear `og:title` y `twitter:title` con él.
- **Añadir una tarjeta social**: un `og-image-v1.png` de 1200×630 en la raíz del repositorio, declarado con `og:image` en URL absoluta, más `og:image:width`, `og:image:height` y `og:image:alt`. `twitter:card` sube de `summary` a `summary_large_image`.
- **Añadir datos estructurados JSON-LD** con dos tipos: `Dataset`, que describe el conjunto de precios como el recurso que realmente es, y `Person`, que da por primera vez una autoría identificable a la página.
- **Añadir `robots.txt` y `sitemap.xml`** en la raíz. Es la pieza de menor valor del cambio y se incluye con los ojos abiertos: su beneficio real hoy es habilitar el informe de cobertura de Search Console y dejar montado el andamio para futuras sub-páginas.- **Extender la higiene de la dirección de contacto a todo el repositorio.** El requisito vigente prohíbe que la dirección aparezca literal en el HTML servido, y se cumple; pero el repositorio es público y se sirve por GitHub Pages, así que sus ficheros fuente son texto plano indexable y consultable por la búsqueda de código. La dirección figuraba literal en cuatro puntos de un change archivado, justo el vector que el ensamblado en tiempo de ejecución existe para bloquear. Se redacta y la prohibición pasa a cubrir el repositorio entero.
- **Añadir instrumentación de búsqueda**: una meta-etiqueta `google-site-verification` en el `<head>` y el alta de una propiedad de prefijo de URL en Search Console. Sin esto, ninguna de las decisiones anteriores es evaluable y la siguiente se tomaría otra vez por intuición.- **BREAKING (a nivel de spec)**: el repositorio deja de ser un único `index.html` autocontenido. Pasa a tener tres ficheros hermanos (`og-image-v1.png`, `robots.txt`, `sitemap.xml`). No se introduce build step: siguen siendo ficheros estáticos servidos tal cual por GitHub Pages.
- **BREAKING (a nivel de spec)**: desaparece la garantía de que las tres descripciones coincidan carácter a carácter con la frase de la cabecera. Esa regla nació como consistencia y hoy impide optimizar el snippet.
- No se toca el iframe, ni su altura, ni su `loading="lazy"`. No se toca el flujo de consentimiento, la política de cookies, la integración GA4, el favicon, la franja de Telegram ni el ensamblado anti-scraping del `mailto:`. No se añade nada entre la cabecera y la tabla.

### Lo que este cambio NO resuelve

Conviene dejarlo escrito para que nadie lea el resultado como "el SEO ya está hecho":

| Consulta | ¿Alcanzable tras este cambio? |
|---|---|
| `creatina barata` | No. Head term contra Amazon, HSN y comparadores establecidos. |
| `creatina monohidrato 1kg precio` | No. Requiere los nombres de producto en el HTML. |
| `cuánto cuesta 100 g de proteína` | Sí. La sección de metodología lo explica en prosa. |
| `prozis vs hsn creatina precio` | Parcialmente. Las marcas ya estarán en el HTML, los precios no. |
| `qué es el mínimo registrado de un precio` | Sí. |

El long-tail de producto —que es donde vive el dato verdaderamente único de este proyecto— sigue bloqueado por el iframe. Desbloquearlo exige hornear la tabla en HTML mediante una GitHub Action programada, lo que rompería la restricción de "sin build step". Esa decisión se ha discutido y se aparca deliberadamente fuera de este cambio.

## Capabilities

### New Capabilities
(none)

### Modified Capabilities
- `pricing-tracker-landing-page`: el requisito de página estática de fichero único admite ficheros estáticos hermanos sin build step; el requisito de metadatos SEO y compartición social se reescribe para desacoplar las descripciones de la frase de la cabecera, fijar un `<title>` orientado a consulta, prohibir cifras volátiles y exigir una tarjeta social con imagen versionada; el requisito de la CTA de sugerencias extiende la prohibición de dirección literal del HTML servido a todo el repositorio público; se añade un requisito de sección de metodología y procedencia; se añade un requisito de datos estructurados; se añade un requisito de directivas de rastreo y sitemap; se añade un requisito de instrumentación de rendimiento en búsqueda.

## Impact

- Ficheros afectados: `index.html` (bloque `<head>` reescrito, una `<section>` nueva al final de `<main>`, un `<script type="application/ld+json">`), más tres ficheros nuevos en la raíz: `og-image-v1.png`, `robots.txt`, `sitemap.xml`.
- Un requisito modificado, uno reescrito y cuatro añadidos en `openspec/specs/pricing-tracker-landing-page/spec.md`.
- El texto indexable de la página pasa de ~400 palabras (60% boilerplate legal) a ~1.000, con la proporción de contenido útil invertida.
- Riesgo de datos estructurados: se descarta explícitamente marcar `Product`/`Offer`. El marcado debe describir contenido visible en la propia página, y los productos viven en el iframe; además `Offer` implica que el sitio vende, lo que aquí no es cierto. Marcar los 35 productos sería una infracción de las directrices con riesgo de acción manual.
- Sin impacto en privacidad, consentimiento, cookies ni analítica. El JSON-LD es estático y no carga nada. La `og:image` se sirve desde el propio origen, sin CDN de terceros.
- Impacto en rendimiento: una imagen de menos de 200 kB que el navegador del visitante **no descarga** (solo la piden los scrapers sociales), y unos 800 bytes de JSON-LD inline. LCP y CLS quedan igual.
- Escenarios de verificación existentes que este cambio puede romper: "Descriptions match the visible project description" (se retira por diseño), "Page loads with no build tooling" (debe seguir pasando; solo cambia el conteo de ficheros) y "Preamble stays within budget" (no debería verse afectado, pero la sección nueva obliga a reconfirmar que nada se ha colado sobre la tabla).
- Alternativa descartada — **frase puente sobre la tabla**: una línea bajo la franja de Telegram del tipo "Precios reales de creatina y proteína en 4 tiendas españolas — cómo funciona" daría contexto a quien aterrice desde Google directamente sobre una hoja de cálculo sin encabezado. Se descarta porque el cambio `move-notices-to-footer` acaba de bajar el preámbulo móvil de 293px a 213px contra un presupuesto duro de 220px, y reintroducir una línea consume ese margen por una hipótesis de rebote que hoy no está medida. Revisable cuando Search Console aporte datos reales de tráfico orgánico.
- Alternativa descartada — **`FAQPage` y formato pregunta-respuesta**: se evaluó redactar la metodología como Q&A para optar a rich results. Google restringió los rich results de FAQ a sitios gubernamentales y sanitarios reconocidos, así que el marcado no produciría ningún resultado enriquecido para este dominio. Sin ese incentivo, la prosa con subtítulos `<h3>` se lee mejor y se mantiene más fácil.
- Alternativa descartada — **`WebSite` con `SearchAction`**: el sitelinks searchbox está retirado y la página no tiene buscador propio. Marcado sin función.
- Reversión: tres ficheros a borrar y un `git revert` sobre `index.html`. Sin estado persistido ni migración. La única secuela no reversible es la caché de los scrapers sociales, mitigada por el sufijo de versión en el nombre de la imagen.
