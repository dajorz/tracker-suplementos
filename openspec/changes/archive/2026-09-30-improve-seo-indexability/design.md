## Context

`index.html` es un fichero estático servido por GitHub Pages en `https://dajorz.github.io/tracker-suplementos/`. Todo su contenido de valor está delegado a un `<iframe>` que apunta a una hoja de Google publicada.

Medido sobre el HTML servido hoy:

```
  Texto indexable total ................. ~400 palabras
  ├─ <dialog> política de cookies ....... ~250  (63%)
  ├─ franja Telegram .................... ~35
  ├─ CTA sugerencias .................... ~25
  ├─ <header> ........................... ~30
  ├─ <footer> ........................... ~35
  └─ banner de consentimiento ........... ~25

  Ocurrencias fuera del iframe de:
    HSN, MyProtein, Prozis, Zumub ....... 0
    creatina, proteína (como producto) ... 0*
    monohidrato, Creapure, whey, isolate . 0
    €/100 g, mínimo registrado ........... 0

  * "creatina y proteína" aparece una vez, en la frase de la cabecera.
```

Jerarquía semántica actual:

```
  <h1> Tracker de Precios de Suplementación
   ├─ <h2> ¿Echas en falta algún producto?     ← un CTA
   └─ <h2> Política de cookies                 ← un aviso legal
```

Los dos únicos `<h2>` de la página declaran que trata de un formulario de contacto y de un documento legal.

Cuatro restricciones condicionan cualquier solución:

1. **El contenido del iframe no es atribuible.** Ningún rastreador lo cuenta como contenido de esta página. No hay meta-etiqueta que lo arregle.
2. **El marcado estructurado debe describir contenido visible.** Es directriz explícita de Google. Bloquea marcar los productos del iframe.
3. **Presupuesto de altura sobre la tabla: 220px a 375px de ancho.** Requisito vigente, con el valor actual en 213px. Quedan 7px de margen.
4. **Sin build step.** No hay compilación, ni CSS propio, ni tests. Cualquier dato que se escriba a mano en el HTML se queda congelado y empieza a envejecer el mismo día.

La restricción 4 tiene una consecuencia que no es obvia y que se aplica varias veces en las decisiones de abajo: **nada de lo que se escriba puede depender de un valor que cambie**. Ni "35 productos", ni una fecha de última actualización, ni un precio concreto.

## Goals / Non-Goals

**Goals:**

- Multiplicar el texto indexable y, sobre todo, cambiar su composición: que la mayoría deje de ser boilerplate legal.
- Introducir en el HTML propio las entidades que hoy solo existen dentro del iframe: las cuatro tiendas y las categorías de producto.
- Conseguir que un enlace compartido en Telegram o Reddit muestre una tarjeta con imagen.
- Dar a la página autoría identificable y metodología explícita, que es lo que sostiene la credibilidad de un dato como "mínimo registrado".
- Hacer medible el resultado: sin Search Console, todo lo anterior es fe.
- Mantener el preámbulo sobre la tabla exactamente como está.

**Non-Goals:**

- No se saca la tabla del iframe. No se añade GitHub Action, ni CSV, ni build step.
- No se crean sub-páginas `/creatina/` ni `/proteina/`.
- No se contrata dominio propio.
- No se marca ningún `Product` ni `Offer`.
- No se toca nada entre la cabecera y la tabla.
- No se persiguen head terms. Ver la tabla de expectativas en `proposal.md`.
- No se reescribe la frase de descripción de la cabecera.

## Decisions

### 1. La sección de metodología va la última, después del CTA

Se consideraron tres ubicaciones:

| Ubicación | SEO | Contexto al aterrizar | Coste |
|---|---|---|---|
| Sobre la tabla | mejor | resuelto | rompe el presupuesto de 220px |
| Tabla → metodología → CTA | igual | no resuelto | entierra el CTA bajo ~600 palabras |
| Tabla → CTA → metodología (elegida) | igual | no resuelto | ninguno |

