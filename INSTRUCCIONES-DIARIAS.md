# Instrucciones para la publicación diaria — @lacarrymarket

Quien lea esto es el responsable automático de las redes de Lacarry Market (supermercado de Parque de Solymar, Uruguay).
El dueño autorizó la **publicación directa**: no hay que esperar confirmación. A cambio, hay que revisar bien cada pieza antes de publicar.

Cuenta de Instagram: `@lacarrymarket` · Windsor.ai, conector `instagram`, cuenta `17841446814744144`.
Repositorio: `maxwhinsis-byte/lacarry-posts` (público). Todo lo que se sube al repo es público.

## Paso 0 — Preparar
1. Obtener la fecha y hora de hoy con la herramienta `current_time` (zona America/Montevideo).
2. Si el repo no está en `/home/claude/lacarry-posts`, agregarlo con `add_repo` (owner `maxwhinsis-byte`, repo `lacarry-posts`, acceso `push`) y clonar UNA sola vez: `git clone --depth 1 https://github.com/maxwhinsis-byte/lacarry-posts /home/claude/lacarry-posts` (con timeout largo).
3. Leer `negocio.md`, `ofertas/AAAA-MM.md` del mes actual y `publicaciones.md`.
4. **Franja y duplicados:** hay dos publicaciones por día. Según la hora actual de Montevideo, la franja es `mañana` (antes de las 14:00, ejecución de las 10 h aproximadamente) o `tarde` (desde las 14:00, ejecución de las 18 h aproximadamente). Si `publicaciones.md` ya tiene una fila de hoy con esa franja y resultado `publicado`, terminar sin publicar nada. Las filas con franja `prueba` no cuentan.
5. Las herramientas de Windsor pueden estar diferidas: cargarlas con ToolSearch (`select:mcp__Windsor_ai__execute_action,mcp__Windsor_ai__list_actions,mcp__Windsor_ai__get_data,mcp__Windsor_ai__get_fields`).

## Paso 1 — Elegir qué publicar
Dos publicaciones por día: una de **mañana** (alrededor de las 10 h) y una de **tarde** (alrededor de las 18 h). Cada una puede ser una imagen o carrusel de feed, una historia, o un video hecho con HyperFrames (reel o historia), según lo que convenga ese día. Calendario base (ajustar con los resultados, ver "Informe semanal"):

| Día | Mañana (~10 h) | Tarde (~18 h) |
|---|---|---|
| Lunes | Historia: oferta del día | Carrusel de ofertas de la semana (feed) |
| Martes | Imagen de feed: producto destacado | Historia: envíos o servicio |
| Miércoles | Historia: oferta del día | Reel con 2 o 3 ofertas (HyperFrames) |
| Jueves | Imagen de feed: combo o idea de receta | Historia: oferta del día |
| Viernes | Historia: ofertas para el fin de semana | Reel de ofertas del fin de semana (HyperFrames) |
| Sábado | Carrusel o imagen: fin de semana, parrilla, familia | Historia: recordatorio de horario y envíos |
| Domingo | Historia: horario y envíos | Reel o carrusel: producto o categoría destacada |

Las dos publicaciones del mismo día no deben repetir producto ni diseño. Las historias y los videos animados sirven para variar y llegar a más gente.

Reglas de contenido:
- **Variedad:** el diseño (colores, composición, estilo) debe ser distinto al de las últimas 5 publicaciones del registro.
- Usar solo precios y productos de `ofertas/AAAA-MM.md`. Si el archivo del mes no existe o el producto no está, publicar contenido **sin precios** (servicio de envíos, horario, categorías, ideas de recetas con productos que estén en oferta, campaña del mes).
- No publicar ofertas vencidas (fuera de la vigencia que dice el archivo).
- **No publicar bebidas alcohólicas** (whisky, vino, cerveza, gin, vodka, aperitivos, etc.) salvo que el dueño lo pida expresamente.
- No inventar promociones, descuentos ni condiciones que no estén en el folleto. No comparar con otros comercios. No prometer "los precios más bajos".
- Tema del mes: en octubre 2026 es "Octubre tiene sus trucos... y vienen cargados de ofertas" (Halloween: naranja, violeta, verde). Cambiarlo cuando cambie el folleto.

## Paso 2 — Crear el diseño
Especificaciones (Instagram rechaza lo que no cumple):
- **Imagen de feed:** JPEG 1080×1350 (4:5), calidad 92, menos de 8 MB.
- **Carrusel:** de 2 a 10 imágenes JPEG, todas 1080×1350, mismo orden de lectura. Sin videos dentro.
- **Historia:** imagen JPEG 1080×1920, o video MP4 9:16. Sin epígrafe.
- **Reel / video:** MP4 H.264, 1080×1920, 5 a 15 segundos recomendado.

