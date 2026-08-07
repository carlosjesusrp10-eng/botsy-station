# CLAUDE.md

Guía para trabajar en este repositorio.

## El negocio

**Botsy Station** es una tienda de paso sobre la carretera, en el Libramiento
Manuel González, Altamira, Tamaulipas (C.P. 89613). **Carlos Rincón es el
dueño.** No es un cliente externo: las decisiones de contenido, precios, datos
y diseño son suyas.

Este repositorio está dedicado por completo a Botsy Station. Empezó como un
taller para producir landing pages de negocios locales, pero ese enfoque quedó
atrás: aquí solo se trabaja en la tienda y en lo que la tienda necesite.

### Qué la hace distinta

No es la tienda de carretera de siempre. Está tematizada al estilo del oeste
americano y funciona más como destino que como parada rápida:

- **Afuera:** granero de lámina verde con mural pintado, tipi, carreta de época,
  estatuas de caballo y vaca a tamaño real, y un poste con el logo.
- **Adentro:** techos altos con vigas de madera, candelabros hechos con ruedas
  de carreta, estrellas doradas y un mural de atardecer que cubre la pared del
  fondo. En el centro del salón hay un tigre a tamaño real que se ha vuelto la
  foto obligada de quien entra.
- **El producto es el gancho:** mercancía americana difícil de conseguir en la
  zona. Refrescos y energizantes (Dr Pepper, Mountain Dew, A&W, Prime, Celsius,
  Body Armor), botana (Cheetos Flamin' Hot, Doritos, Takis, Snyder's,
  Werther's), salsas y enlatados. Además sombreros vaqueros, bolsas de piel y
  de cuero de vaca, cinturones, joyería, perfumes, peluches y souvenirs.

El posicionamiento de la comunicación es ese: **no es una parada de cinco
minutos, es el motivo para desviarse.** Lo práctico (café, baños, parqueadero)
existe y se menciona, pero va en segundo plano.

### Datos reales del negocio

No inventar ninguno de estos. Si hace falta uno que no esté aquí, preguntar.

| Dato | Valor |
|---|---|
| Dirección | Carretera Libramiento Manuel González, Altamira, Tamaulipas, C.P. 89613 |
| Coordenadas | `22.745003, -98.2981235` |
| Horario | Todos los días, 6:00 a.m. – 11:00 p.m. |
| WhatsApp | +52 836 129 6173 |
| Correo | botsystation@gmail.com |
| Instagram | [@botsystation.mx](https://www.instagram.com/botsystation.mx) |
| Facebook | [perfil](https://www.facebook.com/profile.php?id=61590547208377) |
| Pagos | Efectivo, tarjeta y transferencia |
| Sitio | https://botsy-station.vercel.app (dominio propio pendiente) |

## La marca

El logo (`FOTOS/LOGO BOTSY STATION.jpeg`) es un mapache con sombrero vaquero y
pañuelo rojo, dentro de una estrella de sheriff dorada, rodeado por un anillo
con el texto «BOTSY STATION». De ahí sale toda la paleta:

| Variable | Hex | De dónde sale |
|---|---|---|
| `--ambar-oscuro` | `#9D350E` | terracota del texto y el anillo |
| `--ambar` | `#D19100` | dorado de la estrella |
| `--ambar-claro` | `#EDB021` | dorado aclarado, para fondos oscuros |
| `--rojo` | `#D60321` | pañuelo del mapache |
| `--negro` | `#241609` | café casi negro del sombrero |
| `--blanco` | `#FFFCF6` | crema, fondo cálido |

Ojo con el contraste: el terracota **no** se lee sobre fondo oscuro (2.4:1).
Sobre `--negro` hay que usar `--ambar-claro`.

**Tono:** directo, concreto y con humor seco. Nombrar el producto real y las
marcas, no adjetivos genéricos. «Entras a comprar un refresco y te quedas media
hora» funciona; «calidad y servicio» no.

## Este repositorio

```
index.html    la landing: HTML, CSS y JS en un solo archivo
img/          fotos optimizadas para web (WebP) que usa la página
FOTOS/        originales sin tocar, tal como salieron del celular
```

- **Sin build ni framework.** Se abre con doble clic. La única dependencia
  externa es Google Fonts (Archivo Black + Rubik), y con `display=swap` la
  página funciona igual sin red. Animaciones y menú son CSS y JS propios.
- **Despliegue:** Vercel, preset `Other`. Cada push a `main` publica.

Las de `img/` salen de `FOTOS/`: recortadas al encuadre que pide cada sección y
convertidas a WebP. Los originales del celular pesan más de 1 MB cada uno y no
deben referenciarse desde el HTML. Para regenerarlas se usa `sharp` desde un
directorio temporal, nunca instalado en el repositorio.

## Convenciones del código

- **Un solo archivo.** CSS en un `<style>` en el `<head>`, JS en un `<script>`
  al final del `<body>`. No sacar a archivos aparte a menos que haga falta de
  verdad.
- **Secciones numeradas.** El CSS está dividido con comentarios de banner
  (`1. VARIABLES Y BASE`, `2. HEADER`, …, `13. RESPONSIVE`). Al agregar CSS,
  ponerlo en su sección; si es una sección nueva, numerarla al final, antes de
  `RESPONSIVE`.
- **Colores y medidas por variables CSS** en `:root`. Nunca hardcodear un color
  que ya exista como variable.
- **Nombres en español:** `.contenedor`, `.seccion-oscura`, `.btn-primario`,
  `.producto-foto`, `.texto-apoyo`, `.eyebrow`.
- **Marcadores `TODO`.** Los datos que falten van con un comentario
  `<!-- TODO: ... -->` justo encima. Al recibir el dato real, reemplazar el
  valor **y borrar el TODO**. El README lista los pendientes.
- **Imágenes:** siempre `alt` descriptivo, `width` y `height`, y `loading="lazy"`
  salvo la portada. La regla base necesita `height:auto`, si no el atributo
  `height` del HTML anula los `aspect-ratio` del CSS.
- **Responsive.** Revisar siempre a 375px además de escritorio. Tipografías con
  `clamp()`, no breakpoints por tamaño de letra.
- **Accesibilidad mínima no negociable:** `alt` en imágenes, contraste 4.5:1 en
  texto, foco visible, `lang="es"`, y objetivos táctiles de 40px o más.

## Estructura de la landing

1. Header sticky con el logo, cuatro anclas y el botón de WhatsApp.
2. Portada a sangre con el video de nubes de la fachada al atardecer, el mapache
   y «Tu parada obligatoria», con CTA a Google Maps. El video va encima de una
   foto que es su propio primer fotograma; si no puede cargarse, queda la foto.
   Las nubes aceleran según la velocidad del scroll.
3. Franja de marcas americanas en desfile continuo.
4. Presentación breve: qué es Botsy Station.
5. «Lo que encuentras»: seis áreas alternando foto y texto — snacks y dulces,
   bebidas, productos importados, comida caliente, western y regalos, y para el
   camino. Debajo, las fichas de lo práctico.
6. «Conoce Botsy»: la panorámica del salón, el tigre y el video del recorrido.
7. Galería de momentos, en mosaico.
8. Reseñas de Google.
9. «Cómo llegar»: la fachada real de día, dirección, horario y contacto.
10. CTA de redes sociales.
11. Footer y barra fija de celular (Cómo llegar + WhatsApp).

Siempre: `<title>` y `meta description` con la zona, `canonical`, Open Graph
completo (`og:title`, `og:description`, `og:url`, `og:image`), datos
estructurados de `Store`, `theme-color` y favicon.

**El mapache aparece cuatro veces y no más**: portada, presentación, el área
«Para el camino» y el CTA de redes. Los tres archivos de `img/` son recortes del
mismo dibujo original. No se redibuja ni se le cambia el diseño.

## Flujo de trabajo

- **Rama por cambio** (`nueva-seccion-menu`, `actualizar-horario`), nunca
  commits directos a `main`.
- **Mensajes de commit en español**, en imperativo o descriptivo corto:
  `Actualiza el horario de temporada alta`.
- **Antes de dar por terminado un cambio visual, abrirlo y verlo** a 1280px y a
  375px. No basta con que el CSS parezca correcto.
- **No inventar datos del negocio.** Dirección, teléfono, horarios, precios y
  existencias los da Carlos. Si falta uno, dejar el `TODO` y avisar.
- **Cuidado al publicar:** cada merge a `main` sale al aire de inmediato.
