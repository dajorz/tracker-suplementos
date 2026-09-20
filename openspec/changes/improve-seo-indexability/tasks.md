## 0. Decisiones confirmadas

- [x] 0.1 Identidad: el proyecto firma como **`dajorz`**, mismo valor en el nodo `Person` y en la prosa de la sección de metodología
- [x] 0.2 Frecuencia: la hoja se actualiza **a diario**, confirmado. El `<title>` puede afirmarlo
- [x] 0.3 Relación comercial: **no se firma ninguna promesa perpetua**. El modelo de sostenimiento a largo plazo del proyecto no está cerrado, así que la declaración se redacta en presente y se complementa con la promesa estructural de independencia del dato (ver decisión 9 del design)

## 1. Metadatos del `<head>`

- [x] 1.1 Reescribir el `<title>` a "Precios de creatina y proteína en España | Tracker diario" y verificar que no supera los 60 caracteres
- [x] 1.2 Reescribir `meta description` a copy de SERP que nombre las cuatro tiendas y mencione el mínimo registrado y el €/100 g de proteína, sin superar los 160 caracteres
- [x] 1.3 Alinear `og:title` y `twitter:title` con el nuevo `<title>`, carácter a carácter
- [x] 1.4 Alinear `og:description` y `twitter:description` con la nueva `meta description`, carácter a carácter
- [x] 1.5 Revisar las seis cadenas y confirmar que ninguna contiene un recuento de productos, un recuento de tiendas, un precio ni una fecha
- [x] 1.6 Confirmar que el párrafo de descripción del `<header>` queda exactamente como estaba, sin una sola edición
- [x] 1.7 Cambiar `twitter:card` de `summary` a `summary_large_image`
- [x] 1.8 Añadir `og:image` con la URL absoluta de `og-image-v1` bajo `https://dajorz.github.io/tracker-suplementos/`, más `og:image:width` y `og:image:height` con las dimensiones reales medidas en la tarea 2.11, y `og:image:alt` con el texto «Tracker de Precios de Suplementación: precios de creatina y proteína en HSN, MyProtein, Prozis y Zumub»
- [x] 1.9 Confirmar que `canonical`, `og:url`, `og:type`, `og:locale` y `lang="es"` siguen intactos

## 2. Tarjeta social

- [x] 2.1 Crear lienzo de 1200×630 con fondo `#051322`, el navy muestreado del logotipo
- [x] 2.2 Reescalar el logotipo de 1254×1254 a 400×400 con filtro Lanczos o bicúbico y colocarlo en x=88, y=115. Al coincidir su fondo con `#051322` no debe verse borde de caja; si asoma una costura por el viñeteado, aplicar un desvanecido radial suave, nunca recortar el motivo
- [x] 2.3 Componer el bloque de texto en x=552 con caja de 584px: título "Tracker de Precios de Suplementación" a 64px/700, interlineado 1,15, en `#98F13E`, desde y=180
- [x] 2.4 Añadir "creatina · proteína · precios diarios" a 32px/400 en `#E9ECF2` desde y=360, y "HSN · MyProtein · Prozis · Zumub" a 28px/400 en `#A6ADB8` desde y=414
- [x] 2.5 Confirmar que el margen de seguridad es aire del mismo color de fondo y **no** un marco de otro tono: comparar los píxeles de los cuatro bordes con los del centro y confirmar que coinciden
- [x] 2.6 Confirmar que la tarjeta lleva el nombre de la web y **no** el del canal de Telegram, y que el robot es el único elemento compartido entre ambas superficies
- [x] 2.7 Confirmar que no aparece ningún precio, recuento ni fecha en la imagen
- [x] 2.8 Exportar a la raíz como `og-image-v1`, por debajo de 200 kB, con proporción entre 1,85:1 y 2:1 y al menos 1200px de ancho. PNG-24 cuantizado o JPG a calidad 85, ajustando la extensión en el fichero y en la etiqueta
- [x] 2.9 Reducir la tarjeta al 25% y confirmar que el título sigue siendo legible, que es como se ve en la lista de chats de un móvil
- [x] 2.10 Medir el contraste de los tres bloques de texto contra el fondo y confirmar que se mantienen por encima de 4.5:1 tras comprimir
- [x] 2.11 Medir las dimensiones reales del fichero exportado y declarar **esos** valores en `og:image:width` y `og:image:height`
- [x] 2.12 Exportar el robot circular por separado, en alta resolución, y sustituir con él el avatar del canal de Telegram, de modo que exista una sola marca gráfica en ambas superficies (acción externa al repositorio)

