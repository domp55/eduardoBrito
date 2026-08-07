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
| Guardar contacto | Genera y descarga un `.vcf` (vCard 3.0) con la foto incrustada |
| Conectar | Abre el menú nativo de compartir; si no existe, copia el enlace |
| WhatsApp | `wa.me/593958938838` |
| Enviar correo | `mailto:israedu_18@hotmail.com` |
| X | **Pendiente:** apunta a `https://x.com/` — reemplazar por el perfil real |

## Editar los datos

Los textos están en dos lugares y hay que tocar ambos para que el selector
ES/EN siga cuadrando:

1. El HTML visible, en los elementos con atributo `data-i18n`.
2. El objeto `STRINGS` del `<script>`, que guarda las dos versiones de idioma.

El teléfono aparece en tres sitios: el enlace `tel:`, el enlace `wa.me` y la
línea `TEL` del vCard.

Para cambiar la foto o el fondo hay que regenerar los `data:` URI; conviene
optimizar la imagen antes (WebP, ancho máximo 720 px para el fondo y 640 px
para el retrato) para que el archivo no se dispare de tamaño.

## Idioma

Al cargar detecta el idioma del navegador y arranca en inglés solo si es `en-*`.
Para dejar la tarjeta en un único idioma, elimina el bloque `<div class="lang">`
y fija el valor inicial de la variable `lang`.
