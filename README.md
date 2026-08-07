# Tarjeta digital — Eduardo I. Brito V.

Tarjeta de presentación digital de una sola página. Todo vive en `index.html`:
el fondo y la foto van incrustados en base64, así que no hay carpeta de assets
ni dependencias que instalar. La única petición externa es la fuente Poppins de
Google Fonts, y si falla el diseño cae a una tipografía del sistema.

## Publicar

Sirve desde cualquier hosting estático. Con GitHub Pages: **Settings → Pages →
Source: Deploy from a branch → `main` / `root`**. La URL queda en
`https://TU-USUARIO.github.io/NOMBRE-DEL-REPO/`.

Para probar en local basta con abrir el archivo, o levantar un servidor si
quieres que funcione la Web Share API (necesita HTTPS o localhost):

```bash
python3 -m http.server 8000
```

## Qué hace cada botón

| Botón | Acción |
|---|---|
| Save contact / Guardar contacto | Genera y descarga un `.vcf` (vCard 3.0) con la foto incrustada |
| Connect / Conectar | Abre el menú nativo de compartir; si no existe, copia el enlace |
| WhatsApp | `wa.me/593958938838` |
| Send mail / Enviar correo | `mailto:israedu_18@hotmail.com` |

## Editar los datos

Los textos están en dos lugares y hay que tocar ambos para que el selector
siga cuadrando:

1. El HTML visible, en los elementos con atributo `data-i18n`. Va en inglés,
   que es el idioma inicial.
2. El objeto `STRINGS` del `<script>`, con un bloque por idioma (`es`, `en`,
   `zh`). Las claves `title` (cargo corto para el vCard) y `auto` (tooltip del
   botón AUTO) no se pintan en el HTML, pero deben existir en los tres.

Para añadir un idioma: copia un bloque de `STRINGS`, tradúcelo, añade su
etiqueta BCP 47 a `HTML_LANG`, mete una línea en `detectLang()` y añade el
botón en `<div class="lang">`.

El teléfono aparece en tres sitios: el enlace `tel:`, el enlace `wa.me` y la
línea `TEL` del vCard.

Para cambiar la foto o el fondo hay que regenerar los `data:` URI; conviene
optimizar la imagen antes (WebP, ancho máximo 720 px para el fondo y 640 px
para el retrato) para que el archivo no se dispare de tamaño.

## Idioma

La tarjeta arranca **siempre en inglés** (es para uso en el extranjero). El
selector de la esquina ofrece cuatro opciones: `AUTO`, `ES`, `EN` y `中文`.
El HTML estático también está escrito en inglés para que no haya parpadeo antes
de que corra el JS.

`AUTO` recorre `navigator.languages` y muestra el primer idioma que esté
traducido; si el dispositivo está en francés, portugués o cualquier otro, cae a
inglés. No traduce por sí solo: solo elige entre los diccionarios que existen.
Ojo, un dispositivo en chino tradicional (`zh-TW`) recibe el simplificado.

Para cambiar el idioma inicial, edita la última línea del bloque de idioma:
`applyLang('en')` — acepta también `'auto'`. Para dejar la tarjeta en un único
idioma, elimina además el bloque `<div class="lang">`.
