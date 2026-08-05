# Botsy Station — Landing Page

Landing page de **Botsy Station**, tienda americana sobre el Libramiento Manuel
González en Altamira, Tamaulipas: refrescos y botana de Estados Unidos,
sombreros vaqueros, bolsas de piel y souvenirs.

Sitio estático de un solo archivo ([`index.html`](index.html)), sin dependencias
ni proceso de build.

## Estructura

```
index.html    HTML + CSS + JS, todo junto
img/          fotos optimizadas para web (WebP) que usa la página
video/        el recorrido comprimido para web (MP4) que usa la página
FOTOS/        originales sin tocar, tal como salieron del celular
videos/       originales de video (fuera de git: pesan demasiado)
```

Las de `img/` salen de `FOTOS/`: recortadas al encuadre que pide cada sección y
convertidas a WebP. Los originales se conservan para poder rehacer un recorte
sin volver a pedir la foto.

`video/tienda.mp4` sale de `videos/VIDEO RECORRIENDO LA TIENDA.mov` (41 MB, 4K).
Se comprime con `ffmpeg` a 960 px de ancho, sin audio y con `faststart`, lo que
lo deja en 2 MB:

```
ffmpeg -i "videos/VIDEO RECORRIENDO LA TIENDA.mov" -an -vf scale=960:-2 \
  -c:v libx264 -profile:v main -pix_fmt yuv420p -crf 33 -preset veryslow \
  -g 60 -movflags +faststart video/tienda.mp4
```

Los originales de video **no van a git**: 41 MB por archivo inflan el
repositorio para siempre. Se quedan en la carpeta `videos/`, ignorada.

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

- **Dominio propio.** `og:url` y `og:image` apuntan a `botsy-station.vercel.app`.
  Cuando se compre el dominio hay que cambiar las dos etiquetas.
- **El microondas.** La ficha «Microondas para tu sopa» y la tarjeta «Para comer
  aquí mismo» dan por hecho que el cliente puede usarlo. Falta que Carlos lo
  confirme; si es solo para el personal, hay que quitar la ficha y reescribir la
  tarjeta.

Datos ya reales y verificados: dirección, horario, WhatsApp, correo, Google Maps,
Instagram y Facebook. Las reseñas y la calificación (5,0 sobre 5 reseñas) salen
del perfil de Google del negocio.

## Fuera de la página

- **La ficha de Google no tiene sitio web.** Dice «Añadir sitio web». Poner ahí
  `botsy-station.vercel.app` manda tráfico directo y es gratis.

## Fotos que faltarían

La página funciona con lo que hay, pero mejoraría con:

- Fachada en mayor resolución (la actual es de 1195 px de ancho; en pantallas
  grandes se nota un poco blanda).
- Los baños y el parqueadero, que hoy se mencionan sin foto.
- La nevera de refrescos de cerca: la foto actual (`nevera.webp`) es la más
  floja de las seis tarjetas.
