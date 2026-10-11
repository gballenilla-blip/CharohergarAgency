# assets/videos/

Coloca aquí los dos vídeos que referencia la galería (son opcionales):

- `jesus-marti-acustica.mp4`
- `aftermovie-surco.mp4`

Mientras no existan, la web muestra un póster SVG generado en su lugar
(el `<video>` se retira silenciosamente al fallar la carga) — no se ve nada roto.

Consejos de rendimiento (el sitio promete cargar en <3 s):

- Comprime cada vídeo: 720p, ~2–5 MB por archivo.
- Formato recomendado: H.264 (`*.mp4`) con perfil `faststart` para poder
  hacer streaming mientras se descarga:
  `ffmpeg -i entrada.mov -c:v libx264 -crf 26 -preset slow -movflags +faststart -an salida.mp4`
- Alternativa sin gestionar archivos: sube a YouTube/Vimeo en modo no listado
  y sustituye el `src` por la URL embed.
