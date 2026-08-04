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
FOTOS/        originales sin tocar, tal como salieron del celular
```

Las de `img/` salen de `FOTOS/`: recortadas al encuadre que pide cada sección y
convertidas a WebP. Los originales se conservan para poder rehacer un recorte
sin volver a pedir la foto.

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

Un solo `TODO` abierto en `index.html`:

- **Dominio propio.** `og:url` y `og:image` apuntan a `botsy-station.vercel.app`.
  Cuando se compre el dominio hay que cambiar las dos etiquetas.

Datos ya reales y verificados: dirección, horario, WhatsApp, correo, Google Maps,
Instagram y Facebook.

## Fotos que faltarían

La página funciona con lo que hay, pero mejoraría con:

- Fachada en mayor resolución (la actual es de 1195 px de ancho; en pantallas
  grandes se nota un poco blanda).
- El café, los baños y el parqueadero, que hoy se mencionan sin foto.
