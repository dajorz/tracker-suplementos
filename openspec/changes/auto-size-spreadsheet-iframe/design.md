# Design

## Context

La página se sirve como un único HTML estático en GitHub Pages. El iframe visible carga `pubhtml` desde Google Sheets y la política de mismo origen impide que la página lea su DOM. El navegador puede solicitar la publicación para medir una copia, siempre que Google permita esa lectura.

En una prueba exploratoria, la publicación respondió a una petición CORS sin caché; el mismo URL reutilizado desde la caché del iframe falló por CORS. La hoja real y una copia aislada sin scripts midieron 1402,6 px en el navegador probado y 1465 px tras añadir tres filas sintéticas. La copia necesitó hacer visible el contenedor de la pestaña porque Google lo oculta inicialmente. Estos resultados no garantizan la compatibilidad con otros navegadores o cambios de Google.

## Goals / Non-Goals

**Goals:**
- Medir en el navegador del visitante, sin depender de un número fijo de filas ni de una altura por producto.
- Mantener utilizable la hoja si no se puede obtener una medida fiable.
- Evitar que la copia de medición ejecute código externo o interfiera con el documento principal.

**Non-Goals:**
- Sustituir el iframe visible, modificar la hoja, añadir un servidor o reconstruir una tabla propia.
- Garantizar el ajuste si Google deja de permitir la lectura o cambia su HTML de forma incompatible.
- Actualizar en tiempo real una página ya abierta cuando la hoja cambia; la siguiente carga obtiene una nueva medida.

## Decisions

### Medir una copia aislada de la publicación

Solicitar el mismo documento publicado con una estrategia sin caché que evite reutilizar la respuesta del iframe sin cabeceras CORS. Verificar el estado HTTP y la estructura esperada antes de usarla. Crear una copia temporal invisible, del mismo ancho que el iframe visible, en un iframe `srcdoc` aislado mediante `sandbox` sin `allow-scripts`; no ejecutar scripts ni manejadores de eventos del HTML remoto. Limitar los recursos que la copia puede cargar y evitar navegación o envíos, sin impedir las fuentes necesarias para una medición equivalente. Retirar la copia al terminar, también si se produce un error.

Reproducir el estado visible de la pestaña publicada sin ejecutar los scripts de Google, esperar a las fuentes y medir la altura renderizada real de la tabla y los elementos necesarios de su contenedor. No tomar directamente el `scrollHeight` de un viewport alto: puede ser como mínimo su `clientHeight` e impedir que el iframe se reduzca. Sumar el espacio de la barra horizontal medida y un pequeño margen de redondeo, sin recortar contenido. Validar que la tabla y la medida son plausibles antes de aplicar el resultado.

Se descartan el acceso directo al DOM del iframe y `ResizeObserver` sobre él: observan el elemento exterior, no el documento de otro origen. Contar filas o fijar una altura amplia no refleja los saltos de línea ni las fuentes y solo posterga el problema. Una tabla propia o un proxy cambiarían la solución y su mantenimiento.

### Recalcular y gestionar resultados obsoletos

Medir al cargar y cuando cambie el ancho efectivo del iframe, incluyendo rotación o cambio de tamaño. Repetir la medición si cambia la escala efectiva de pantalla al mover la ventana entre monitores, aunque el ancho CSS no cambie. Reutilizar la publicación obtenida durante la carga para los recálculos; no consultar periódicamente ni recargar el iframe visible. Controlar las mediciones asíncronas para que un resultado anterior no sobrescriba el correspondiente al ancho o escala más recientes. Conservar `loading="lazy"` y el iframe original; la copia temporal no deberá ocupar espacio ni ser navegable o anunciada por tecnologías de asistencia.

### Fallar sin perder acceso a la hoja

Conservar una altura inicial de reserva para JavaScript deshabilitado y errores. Ante fallo de red, CORS, estructura inesperada, fuente o medida inválida, registrar un error identificable y conservar la altura inicial o la última medida válida; el iframe debe seguir visible y permitir scroll vertical y horizontal. No afirmar que la ausencia de scroll vertical está garantizada en este estado.

## Risks / Trade-offs

- [HTML o CORS de Google cambian] → Validar estructura y respuesta; conservar el iframe utilizable y registrar el fallo. La integración depende de un formato externo no contractual.
- [La copia y el iframe real difieren por fuentes, scripts, estilos, barras o cambios entre peticiones] → Medir con el mismo ancho, esperar fuentes y verificar contra el iframe real en varios navegadores; usar margen acotado y no prometer equivalencia universal.
- [La copia carga recursos o ejecuta contenido externo] → Eliminar scripts y elementos activos, aislar sin permiso de scripts y restringir recursos; comprobar explícitamente que no se ejecuta código de la copia.
- [Medidas obsoletas o limpieza incompleta] → Ignorar resultados antiguos y liberar observadores y copias temporales; probar cambios rápidos de ancho y errores.
- [Cambios de hoja con la página abierta] → Documentar que el ajuste se actualiza al recargar; la copia y el iframe se solicitan por separado y pueden no representar exactamente la misma revisión.

## Migration Plan

Actualizar el HTML y la documentación, probar localmente y publicar con GitHub Pages como hasta ahora. Si hay una regresión, revertir el cambio restaura el iframe con altura fija; no hay migración de datos.