## 3. Sección de metodología

- [x] 3.1 Añadir una `<section>` nueva como último hijo de `<main>`, después de la sección de sugerencias, encabezada por `<h2>` "Cómo funciona este tracker"
- [x] 3.2 Redactar el apartado `<h3>` de tiendas y productos, nombrando HSN, MyProtein, Prozis y Zumub, y las categorías cubiertas: creatina monohidrato, Creapure®, whey concentrada, whey isolate, clear whey, caseína y proteína de soja aislada
- [x] 3.3 Redactar el apartado `<h3>` del €/100 g de proteína, explicando que normaliza el precio según el contenido proteico real para poder comparar formatos distintos
- [x] 3.4 Redactar el apartado `<h3>` del «mínimo registrado», dejando claro que es el precio más bajo observado desde que se sigue el producto y no un PVP de referencia ni un precio tachado
- [x] 3.5 Redactar el apartado `<h3>` de frecuencia de actualización, indicando que la hoja se refresca a diario
- [x] 3.6 Redactar el apartado `<h3>` de autoría firmando como `dajorz`
- [x] 3.7 Redactar la declaración comercial **en presente y sin cláusula de permanencia** ("hoy no hay...", nunca "nunca habrá..."), cubriendo explícitamente tanto la web como el canal de Telegram al que esta página enlaza
- [x] 3.8 Añadir, junto a la anterior y como afirmación separada, la promesa estructural: el orden de la tabla y el mínimo registrado salen solo de precios observados y no los influye ningún acuerdo comercial. Redactarla de forma que siga siendo cierta aunque alguna de las dos superficies se monetice
- [x] 3.9 Releer los apartados con un único criterio: ¿le sirve esto a alguien que acaba de ver la tabla por primera vez? Eliminar cualquier párrafo que solo exista para contener palabras clave
- [x] 3.10 Revisar la sección completa y confirmar que no contiene ningún recuento, precio ni fecha concreta
- [x] 3.11 Aplicar al bloque el mismo lenguaje visual que la tarjeta de sugerencias (fondo blanco, `rounded-xl`, `ring-1 ring-slate-200`), con el texto alineado a la izquierda por ser prosa larga

## 4. Datos estructurados

- [x] 4.1 Añadir un único `<script type="application/ld+json">` con un nodo `Dataset` y un nodo `Person`
- [x] 4.2 Poblar el `Dataset` con `name`, `description`, `url` canónica, `inLanguage` `es-ES`, `isAccessibleForFree` true, `keywords` y `creator` apuntando al `Person`
- [x] 4.3 Declarar `temporalCoverage` como `2026-08-30/..` y confirmar que **no** se declara `dateModified`
- [x] 4.4 Confirmar que el marcado no contiene ningún nodo `Product`, `Offer`, `AggregateOffer` ni `ItemList`
- [x] 4.5 Confirmar que el marcado no contiene ningún `FAQPage` ni `SearchAction`
- [x] 4.6 Validar el JSON-LD con la herramienta de prueba de resultados enriquecidos de Google y con el validador de schema.org, y dejar constancia de que pasa sin errores

## 5. Directivas de rastreo

- [x] 5.1 Crear `robots.txt` en la raíz permitiendo todos los agentes y con la línea `Sitemap: https://dajorz.github.io/tracker-suplementos/sitemap.xml`
- [x] 5.2 Crear `sitemap.xml` en la raíz listando únicamente la URL canónica, sin elemento `<lastmod>`
- [x] 5.3 Tras desplegar, confirmar que ambos ficheros devuelven HTTP 200 con su contenido y no el HTML de la página

## 6. Instrumentación

- [x] 6.1 Crear en Search Console una propiedad de **prefijo de URL** sobre `https://dajorz.github.io/tracker-suplementos/` (la propiedad de dominio no es viable: requiere control DNS sobre `github.io`)
- [x] 6.2 Añadir la meta-etiqueta `google-site-verification` al `<head>`, precedida de un comentario de una línea que explique que borrarla desverifica la propiedad
- [x] 6.3 Completar la verificación en Search Console y enviar el sitemap
- [x] 6.4 Confirmar que la etiqueta de verificación no emite ninguna petición, no escribe cookies ni almacenamiento, y queda fuera de la puerta de consentimiento
- [ ] 6.5 Registrar la cifra de partida de impresiones y clics orgánicos, para poder evaluar este cambio dentro de unos meses con datos en lugar de intuición

