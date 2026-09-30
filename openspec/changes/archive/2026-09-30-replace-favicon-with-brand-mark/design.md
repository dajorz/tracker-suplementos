## Context

`index.html` declara hoy el favicon como un emoji 📊 dentro de un data URI SVG. La spec vigente lo exige así, con la prohibición expresa de cualquier fichero binario de favicon. El diseño archivado de `2026-09-17-add-emoji-favicon` lo presentaba como provisional y aplazaba «a real designed icon». Este change resuelve ese aplazamiento.

La marca ya existe. `improve-seo-indexability` adoptó como marca única la composición circular del robot con coctelera, y la usa en `og-image-v1.png`. El original es un PNG cuadrado de 1254×1254 y 1,18 MB, con fondo `#051322`, un aro lima y el motivo dentro del aro.

Restricciones que atraviesan el proyecto:
- No hay build step. Cualquier derivado se genera una vez y se sube tal cual.
- El requisito que admite ficheros estáticos hermanos en la raíz llega con `improve-seo-indexability`, todavía activo. La spec principal aún dice «single self-contained `index.html`».
- La web se sirve en un subdirectorio: `https://dajorz.github.io/tracker-suplementos/`.

## Goals / Non-Goals

**Goals:**
- Que la pestaña, los marcadores y la pantalla de inicio de iOS muestren la marca del proyecto.
- Publicar solo lo que se sirve, con un peso proporcional a su uso.

**Non-Goals:**
- El favicon en los resultados de Google. No es alcanzable desde este repositorio (decisión 5).
- Una variante simplificada para 16px. Es trabajo de diseño gráfico, no de esta integración (ver riesgos).
- Manifiesto web, iconos PWA, `favicon.ico` o variantes por tema de color.

## Decisions

### 1. Ficheros hermanos, no PNG en base64 inline

Un PNG de 32px en base64 dentro del `href` habría mantenido la letra del requisito actual y no dependería de ningún otro change. Se descarta por dos motivos:
- iOS no tiene equivalente inline razonable para el icono de pantalla de inicio.
- Mete 3–4 KB de base64 opaco en el `<head>`, que se descargan en cada visita aunque el navegador ya tenga el icono en caché.

Con ficheros separados, la caché del navegador funciona por URL como con cualquier otro recurso.

*Alternativa considerada:* servir `icon.png` directamente. Se descarta: 1,18 MB para pintar 32 píxeles.

### 2. Dos tamaños: 32 y 180

- **32×32** para `rel="icon"`. Es el tamaño real de la pestaña en pantallas densas. En pantallas 1x el navegador lo reduce a 16.
- **180×180** para `rel="apple-touch-icon"`. Es el tamaño que iOS usa en los iPhone actuales, y lo reduce para los demás dispositivos.

No se añaden 16, 48 ni 192. Cada tamaño extra es otro fichero a mantener sincronizado con la marca, sin ninguna superficie que lo necesite hoy.

### 3. El favicon de pestaña se recorta al aro y es transparente fuera; el de iOS es opaco

A 16px, la imagen completa se lee como «mancha blanca con aro verde». El margen navy que rodea el aro ocupa alrededor de un 8% por lado, y a ese tamaño cada píxel cuenta. Recortando al borde exterior del aro, el motivo gana del orden de 2px útiles a 32px, y a 32px el robot se reconoce.

Fuera del círculo, alfa 0 con borde suavizado. Así el icono es un círculo limpio tanto en barras de pestañas claras como oscuras. Con el fondo navy cuadrado, en tema claro se vería como un cuadrado oscuro.

El `apple-touch-icon` sigue el criterio contrario:
- iOS rellena de negro cualquier transparencia, y aplica su propia máscara de esquinas redondeadas.
- Se usa la imagen completa, con su margen navy y opaca de borde a borde. El margen evita que la máscara de iOS toque el aro.

El centro y el radio del aro **se miden sobre los píxeles del original**, no se estiman. Un recorte descentrado de 1–2 px en origen se ve como un aro de grosor irregular.

