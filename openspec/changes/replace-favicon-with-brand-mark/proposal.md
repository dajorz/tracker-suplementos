## Why

El favicon actual es un emoji 📊 dibujado en un data URI, y su diseño lo presentó expresamente como provisional hasta disponer de un icono propio. Ese icono ya existe: la marca circular del robot, adoptada como marca única en `improve-seo-indexability` y usada ya en la tarjeta social. Mantener el emoji deja la pestaña como la única superficie que no reconoce la marca.

## What Changes

- Sustituir el favicon emoji por dos ficheros ráster derivados de la marca circular, en la raíz del repositorio:
  - `favicon-v1.png`, 32×32, recortado al aro y con transparencia fuera del círculo.
  - `apple-touch-icon-v1.png`, 180×180, opaco sobre el navy de la marca, para la pantalla de inicio de iOS.
- En `index.html`, reemplazar el `<link rel="icon">` con data URI por un `<link rel="icon" type="image/png" sizes="32x32">` y un `<link rel="apple-touch-icon">` que apuntan a esos ficheros.
- Nombres versionados desde el primer día, con el mismo criterio que `og-image-v1.png`: los navegadores cachean el favicon por URL con mucha persistencia.
- El original de 1254×1254 no entra en el repositorio; solo se publican los derivados.
- **BREAKING (a nivel de spec)**: el requisito «Favicon» deja de exigir un data URI y deja de prohibir un fichero binario de favicon.
- No se toca nada más: ni el cuerpo de la página, ni el iframe, ni el consentimiento, ni los metadatos sociales o estructurados.

## Capabilities

### New Capabilities
(ninguna)

### Modified Capabilities
- `pricing-tracker-landing-page`: el requisito «Favicon» pasa de un emoji en data URI a ficheros ráster hermanos derivados de la marca propia del proyecto, y añade el icono de pantalla de inicio de iOS.

## Impact

- Ficheros afectados: `index.html` (una línea del `<head>` sustituida por dos) y dos ficheros PNG nuevos en la raíz.
- Sin build step: los derivados se generan una vez y se suben tal cual, igual que `og-image-v1.png`.
- Dependencia de orden: el requisito que admite ficheros hermanos en la raíz llega con `improve-seo-indexability`. Este change se puede implementar ya, pero se archiva después de aquél.
- Sin efecto en los resultados de Google: la web vive en un subdirectorio del hostname, y Google solo toma el favicon de la raíz del hostname.