El eje SEO no decide: dentro de una misma URL, la posición en el orden de documento pesa de forma marginal comparada con el hecho de existir, que es lo que faltaba. Así que decide el eje de producto.

El CTA `mailto:` es el único canal de feedback del proyecto, y ya está en zona difícil: el iframe captura la rueda del ratón, de modo que todo lo que queda debajo requiere una intención deliberada de desplazarse. Intercalar 600 palabras entre la tabla y el CTA lo mata del todo. La metodología, en cambio, la busca quien la busca; que esté al final no la penaliza.

La opción "sobre la tabla" se descarta por la restricción 3, no por preferencia. El cambio `move-notices-to-footer` dejó el preámbulo móvil en 213px contra un techo de 220px. No hay sitio.

### 2. Las descripciones se desacoplan de la cabecera; el `<title>` se reescribe

La regla vigente exige que `meta description`, `og:description` y `twitter:description` coincidan carácter a carácter con la frase de la cabecera. Nació como garantía de consistencia y hoy cuesta clics, porque las dos cadenas tienen trabajos distintos:

```
  frase del header  →  "quién soy"       →  humano que YA llegó
  meta description  →  "por qué clicar"  →  humano que AÚN NO llegó
```

Se desacoplan, pero se mantiene la coherencia entre las tres etiquetas: las tres son copy para superficies externas y deben decir lo mismo.

```
  <title>       Precios de creatina y proteína en España | Tracker diario
                └─ 57 caracteres. Keyword al frente; Google corta ~60.

  description   Precios diarios de creatina y proteína en HSN, MyProtein,
                Prozis y Zumub. Mínimo histórico registrado y €/100 g de
                proteína para comparar sin marketing.
                └─ ~152 caracteres.
```

Se evaluó y **se rechaza** incluir cifras en la description ("35 productos", "4 tiendas"). Por la restricción 4, cualquier número escrito a mano es una afirmación que empieza a envejecer el día que se publica y que nadie recordará actualizar. Los nombres de las cuatro tiendas, en cambio, son estables y aportan el mismo gancho de concreción.

`og:title` y `twitter:title` adoptan el valor del `<title>`. La frase visible de la cabecera no cambia.

### 3. La tarjeta social es genérica de marca, no de datos

```
   Datos reales                     Marca genérica (elegida)
   "Creatina 1 kg desde 12,98 €"    "Tracker de Precios de Suplementación
                                     creatina · proteína
   ▓ mucho más clicable              HSN · MyProtein · Prozis · Zumub"
   ▓ caduca en días
   ▓ exige regeneración              ▓ nunca miente
     automática → build step         ▓ menos gancho
```

Restricción 4 otra vez. Sin Action no hay forma de regenerar la imagen, y una tarjeta que anuncia un precio de hace tres semanas es peor que una genérica. Si algún día se hornea la tabla, regenerar la imagen con el mínimo del día sale casi gratis en el mismo workflow; el nombre versionado (decisión 4) deja esa puerta abierta.

#### Composición

El proyecto ya dispone de un logotipo cuadrado de 1254×1254 —un robot con coctelera sobre fondo azul marino y acentos lima— que es también el avatar del canal de Telegram. **No sirve como `og:image` tal cual**: `summary_large_image` exige un mínimo de 2:1 y recorta al centro, lo que amputaría la coctelera y el distintivo inferior, justo los dos elementos que comunican la materia. Sí sirve como fuente de identidad: aporta paleta y marca gráfica, que hasta ahora no existían.

Paleta muestreada del propio fichero, no estimada:

```
  #051322   fondo navy     19,7% de los pixeles, y las cuatro esquinas
  #98F13E   lima           acento del robot
  #E9ECF2   blanco frio    luces del robot
  #A6ADB8   atenuado       derivado: #E9ECF2 al 70% sobre el fondo
```

Contraste contra `#051322`: lima 13,4:1 · blanco 15,8:1 · atenuado 8,1:1. Los tres superan AA con holgura.

