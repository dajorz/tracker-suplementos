## 1. Cabecera y metadatos

- [x] 1.1 Cambiar el `<h1>` a "NutriChollos" y la descripción a "Tracker independiente de precios reales de proteína y creatina en España"
- [x] 1.2 Cambiar `<title>`, `og:title` y `twitter:title` a "Precios de creatina y proteína en España | NutriChollos"
- [x] 1.3 Cambiar `meta description`, `og:description` y `twitter:description` a la cadena acordada en el diseño; confirmar que tiene 160 caracteres o menos
- [x] 1.4 Confirmar que la descripción de la cabecera cabe en una línea a 864 px y en dos a 375 px

## 2. Datos estructurados

- [x] 2.1 Añadir al `@graph` los nodos `Organization` (con `logo`, `sameAs` y `founder`) y `WebSite` (con `publisher`)
- [x] 2.2 Añadir `publisher` al `Dataset`, manteniendo `creator`
- [x] 2.3 Confirmar que hay un único script JSON-LD, sin `SearchAction`, `Product` ni `Offer`
- [x] 2.4 Validar en la herramienta de resultados enriquecidos de Google y en validator.schema.org. 2026-11-28: Google, Dataset válido (solo aviso opcional "license"); schema.org, Dataset y WebSite con 0 errores y 0 advertencias

## 3. Cuentas oficiales

- [x] 3.1 Añadir al pie la línea de cuentas oficiales con los tres enlaces de texto
- [x] 3.2 Confirmar a mano que las tres URLs abren el perfil correcto
- [x] 3.3 Buscar en todo el repo y confirmar que no aparece `tiktok.com/@nutrichollos` sin el sufijo `_tracker`

## 4. Leyenda

- [x] 4.1 Añadir la leyenda bajo el iframe: cuatro estados de fila con muestra y texto, y la explicación del rango
- [x] 4.2 Comprobar que los colores de las muestras coinciden con los de la hoja publicada
- [x] 4.3 Medir el contraste del texto de la leyenda (mínimo 4.5:1)

## 5. No regresión

- [x] 5.1 Confirmar que entre `</header>` y el iframe solo sigue estando la franja de Telegram, y que el preámbulo mide 220 px o menos a 375 px
- [x] 5.2 Confirmar que la medición automática de la altura del iframe sigue funcionando
- [x] 5.3 Confirmar que el flujo de consentimiento, el botón «Cookies» y el `mailto:` siguen funcionando
- [x] 5.4 Tras desplegar, pedir la indexación de la URL en Search Console y anotar la fecha para seguir la consulta "nutrichollos" en Rendimiento. Indexación solicitada el 2026-11-28; revisar la consulta en una o dos semanas
