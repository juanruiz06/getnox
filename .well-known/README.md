# `.well-known/` — universal links de iOS

`apple-app-site-association` (AASA) es lo que hace que iOS abra la app en vez del
navegador cuando alguien toca un enlace `https://getnox.es/p/…`, `/e/…`, `/v/…` o
`/u/…`. Hoy está **preparado pero no activo**: el Team ID ya es el real (`NW3H8PF3XV`, del
proyecto iOS de la app); falta publicar un build de la app con `associatedDomains`
(spec 042 §8, build 20; PR preparada en juanruiz06/nox).

## Qué falta antes de activarlo

1. ~~Sustituir `TEAMID`~~ Hecho: `NW3H8PF3XV.com.juanruiz.nox` (si el Team ID cambiara,
   está en App Store Connect → Membership details).
2. En el repo de la app (`juanruiz06/nox`), añadir a `app.json`:
   `expo.ios.associatedDomains: ["applinks:getnox.es"]`. Cambia el fingerprint, así que
   **exige build nuevo** (no llega por OTA).
3. Comprobar que GitHub Pages lo sirve:

   ```bash
   curl -I https://getnox.es/.well-known/apple-app-site-association
   ```

   Tiene que responder **200**. GitHub Pages lo sirve sin extensión como
   `application/octet-stream`; desde iOS 9.3 Apple **acepta** ese content-type (ya no
   exige `application/json` ni firma). Si algún día dejara de valer, la salida es mover
   el sitio a un hosting donde se pueda fijar la cabecera (Cloudflare Pages, Netlify).

4. Verificar el fichero con el validador de Apple
   (<https://app-site-association.cdn-apple.com/a/v1/getnox.es>) DESPUÉS de publicar:
   la CDN de Apple lo cachea, puede tardar en refrescarse.

## Notas

- El fichero **no lleva extensión** a propósito: iOS lo pide exactamente en
  `/.well-known/apple-app-site-association`.
- La raíz del repo tiene `.nojekyll`, que es lo que hace que GitHub Pages publique las
  carpetas que empiezan por punto (sin él, `.well-known/` no se serviría).
- Mientras el AASA no esté activo, los enlaces siguen funcionando: abren la página
  puente (`/p/`, `/e/`, `/v/`, `/u/`, `/a/`) con el botón "Abrir en NOX".