El fondo del logotipo **ya es** `#051322`, así que colocado sobre ese mismo color no muestra borde de caja: queda recortado sin necesidad de máscara. Si aparece una costura por el viñeteado del original, se resuelve con un desvanecido radial suave en el borde, nunca recortando el motivo.

```
  1200 x 630  ·  fondo #051322  ·  margen de seguridad 64px

  +------------------------------------------------------------+
  |                                                            |
  |    +--------------+                                        |
  |    |              |   Tracker de Precios         64px/700  |  #98F13E
  |    |    robot     |   de Suplementacion          lh 1,15   |
  |    |   400x400    |                                        |
  |    |   x88 y115   |   creatina · proteina ·      32px/400  |  #E9ECF2
  |    |              |   precios diarios                      |
  |    +--------------+                                        |
  |                       HSN · MyProtein ·          28px/400  |  #A6ADB8
  |                       Prozis · Zumub                       |
  |                                                            |
  +------------------------------------------------------------+
        x: 88...488          x: 552...1136  (caja de texto 584px)
```

| Elemento | x | y | tamaño | color |
|---|---:|---:|---|---|
| Logotipo | 88 | 115 | 400×400 | — |
| Título (2 líneas) | 552 | 180 | 64px / 700 / lh 1,15 | `#98F13E` |
| Categorías (2 líneas) | 552 | 360 | 32px / 400 | `#E9ECF2` |
| Tiendas (2 líneas) | 552 | 414 | 28px / 400 | `#A6ADB8` |

Tipografía: cualquier sans humanista de amplia disponibilidad (Inter, o la sans de sistema). No se incrusta ninguna fuente, porque la imagen se exporta rasterizada.

El margen de 64px no es estético: 1200×630 es 1,905:1, y un recorte a 2:1 se come unos 15px por arriba y por abajo. **Es aire, no un marco.** Pintarlo como banda de otro tono parece buena idea hasta que el scraper recorta: el marco queda visiblemente más fino arriba y abajo que a los lados, y se lee como un fallo de render. El lienzo va de un solo color de borde a borde.

Las medidas de la tabla son una referencia proporcional, no un tamaño obligatorio. Lo que la spec exige es proporción entre 1,85:1 y 2:1, un mínimo de 1200px de ancho y que `og:image:width`/`og:image:height` coincidan exactamente con el fichero. Un lienzo mayor con las mismas proporciones se ve mejor en pantallas densas y no cuesta nada mientras se respete el presupuesto de peso.

Los tamaños de tipo se fijan para que el título siga leyendo al 25% de escala —16px efectivos—, que es como aparece la tarjeta en la lista de chats de un móvil. Las dos líneas secundarias caen a 8px y 7px a esa escala: son decorativas en miniatura y legibles a tamaño completo. Se acepta, porque el título es el que tiene que trabajar solo.

Exportación: componer en PNG-24 y cuantizar hasta bajar de 200 kB. El logotipo se reescala de 1254 a 400 con un filtro de buena calidad (Lanczos o bicúbico), nunca por vecino más próximo. Si tras cuantizar sigue por encima del presupuesto, JPG a calidad 85 es aceptable —la spec pide una imagen ráster, no un formato concreto—, cambiando entonces la extensión tanto en el fichero como en la etiqueta.

`og:image:alt`: «Tracker de Precios de Suplementación: precios de creatina y proteína en HSN, MyProtein, Prozis y Zumub».

**El nombre de la tarjeta es el de la web, no el del canal.** El logotipo, compartido entre ambas superficies, es el que establece la continuidad; duplicar nombres obligaría al visitante a aprenderse dos marcas para la misma cosa. La tarjeta enlaza a la web, así que manda la web.

