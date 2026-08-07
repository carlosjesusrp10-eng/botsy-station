# Botsy Station — Landing Page

Landing page de **Botsy Station**, tienda americana sobre el Libramiento Manuel
González en Altamira, Tamaulipas: refresco y botana importada, dulcería, comida
caliente, sombreros vaqueros y accesorios para el camino.

Sitio estático de un solo archivo ([`index.html`](index.html)), sin proceso de
build.

## Estructura

```
index.html    HTML + CSS + JS, todo junto
img/          fotos optimizadas para web (WebP) que usa la página
video/        los videos comprimidos para web (MP4) que usa la página
FOTOS/        originales sin tocar, tal como salieron del celular
videos/       originales de video (fuera de git: pesan demasiado)
```

Las de `img/` salen de `FOTOS/`: recortadas al encuadre que pide cada sección y
convertidas a WebP. Los originales se conservan para poder rehacer un recorte
sin volver a pedir la foto.

### Dependencias

Una sola, y es externa: **Google Fonts** (Archivo Black para los titulares,
Rubik para el resto). Se carga con `preconnect` y `display=swap`, así que si no
hay red la página se ve igual con la tipografía del sistema. Si algún día no se
quiere depender de Google, se descargan los dos `.woff2` a `fonts/` y se cambia
el `<link>` por un `@font-face`.

Lo demás —animaciones, menú, parallax— es CSS y JavaScript propios. Sin
framework, sin librerías, sin build.

## Las imágenes

Se regeneran con `sharp` desde un directorio temporal, nunca instalado en el
repositorio:

```bash
npm install sharp
```

**El mapache.** Sale de `FOTOS/efa27d6d-9107-4deb-81ca-3a4d17c70442.png`, que
viene con fondo blanco y sin transparencia. El fondo se quita con un relleno por
inundación desde los bordes, no con un umbral global: la cara tiene blancos
(hocico y ojos) que hay que conservar. De ahí salen tres recortes —
`mapache.webp`, `mapache-medio.webp` y `mapache-cabeza.webp` — que son encuadres
del mismo dibujo, sin redibujar nada.

Ojo con el alfa: hay que cortar a cero los valores por debajo de 24. El fondo del
PNG no es blanco puro y sin ese corte queda un velo de alfa 9 sobre toda la caja,
que se ve como un rectángulo tenue y multiplica el peso del archivo por cuatro.

**La portada.** `hero-atardecer.webp` (escritorio) y `hero-atardecer-movil.webp`
(vertical, para pantallas de menos de 700 px). **Salen del primer fotograma del
video de nubes**, no de una foto aparte: así, cuando el video se enciende encima,
no se nota ningún salto. Si se recodifica el video, hay que volver a sacarlos.

**La galería.** Sus ocho fotos se muestran como mucho en huecos de 273 px, así
que se guardan a 620 px de ancho. Antes estaban a 1050 y sobraban 820 KB.

## Los videos

**Las nubes de la portada.** Salen de `VIDEOS/VIDEO NUBES EN MOVIMIENTO.mp4`
(3840×2160, HEVC, 5 s, 11 MB). Hay tres cosas que arreglar al codificarlo, y
ninguna es opcional:

1. **Viene en HEVC/H.265, que Chrome y Firefox no reproducen.** Hay que pasarlo
   a H.264 sí o sí.
2. **Trae una marca de agua «Ai»** en la esquina superior izquierda, en la caja
   `x 94–163, y 91–159` del fotograma original. Se quita recortando el 9%
   superior. Si algún día llega un original sin marca, ese recorte sobra.
3. **La cámara se va abriendo**, así que en bucle daría un salto seco. Se
   codifica en ida y vuelta (el clip y luego el mismo clip al revés), lo que
   enlaza solo.

```bash
ffmpeg -y -t 3.2 -i "VIDEOS/VIDEO NUBES EN MOVIMIENTO.mp4" -filter_complex "[0:v]crop=iw:ih*0.91:0:ih*0.09,scale=1600:-2,setsar=1,split[a][b];[b]reverse[r];[a][r]concat=n=2:v=1[v]" -map "[v]" -an -c:v libx264 -profile:v high -pix_fmt yuv420p -crf 24 -preset slow -x264-params "aq-mode=3" -g 60 -movflags +faststart video/nubes.mp4
```

La versión de celular es la misma receta con un recorte vertical
(`crop=iw*0.42:ih*0.91:iw*0.29:ih*0.09`), `scale=720:-2` y `-crf 27`, que la
deja en `video/nubes-movil.mp4`.

**Tres cosas de esa orden no son adorno, y saltárselas emborrona el cielo**
—que es justo donde más se nota, porque es un degradado grande:

- **`-profile:v high`**, no `main`. El perfil `main` desactiva el transformado
  8×8 y a igual peso pierde bastante definición. `high` lo soportan todos los
  navegadores que importan; el `main` del video de la tienda es herencia, no
  un requisito.
