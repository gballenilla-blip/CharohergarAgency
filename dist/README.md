# Charohergar Agency — carpeta de despliegue

Sitio de una sola página, 100 % estático. **No hay build step**: `index.html`
es el sitio entero (CSS y JS en línea; librerías por CDN).

```
dist/
├── index.html            ← el sitio
├── _headers              ← cabeceras de caché/seguridad (solo Cloudflare Pages)
├── robots.txt
├── sitemap.xml
├── .gitignore
└── assets/
    └── videos/           ← pon aquí los .mp4 de la galería (opcional)
```

Despliega **esta carpeta `dist/`** tal cual. Aunque solo lleves `index.html`
funciona, pero para que los vídeos de la galería tengan dónde estar, sube la
carpeta completa.

> Nota: las librerías (three, gsap, swiper, motion, ogl) y las fuentes se cargan
> por CDN, así que el sitio necesita conexión a internet al abrirse. No incluye
> assets locales salvo los vídeos opcionales.

---

## Opción A — Cloudflare Pages (recomendado, gratis y sin límite de ancho de banda)

1. Sube el contenido de `dist/` a un repositorio en GitHub (raíz del repo, no una subcarpeta).
   ```bash
   cd dist
   git init
   git add .
   git commit -m "Charohergar site"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
   git push -u origin main
   ```
2. En [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** → **Create** → **Pages** → **Connect to Git**.
3. Elige el repo. En *Build settings* no pongas comando de build (no hay build).
   - **Build command:** *(vacío)*
   - **Build output directory:** `/`  (o `dist` si subiste solo esa carpeta al repo)
4. **Deploy**. Te da una URL tipo `https://tu-proyecto.pages.dev`. En minutos puedes añadir un dominio propio en *Custom domains*.

El archivo `_headers` se aplica automáticamente: caché larga para assets,
`must-revalidate` para el HTML, y cabeceras de seguridad básicas.

## Opción B — GitHub Pages (más simple)

1. Sube `dist/` a un repo (raíz).
2. **Settings → Pages → Source:** *Deploy from a branch* → `main` / `/ (root)` → **Save**.
3. En ~1 minuto: `https://TU_USUARIO.github.io/TU_REPO/`.

GitHub Pages ignora `_headers` (es de Cloudflare) — solo afecta a las cabeceras, el sitio funciona igual.

## Opción C — Netlify (el deploy más rápido, drag & drop)

1. Ve a [app.netlify.com/drop](https://app.netlify.com/drop).
2. Arrastra la carpeta `dist/` entera.
3. Te da al instante una URL; puedes cambiar el nombre del sitio o apuntar un dominio en *Domain management*.

---

## Antes de publicar (checklist)

- [ ] **Vídeos (opcional):** añade `jesus-marti-acustica.mp4` y `aftermovie-surco.mp4` en `assets/videos/`. comprimidos (~2–5 MB c/u). Si no están, la galería usa pósters SVG sin romperse.
- [ ] **URL real:** reemplaza `https://EJEMPLO.pages.dev` en `robots.txt` y `sitemap.xml` por tu dominio.
- [ ] **Open Graph:** en el `<head>` de `index.html`, añade `og:url` y `og:image` (una imagen ≥1200×630 para que la preview en redes se vea bien). Ahora solo hay `og:title/description/type`.
- [ ] **Favicon (recomendado):** añade `<link rel="icon" href="favicon.svg">` en el `<head>` y sube un `favicon.svg`. Sin él, la consola del navegador muestra un 404 inofensivo.
- [ ] **Prueba carga:** comprueba que abre en <3 s (ej. [PageSpeed Insights](https://pagespeed.web.dev/)).

## Comprobar que funciona en local

```bash
# Cualquiera de estas, desde dentro de dist/
npx serve .
python -m http.server 8000
```

Luego abre `http://localhost:8000` (o la puerto que indique). Ábrelo con un
servidor, **no** con `file://`: los módulos ES (`import ... from 'ogl'`) y el
importmap no funcionan abriendo el archivo directamente.