**La marca gráfica pasa a ser una sola, en composición circular.** Al producir la tarjeta apareció un redibujo circular del robot, distinto del logotipo cuadrado original, y mantener los dos habría roto el puente visual que justificaba poner el robot. Se resuelve adoptando el circular como marca única, incluido el avatar del canal. El motivo no es estético: Telegram recorta los avatares a círculo, de modo que las esquinas del logotipo cuadrado ya se están perdiendo hoy en la superficie donde más se ve la marca. Componer en círculo es adaptarse a cómo se muestra realmente, no una concesión al formato de la tarjeta. Sustituir el avatar del canal queda fuera del repositorio, pero se registra como tarea para que el puente no se quede a medias.

**La lista de tiendas es la única concesión a la restricción 4.** No es una cifra, pero envejece si algún día se añade una quinta tienda, y una imagen cacheada por los scrapers es mucho más difícil de corregir que una cadena de texto. Se acepta porque sin ella la tarjeta pierde toda concreción, y porque el versionado de la decisión 4 le da salida: **añadir una tienda es el disparador para publicar `og-image-v2.png`**.

### 4. El fichero se llama `og-image-v1.png`, con versión desde el día uno

Los scrapers sociales —Telegram de forma especialmente agresiva, también Reddit y Slack— cachean la imagen por URL y durante mucho tiempo. Si se reutiliza el nombre `og-image.png` tras un rediseño, todos los enlaces ya compartidos seguirán mostrando la versión vieja, potencialmente durante meses, sin mecanismo de invalidación al alcance.

El sufijo de versión cuesta cero hoy y convierte un problema irreversible en un cambio de una línea. Se adopta aunque todavía no exista una v2.

Especificaciones fijadas: 1200×630, PNG, por debajo de 200 kB, `og:image` en **URL absoluta** (los relativos funcionan en unos scrapers y fallan en otros), y `og:image:width`/`og:image:height` declarados para que el scraper reserve la caja sin reflow. `twitter:card` sube a `summary_large_image`, sin lo cual la imagen seguiría mostrándose en miniatura.

### 5. Solo `Dataset` y `Person`; ni `Product`, ni `FAQPage`, ni `WebSite`

Evaluación honesta de cada tipo:

| Tipo | Veredicto |
|---|---|
| `Product` + `Offer` | **Prohibido.** Describiría contenido no visible en la página (restricción 2) e implicaría que el sitio vende, lo que es falso. Riesgo de acción manual. |
| `FAQPage` | **Inútil.** Google limitó sus rich results a sitios gubernamentales y sanitarios reconocidos. No produciría nada aquí. |
| `WebSite` + `SearchAction` | **Inútil.** El sitelinks searchbox está retirado y la página no tiene buscador. |
| `Dataset` | **Se adopta.** Describe el recurso, no contenido oculto: es literalmente un conjunto de datos de precios. Sin riesgo. |
| `Person` | **Se adopta.** Sin rich result, pero es la señal de entidad y autoría que hoy no existe. |

Hay que decir con claridad cuánto vale esto:

```
   JSON-LD   ▓░░░░░░░░░   payoff real de tráfico
   Prosa     ▓▓▓▓▓▓░░░░   payoff real de tráfico
```

El marcado se adopta porque es correcto, barato y a prueba de futuro, **no porque vaya a traer visitas**. Dataset Search es una superficie minúscula para consumo general. Quien lea este documento dentro de seis meses buscando por qué el tráfico no subió debe mirar la prosa, no el JSON-LD.

El `Dataset` **no declara `dateModified`**. Sería un valor escrito a mano que envejece (restricción 4) y una fecha falsa es peor señal que ninguna fecha. Sí declara `temporalCoverage` como intervalo abierto `2026-08-30/..`, que es cierto y no caduca.

### 6. `robots.txt` y `sitemap.xml` se incluyen, con el valor que tienen

Es la pieza más débil del cambio y merece justificarse en lugar de colarse:

- Un `robots.txt` que permite todo es funcionalmente un no-op: permitir es el comportamiento por defecto. Su único contenido con función es la línea `Sitemap:`.
- Un `sitemap.xml` con una sola URL no aporta descubrimiento: Google ya conoce la raíz.