### 4. Nombres versionados: `favicon-v1.png` y `apple-touch-icon-v1.png`

Aplica el mismo razonamiento que la decisión 4 de `improve-seo-indexability` para `og-image-v1.png`. Los navegadores cachean el favicon con una persistencia notoria y sin mecanismo de invalidación al alcance. Con un nombre fijo, un rediseño futuro tardaría semanas en llegar a quien ya visitó la página. El sufijo no cuesta nada hoy y convierte ese problema en un cambio de una línea.

La guía de Google de mantener estable la URL del favicon no aplica, por la decisión 5.

### 5. El favicon no aparecerá en Google, y se asume

Google Search admite un único favicon por hostname y no reconoce home pages en subdirectorio. Para `dajorz.github.io/tracker-suplementos/`, el favicon de las SERP sería el de `dajorz.github.io/`, que no pertenece a este repositorio. No existe ni se prevé un sitio en esa raíz ni un dominio propio.

Consecuencias:
- No se optimiza para requisitos de Google, como los múltiplos de 48 px.
- Este change no contamina la línea base de CTR orgánico que registra `improve-seo-indexability`.

### 6. El original no entra en el repositorio

El repositorio es público y GitHub Pages sirve todo lo que contiene. Subir el original publicaría 1,18 MB que ninguna página referencia. Se mantiene fuera del árbol de trabajo. No se crea un `.gitignore` para un único fichero, porque sería otra pieza más que mantener.

Si hace falta regenerar los derivados, el original se trae de vuelta temporalmente. No hay build step que lo necesite en el repositorio.

### 7. Generación manual, una vez

Los dos PNG se producen con cualquier herramienta capaz de reescalar con filtro de calidad (bicúbico o Lanczos, nunca vecino más próximo) y de aplicar una máscara circular con antialiasing. Se suben como resultado final.

No es un build step, por el mismo criterio que se aplicó a `og-image-v1.png`: preparar un fichero antes del commit es autoría, no procesamiento previo al despliegue.

Presupuestos de peso: favicon por debajo de 5 KB, icono de iOS por debajo de 30 KB.

## Risks / Trade-offs

- **[A 16px el robot no se distingue]** → Se acepta. En pantallas 1x el icono se lee como aro lima con relleno claro, que sigue siendo distintivo y coherente con la marca. Si molesta, una variante simplificada (solo la cara del robot) es un change de diseño aparte, y el nombre versionado permite publicarla sin conflicto de caché.
- **[Orden de archivado]** → Si este change se archiva antes que `improve-seo-indexability`, la spec principal exigiría un único fichero y a la vez dos PNG hermanos. Mitigación: implementar cuando se quiera, y archivar este change estrictamente después de aquél. Queda como tarea explícita.
- **[`improve-seo-indexability` declara «no se toca el favicon»]** → Era cierto para su alcance, y su tarea 9.10 comprobó el estado de entonces. Este change no modifica aquél ni reabre esa tarea. Se limita a cambiar el favicon después.
- **[Caché del navegador durante la validación]** → Tras desplegar, el favicon viejo puede seguir apareciendo en el navegador de quien valida. El nombre nuevo lo evita en la práctica. Si aun así persiste, se valida en una ventana privada.
- **[Máscara de iOS sobre el aro]** → Si el aro quedara demasiado cerca del borde, las esquinas redondeadas de iOS lo tocarían. Mitigación: se conserva el margen navy del original en el icono de 180 px.

## Migration Plan

1. Generar los dos PNG y colocarlos en la raíz.
2. Sustituir la línea del favicon en `index.html` por los dos `<link>`.
3. Sacar el original del árbol de trabajo.
4. Desplegar con el push a la rama publicada y validar sobre la URL servida.
5. Archivar después de `improve-seo-indexability`.

Rollback: restaurar la línea del data URI en `index.html`. Los PNG pueden quedarse sin referencias o borrarse; sin enlace, nadie los pide.