## 7. Verificación de indexabilidad

- [ ] 7.1 Buscar en el HTML servido, fuera del iframe, las cadenas `HSN`, `MyProtein`, `Prozis` y `Zumub`, y confirmar que las cuatro aparecen
- [ ] 7.2 Buscar en el HTML servido, fuera del iframe, `monohidrato`, `Creapure`, `whey`, `isolate` y `caseína`, y confirmar que todas aparecen
- [ ] 7.3 Contar el texto indexable de la página y confirmar que la política de cookies ha dejado de ser su componente mayoritario
- [ ] 7.4 Listar los encabezados en orden de documento y confirmar que existe un `<h2>` sobre la materia del tracker, y no solo el CTA y la política de cookies
- [x] 7.5 Renderizar la página con JavaScript desactivado y confirmar que la sección de metodología, el JSON-LD y todos los metadatos siguen presentes

## 8. Verificación de presentación social

- [x] 8.1 Validar la URL en un depurador de tarjetas sociales y confirmar que se renderiza una tarjeta grande con imagen
- [x] 8.2 Comprobar la vista previa real en Telegram **antes** de anunciar nada en el canal, dado que su caché es difícil de invalidar
- [x] 8.3 Confirmar que el texto de la tarjeta se lee en la miniatura que muestra Telegram en móvil

## 9. Verificación de no regresión

- [x] 9.1 Medir la distancia desde el top del viewport hasta el borde superior del iframe a 375px de ancho y confirmar que sigue siendo de 220px o menos
- [x] 9.2 Confirmar que no se ha añadido absolutamente nada entre `</header>` y la sección del iframe
- [x] 9.3 Confirmar en un viewport de 900px de alto que el borde superior del iframe sigue visible sin hacer scroll
- [x] 9.4 Confirmar que el iframe conserva su `src`, su altura `80vh` con mínimo de 600px y su `loading="lazy"`
- [x] 9.5 Confirmar que el flujo de consentimiento sigue intacto: banner en primera visita, aceptar inyecta `gtag.js` una sola vez, rechazar borra las cookies `_ga` y recarga
- [x] 9.6 Confirmar que «Cookies» en el pie sigue reabriendo el banner y que «Más información» sigue abriendo el diálogo de política
- [x] 9.7 Confirmar que el CTA `mailto:` se sigue ensamblando por JavaScript y que la dirección no aparece literal en el HTML servido
- [x] 9.8 Confirmar que el clic en Telegram sigue emitiendo `join_telegram` solo con consentimiento aceptado
- [x] 9.9 Medir el contraste de todo el texto nuevo de la sección de metodología contra su fondo y confirmar que alcanza 4.5:1
- [x] 9.10 Confirmar que el favicon sigue siendo un data URI y que no se ha añadido ningún fichero binario de favicon
- [x] 9.11 Confirmar que sigue sin existir ningún script de build, ningún `package.json` y ninguna hoja de estilos propia
- [x] 9.12 Releer la declaración comercial y confirmar que no contiene ningún verbo en futuro ni ninguna palabra que prometa permanencia

## 10. Higiene del repositorio público

- [x] 10.1 Redactar la dirección de contacto literal en `openspec/changes/archive/2026-09-12-add-pricing-tracker-landing-page/` (design.md, proposal.md y dos líneas de tasks.md), sustituyéndola por una referencia descriptiva
- [x] 10.2 Buscar la dirección en todo el árbol de trabajo y confirmar que solo queda partida en fragmentos dentro de `index.html`
- [x] 10.3 Limpieza del historial de git: **descartada de forma deliberada**. Un `push --force` no elimina los objetos del alojamiento —siguen siendo alcanzables durante un plazo no garantizado, y purgar las vistas cacheadas exige abrir un ticket al proveedor—, mientras que redactar la rama por defecto ya cierra los tres vectores por los que entra una cosecha masiva: la búsqueda de código, la indexación de blobs y las URLs de fichero en crudo. Reescribir el historial y forzar dos ramas publicadas no compraba la garantía que aparentaba
- [x] 10.4 Revisar los artículos nuevos de este change y confirmar que no contienen datos personales, direcciones ni intenciones de negocio no anunciadas públicamente
