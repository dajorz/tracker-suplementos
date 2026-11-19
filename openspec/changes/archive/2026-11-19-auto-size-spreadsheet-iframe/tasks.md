# Tasks

## 1. Medición segura

- [x] 1.1 Obtener la publicación desde `index.html` con una petición compatible con CORS y sin reutilizar la respuesta en caché del iframe; verificar que se comprueban HTTP y estructura y que el iframe visible conserva su URL y carga diferida.
- [x] 1.2 Crear y limpiar una copia temporal aislada sin scripts ni elementos activos, con recursos restringidos y el ancho del iframe visible; verificar que no ejecuta código de la hoja ni aparece en el flujo o el árbol accesible.
- [x] 1.3 Hacer visible la pestaña en la copia, esperar las fuentes y medir la altura de la tabla y la barra horizontal; comparar las medidas con la hoja real en el navegador y comprobar que una tabla más corta puede reducir la altura.

## 2. Ajuste y recuperación

- [x] 2.1 Aplicar la altura calculada y recalcular al cambiar el ancho o la escala de pantalla, descartando resultados asíncronos obsoletos; comprobar cambios rápidos y que la última medida prevalece.
- [x] 2.2 Mantener una altura inicial y registrar fallos de red, CORS, estructura o medida sin ocultar el iframe; verificar esos fallos y JavaScript deshabilitado, incluida la posibilidad de recorrer toda la hoja.
- [x] 2.3 Actualizar `README.md` con el ajuste automático, sus dependencias, el modo de reserva y la necesidad de recargar para recoger cambios posteriores de la hoja; comprobar que no indica recalibrar por cada producto.

## 3. Verificación integrada

- [x] 3.1 En 375, 1280 y 1920 px, inspeccionar el iframe real tras la medición: `scrollHeight <= clientHeight`, scroll vertical sobre la tabla mueve la página, todas las columnas son alcanzables y la página no desborda horizontalmente.
- [x] 3.2 Probar con filas adicionales y eliminadas en un fixture controlado: al cargar cada versión cambia la altura sin editar constantes y no se recortan filas; verificar también que la copia no conserva espacio de una medición mayor previa.
- [x] 3.3 Repetir las comprobaciones relevantes en navegadores o motores disponibles con fuentes y barras distintas; documentar cualquier motor no verificado y no atribuirle compatibilidad probada.
- [x] 3.4 Comprobar que la URL, el estilo, el orden del contenido, la carga diferida, el consentimiento y el despliegue estático siguen funcionando, y revisar que las copias temporales se eliminan también en caso de error.