Lo que sí aportan, juntos: poder enviar el sitemap en Search Console y obtener el informe de cobertura, que es retroalimentación real sobre indexación. Y dejar montado el andamio para las sub-páginas del día en que se hornee la tabla.

El sitemap **no lleva `<lastmod>`**. Misma lógica que la decisión 5: un `lastmod` estático que nunca cambia es una señal que Google acaba ignorando, y mientras tanto miente.

### 7. Search Console es parte del cambio, no un "luego ya veremos"

Sin instrumentación, todo lo anterior es fe. Se incorpora como tarea explícita.

Hay una arruga técnica que conviene conocer antes de intentarlo: `dajorz.github.io` está en la Public Suffix List, así que se trata como sitio propio, pero **no se puede dar de alta una propiedad de dominio** porque eso exige control DNS, que aquí no existe. Hay que crear una propiedad de **prefijo de URL** sobre `https://dajorz.github.io/tracker-suplementos/` y verificarla con la meta-etiqueta `google-site-verification` en el `<head>`.

Consecuencia secundaria: una propiedad de prefijo sobre una subcarpeta solo reporta esa subcarpeta, que es exactamente lo que se quiere.

### 8. El proyecto firma como `dajorz`

El marcado `Person` y la línea "quién está detrás" de la metodología exigen decidir con qué nombre se firma. Hay un intercambio real:

```
   Nombre real          → más E-E-A-T, es exposición personal
   Pseudónimo "dajorz"  → menos señal, coherente con el handle ya público
```

Se fija **`dajorz`**, por ser lo que ya aparece en la URL del sitio y no añadir exposición personal que hoy no existe. El mismo nombre se usa en el nodo `Person` y en la prosa de la sección de metodología: una identidad inconsistente entre marcado y texto visible es peor señal que un pseudónimo.

### 9. La declaración comercial es un estado presente, no una promesa perpetua

La primera redacción de esta sección exigía "declarar si el proyecto usa enlaces de afiliación". Formulada así, invita a escribir una frase absoluta —"este tracker no tiene enlaces de afiliación"— que el proyecto no está en posición de sostener indefinidamente, porque su modelo de sostenimiento a largo plazo no está cerrado.

Una promesa absoluta que luego se revoca es peor que no haberla hecho, porque el activo que se quema es exactamente el que da valor al dato. Así que la frase se parte en dos afirmaciones con naturalezas distintas:

```
  A │ "hoy no hay enlaces de afiliación"
    │ ▸ presente, revocable, requiere mantenimiento
    │ ▸ debe cubrir las DOS superficies: la web y el canal
    │
  B │ "el orden de la tabla y el mínimo registrado salen solo
    │  de precios observados, nunca de un acuerdo comercial"
    │ ▸ estructural, sostenible indefinidamente
    │ ▸ es la que protege la credibilidad del dato
```

**A** se redacta en presente y sin cláusula de permanencia. Nada de "nunca tendrá"; sí "hoy no tiene".

**B** es la que se puede firmar, porque no habla de ingresos sino de qué no contamina el dato. Cualquier forma de sostenimiento futuro es compatible con ella, siempre que ningún acuerdo comercial influya en la ordenación ni en el cálculo del mínimo. Esa es la línea que no conviene cruzar, y por eso se fija en la spec y no solo en la prosa.

Hay además una fuga que la formulación original no cubría: **la web enlaza al canal de Telegram**. Si alguna vez las dos superficies divergieran —una con enlaces comerciales y la otra sin ellos— y la web declarase "sin afiliación" a secas, la afirmación sería cierta en la letra y engañosa en el fondo, porque la página embuda visitantes hacia la otra. Por eso la declaración debe cubrir explícitamente ambas superficies y nombrar la diferencia cuando la haya.

Disparador de mantenimiento, para que quede escrito: **si alguna vez la web o el canal incorporan un enlace de afiliación, esta línea de la sección de metodología debe actualizarse en el mismo cambio, no después.** Es la única afirmación de la página que caducaría por una decisión propia y no por el paso del tiempo, lo que la hace fácil de olvidar.