- **Nada de `fps=25`.** El original va a 30, y bajarlo a 25 no es una división
  exacta: reparte los fotogramas de forma desigual y mete micro-tirones.
- **`-t 3.2` en vez del clip entero.** El bucle de ida y vuelta sale de 6,4 s
  en lugar de 10, y esos fotogramas que se ahorran se reparten entre los que
  quedan. Da la calidad de un CRF 24 por el peso de un CRF 27.

La primera versión que se publicó iba en `main`, CRF 31 y 25 fps: pesaba
949 KB y se veía blanda, con el letrero de la entrada convertido en mancha.

Los pósters salen del video ya codificado, no del original:

```bash
ffmpeg -y -i video/nubes.mp4 -frames:v 1 poster-ancho.png
```

**El recorrido por la tienda.** `video/tienda.mp4` sale de
`videos/VIDEO RECORRIENDO LA TIENDA.mov` (41 MB, 4K).
Se comprime con `ffmpeg` a 960 px de ancho, sin audio y con `faststart`, lo que
lo deja en 2 MB:

```bash
ffmpeg -i "videos/VIDEO RECORRIENDO LA TIENDA.mov" -an -vf scale=960:-2 -c:v libx264 -profile:v main -pix_fmt yuv420p -crf 33 -preset veryslow -g 60 -movflags +faststart video/tienda.mp4
```

Los originales de video **no van a git**: 41 MB por archivo inflan el
repositorio para siempre. Se quedan en la carpeta `videos/`, ignorada.

## Peso

| | |
|---|---|
| HTML | 64 KB |
| Carga inicial en celular | 262 KB (+ ~50 KB de fuentes) |
| Video de nubes | 558 KB en celular, 1.693 KB en escritorio |
| Página entera, con todo el scroll | 2,2 MB en 23 imágenes |
| Recorrido por la tienda | 2,1 MB, y solo si el visitante le da al play |

Todas las imágenes llevan `loading="lazy"` menos tres, que están en la primera
pantalla: el logo, la portada y el mapache.

**El video de nubes no se descarga siempre.** El `<video>` sale sin `src`: se lo
pone el JavaScript, y solo si el visitante no pidió menos movimiento, no tiene el
ahorro de datos activado y no va por una red 2G. Quien entre desde la carretera
con mala señal se queda con la foto, que es el mismo fotograma y ya está pintada.
El video también se pausa solo al pasar de largo la portada, para no seguir
descodificando fotogramas que nadie ve.

## Desarrollo local

Abre `index.html` con doble clic en el navegador. Eso es todo.

## Despliegue en Vercel

Este repositorio se despliega automáticamente en Vercel:

1. En [vercel.com](https://vercel.com) → **Add New → Project**.
2. Importa este repositorio de GitHub.
3. Framework Preset: **Other** (es un sitio estático, sin build).
4. **Deploy**.

Cada `push` a la rama principal genera un nuevo despliegue automáticamente.

## Pendiente

Dos `TODO` abiertos en `index.html`:

- **Dominio propio.** `canonical`, `og:url`, `og:image` y el bloque de datos
  estructurados apuntan a `botsy-station.vercel.app`. Los cuatro sitios están
  marcados con el comentario `DOMINIO` para cambiarlos de una pasada.
- **La foto de «Para el camino».** Es la única de las seis áreas que no tiene
  fotografía: ninguna de las 27 imágenes de `FOTOS/` muestra los cargadores, los
  cables ni los audífonos. La vitrina negra que lo parecía es en realidad la de
  perfumes, joyería y encendedores. Mientras tanto el bloque va con tratamiento
  gráfico y el mapache. Cuando llegue la foto, se sustituye el `.area-grafico`
  por un `.area-foto` igual que los otros cinco.

Datos ya reales y verificados: dirección, horario, WhatsApp, correo, Google Maps,
Instagram y Facebook. Las reseñas y la calificación (5,0 sobre 5 reseñas) salen
del perfil de Google del negocio. El microondas está confirmado como de uso para
el cliente.

## Fuera de la página

- **La ficha de Google no tiene sitio web.** Dice «Añadir sitio web». Poner ahí
  `botsy-station.vercel.app` manda tráfico directo y es gratis.

## Fotos que mejorarían la página

- El exhibidor de tecnología y accesorios (ver el `TODO` de arriba). Es la que
  más falta hace.
- Los baños y el parqueadero, que hoy se mencionan sin foto.
- La fachada de día en mayor resolución: la actual es de 1195 px de ancho y en
  pantallas grandes se nota un poco blanda.

## Preparado, pero sin hacer

- **Versión en inglés.** La página va solo en español. El `<head>` tiene el sitio
  marcado para añadir los `<link rel="alternate" hreflang="…">` cuando exista
  una `/en/`.

## Imágenes que ya no usa la página

`barra-cafe.webp`, `comida-caliente.webp`, `logo.webp` y `tigre.webp` quedaron
del diseño anterior. No molestan —nadie las descarga— pero se pueden borrar si
se quiere dejar `img/` limpio.