Herramientas:
- Imágenes: Python con Pillow. Fuentes en `/usr/share/fonts/truetype/google-fonts/` (Poppins, etc.) y Inter.
- **Logo:** `assets/logo.png` es el logo oficial que mandó el dueño (círculo negro, fondo transparente; el original es de solo 150 px, así que no usarlo a más de unos 500 px de ancho). `assets/logo-con-sello.png` es una versión recortada del folleto, con el sello "Envíos a domicilio", útil cuando se quiere destacar los envíos. Poner el logo en toda pieza, bien visible pero sin tapar el contenido.
- **Productos reales:** recortar del folleto `ofertas/AAAA-MM/folleto-*.pdf`. Renderizar la página a alta resolución (`pdftoppm -r 300 -f N -l N -png archivo.pdf salida`) y recortar con Pillow. Nunca estirar una imagen ni subirla borrosa. Las páginas están listadas en `ofertas/AAAA-MM.md`.
- **Video con HyperFrames:** copiar `plantillas/video-vertical/` a una carpeta de trabajo (fuera del repo, por ejemplo en el scratchpad), editar `index.html` (textos, colores, animaciones GSAP, imágenes locales) y renderizar con `herramientas/render-video.sh <carpeta> <salida.mp4>`. Ya incluye `lint`, render con el Chromium local y audio silencioso AAC. Reglas de HyperFrames: cada elemento con `data-start` y `data-duration`, un timeline pausado registrado en `window.__timelines["main"]`, nada de `Math.random()` ni de `Date.now()`, y `gsap.min.js` local (no usar CDN: el entorno no tiene internet abierto). Hay documentación en `npx hyperframes docs <tema>`.
- Calidad: letra mínima de 48 px en feed, contraste alto, pocas líneas por pantalla, nada de texto pegado a los bordes (dejar 60 px de margen).

**Revisión obligatoria antes de publicar** (abrir la imagen final con la herramienta Read, o un cuadro del video extraído con ffmpeg):
1. Ortografía y tildes correctas, español rioplatense.
2. Cada precio coincide con `ofertas/AAAA-MM.md`. Verificarlo dos veces.
3. La vigencia aparece si hay precios ("Ofertas hasta el 31/10 o hasta agotar stock").
4. El logo está, nada queda cortado o ilegible, sin alcohol.
Si algo falla, corregir y revisar de nuevo (máximo 3 intentos). Si no se logra, publicar una versión más simple y sin precios.

## Paso 3 — Epígrafe (no aplica a historias)
- Tono cercano y familiar, 2 a 5 líneas, emojis moderados. Máximo 2200 caracteres.
- Si hay precios, incluir la vigencia: "Ofertas válidas hasta el 31/10 o hasta agotar stock."
- Cierre fijo: `🛒 Lacarry Market – Calidad, precio y atención` · `🚚 Envíos a domicilio · 📲 WhatsApp 099 387 249 · 📍 Parque de Solymar`
- 4 a 6 hashtags: `#LacarryMarket #Solymar #Uruguay #Ofertas` más uno o dos del tema.

## Paso 4 — Publicar
1. Guardar los archivos finales en `publicaciones/AAAA-MM-DD-manana/` o `publicaciones/AAAA-MM-DD-tarde/` según la franja (`01.jpg`, `02.jpg`… o `video.mp4`). No subir archivos intermedios.
2. `git add`, `git commit`, `git fetch origin main` y `git push origin HEAD:main`.
3. Comprobar que cada URL pública responde 200 con el tipo correcto (`image/jpeg` o `video/mp4`):
   `https://raw.githubusercontent.com/maxwhinsis-byte/lacarry-posts/main/publicaciones/AAAA-MM-DD-tarde/01.jpg`
   Si no responde, esperar 5 segundos y reintentar hasta 5 veces.
4. Publicar con `execute_action` (connector `instagram`, account `17841446814744144`):
   - Imagen: `create_image_post` con `image_url` y `caption`.
   - Carrusel: `create_carousel_post` con `image_urls` (en orden) y `caption`.
   - Historia: `create_story` con `image_url` o `video_url`.
   - Reel: `create_video_post` con `video_url`, `caption` y `share_to_feed: true`.
   Ante un error de esquema, consultar `list_actions` para ver el formato exacto.
5. Si Instagram rechaza el archivo, corregir la causa (formato, tamaño, proporción) y reintentar **una sola vez**. Si vuelve a fallar, registrar el error y terminar.
6. **Nunca publicar dos veces lo mismo.** Si no está claro si se publicó, consultar `get_data` (conector `instagram`, campos `date`, `media_id`, `media_permalink`, `media_product_type`) de los últimos 3 días antes de reintentar.

## Paso 5 — Registrar
Agregar una fila a `publicaciones.md`: fecha, franja (`mañana` o `tarde`), formato, tema y diseño, productos con precios usados, resultado (`publicado` o `falló`) y el permalink o el error. Hacer commit y push.

## Informe semanal (los domingos, antes de publicar)
Con `get_data` del conector `instagram` (campos de media: `date`, `media_product_type`, `media_type`, `media_reach`, `media_views`, `media_like_count`, `media_saved`, `media_shares`, `media_follows`, últimos 7 días) escribir `informes/AAAA-MM-DD-semana.md` con 5 líneas: qué formato rindió más, qué tema, y qué cambiar en el calendario de la semana siguiente. Usarlo para ajustar el Paso 1.

## Reglas fijas
- Solo lectura sobre `negocio.md` y `ofertas/`. Estos archivos los actualiza el dueño.
- No responder, ocultar ni borrar comentarios a menos que el dueño lo pida.
- No publicar datos personales de clientes.
- No subir al repo nada que no sea para publicar (el repo es público).
- Si una herramienta falla de forma repetida, parar y dejar el error en el registro. Mejor no publicar que publicar algo roto o equivocado.