Queda fuera de alcance, pero anotado para cuando aplique: si alguna vez se incorporan enlaces comerciales en cualquiera de las dos superficies, conviene revisar antes los requisitos de identificabilidad de las comunicaciones comerciales (LSSI art. 20) y de transparencia en contextos de comparación.

## Risks / Trade-offs

**[Este cambio puede no mover el tráfico orgánico en absoluto]** → Es el riesgo principal y es alto. Un dominio sin autoridad, con el contenido diferencial encerrado en un iframe, compitiendo en un nicho comercial saturado. El techo realista son consultas conceptuales de long-tail ("qué es el mínimo registrado", "cuánto cuesta 100 g de proteína"), no consultas de producto. Mitigación: ninguna dentro de este alcance; la palanca de verdad es hornear la tabla, y está deliberadamente fuera. Lo que sí se hace es instrumentar (decisión 7) para que la siguiente decisión se tome con datos y no con intuición.

**[La sección de metodología puede degenerar en relleno para buscadores]** → Si se escribe pensando en keywords en lugar de en el lector, se nota, se lee mal y Google lo trata como contenido de bajo valor. Mitigación: el criterio de redacción es que cada subtítulo responda a una duda real de alguien que mira la tabla por primera vez. La cobertura de keywords es consecuencia, no objetivo. Si un párrafo no le sirve a nadie, sobra aunque contenga la palabra "creatina".

**[Un visitante que aterrice desde Google verá una hoja de cálculo sin contexto]** → La metodología está al final (decisión 1) y la frase puente sobre la tabla se ha descartado (restricción 3). Quien llegue desde una búsqueda ve título, frase de cabecera, franja de Telegram y una tabla. Puede rebotar. Aceptado conscientemente: la alternativa consumía el presupuesto de altura que otro cambio acaba de conquistar, para resolver un problema que todavía no está medido. Revisable con datos de Search Console.

**[El repositorio deja de ser un único fichero]** → Es una pérdida real de simplicidad, y la requirement que lo garantizaba se modifica de forma explícita en lugar de erosionarse en silencio. Lo que se protege no es el conteo de ficheros sino la ausencia de build step, que se mantiene intacta: los tres ficheros nuevos se sirven tal cual.

**[La caché de los scrapers sociales es irreversible]** → Si se publica una `og:image` con un error, los enlaces ya compartidos lo arrastran durante meses. Mitigación: el sufijo de versión (decisión 4) y una tarea explícita de validar la tarjeta en un scraper real **antes** de anunciar el cambio en el canal de Telegram.

**[Un `Dataset` sin `dateModified` desaprovecha la señal de frescura]** → Cierto, y asumido. La alternativa —una fecha a mano que se congela— es peor. La frescura real solo se puede declarar con honestidad el día que haya un proceso automático que la actualice.

**[Verificar Search Console con meta-etiqueta acopla el HTML a un servicio externo]** → Es una línea en el `<head>` que no carga nada ni observa al visitante, así que no afecta a privacidad ni al flujo de consentimiento. Pero es una dependencia de configuración invisible: si alguien la borra por parecer ruido, la propiedad se desverifica silenciosamente. Mitigación: un comentario de una línea junto a la etiqueta explicando qué rompe al quitarla.

**[La declaración comercial caducaría por decisión propia y nadie lo notaría]** → Es la única afirmación de la página que no envejece con el tiempo sino con un acto voluntario: incorporar un enlace de afiliación. Precisamente por eso es fácil de olvidar, y una página que se siguiera proclamando sin afiliación después de haberla incorporado perdería de golpe lo único que la distingue de un comparador comercial. Mitigación: el disparador está escrito en la decisión 9 y la spec exige que la declaración cubra también el canal enlazado, que es la superficie más propensa a divergir. No hay mitigación automática posible sin build step.

**[Reversión]** → Borrar tres ficheros y revertir `index.html`. Sin estado, sin migración, sin caché que invalidar salvo la de los scrapers sociales, ya cubierta.
