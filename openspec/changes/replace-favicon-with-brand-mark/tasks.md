## 1. Medición del original

- [ ] 1.1 Medir sobre los píxeles de `icon.png` el centro y el radio exterior del aro lima, a partir de los primeros píxeles no navy en cada eje. Registrar los valores antes de recortar nada
- [ ] 1.2 Confirmar que el color de las cuatro esquinas del original es `#051322`

## 2. Favicon de pestaña

- [ ] 2.1 Recortar el original a un cuadrado centrado en el aro, con lado igual al diámetro exterior del aro medido en 1.1
- [ ] 2.2 Reescalar a 32×32 con filtro bicúbico o Lanczos, nunca vecino más próximo
- [ ] 2.3 Aplicar una máscara circular con antialiasing: alfa 0 fuera del aro y borde suavizado, sin recortar el propio aro
- [ ] 2.4 Exportar como `favicon-v1.png` en la raíz, por debajo de 5 KB
- [ ] 2.5 Verificar que el fichero mide exactamente 32×32 y que sus cuatro esquinas tienen alfa 0

## 3. Icono de pantalla de inicio de iOS

- [ ] 3.1 Reescalar el original completo, con su margen navy, a 180×180 con filtro bicúbico o Lanczos
- [ ] 3.2 Aplanar sobre `#051322` para que ningún píxel quede transparente
- [ ] 3.3 Exportar como `apple-touch-icon-v1.png` en la raíz, por debajo de 30 KB
- [ ] 3.4 Verificar que el fichero mide exactamente 180×180 y que ningún píxel tiene alfa inferior a 255

## 4. Integración en la página

- [ ] 4.1 En `index.html`, sustituir el `<link rel="icon">` con data URI por `<link rel="icon" type="image/png" sizes="32x32" href="favicon-v1.png">`
- [ ] 4.2 Añadir justo debajo `<link rel="apple-touch-icon" href="apple-touch-icon-v1.png">`
- [ ] 4.3 Confirmar que en el `<head>` no queda ningún `data:` en un `rel="icon"` y que hay exactamente un `<link rel="icon">`

## 5. Original fuera del repositorio

- [ ] 5.1 Mover `icon.png` fuera del árbol de trabajo, a una ubicación del propietario
- [ ] 5.2 Confirmar con `git status` que `icon.png` no aparece y que los únicos ficheros nuevos son los dos PNG derivados
- [ ] 5.3 Confirmar que ningún fichero cuadrado de más de 180×180 píxeles queda rastreado en el repositorio

## 6. Validación local

- [ ] 6.1 Abrir `index.html` en local y comprobar que la pestaña muestra la marca circular en tema claro y en tema oscuro
- [ ] 6.2 Comprobar el icono a 16px (zoom del sistema al 100% en pantalla 1x o equivalente) y aceptar o rechazar explícitamente cómo se lee
- [ ] 6.3 Ejecutar `openspec validate replace-favicon-with-brand-mark` sin errores

## 7. Validación tras el despliegue

- [ ] 7.1 Pedir por HTTPS `favicon-v1.png` y `apple-touch-icon-v1.png` bajo `https://dajorz.github.io/tracker-suplementos/` y confirmar estado 200 con tipo de contenido `image/png`
- [ ] 7.2 Abrir la página servida en una ventana privada y confirmar el favicon nuevo en la pestaña y al guardarla como marcador
- [ ] 7.3 En un iPhone, «Añadir a pantalla de inicio» y confirmar que el icono muestra la marca completa sin que la máscara de esquinas toque el aro

## 8. Cierre

- [ ] 8.1 Archivar este change solo después de que `improve-seo-indexability` esté archivado, porque es el que admite ficheros hermanos en la raíz en la spec principal
