# FUNDALMA — Guía de publicación (GitHub + Netlify + nic.ar)

Sitio estático nuevo para fundalma.org.ar, con diseño basado en colegiovelezsarsfield.net.

## Estructura

```
fundalma-web/
├── index.html                → Página principal (one-page)
├── novedades.html            → Novedades (editable)
├── nivel-inicial.html        → Inicial: info + fotos
├── nivel-primario.html       → Primario: info + fotos
├── nivel-secundario.html     → Secundario: info + recursos + fotos
├── casa-de-maria.html        → La Casa de María
├── proyectos-solidarios.html → Proyectos de la Fundación
├── css/styles.css            → Estilos (colores en :root)
├── img/                      → Poner acá tus fotos propias
└── netlify.toml              → Config de Netlify + redirecciones
```

## ANTES de publicar — 3 reemplazos obligatorios

1. **Formulario de inscripción**: buscar `https://forms.gle/REEMPLAZAR-INSCRIPCION` en todos los archivos y reemplazar por el link real del Google Form (aparece en index.html, novedades.html y en el header/footer de todas las páginas).
2. **Formulario de consulta**: reemplazar `https://forms.gle/REEMPLAZAR-CONSULTA` en index.html.
3. **WhatsApp**: verificar el número en el botón flotante (`wa.me/5491138775820`, tomado del 11-3877-5820 de administración). Cambiarlo si corresponde otro.

> Truco: en VS Code, Ctrl+Shift+H permite buscar y reemplazar en toda la carpeta.

## IMPORTANTE — Imágenes

Las fotos actuales apuntan a los servidores de Wix (`static.wixstatic.com`). **Funcionan mientras el sitio Wix siga activo.** Antes de dar de baja Wix:

1. Descargá las fotos que quieras conservar desde el administrador de Wix.
2. Guardalas en la carpeta `img/`.
3. Reemplazá en los HTML las URLs `https://static.wixstatic.com/...` por `img/nombre-foto.jpg`.

Lo mismo aplica a los PDF alojados en Wix (Muestra Anual 2024, taller de escritura): descargarlos, ponerlos en una carpeta `docs/` y actualizar los links.

## Paso 1 — Subir a GitHub

1. Crear cuenta en https://github.com (si no tenés).
2. Crear repositorio nuevo: **New repository** → nombre `fundalma-web` → Public o Private → Create.
3. Subir los archivos. Opción simple sin instalar nada: en el repo, **Add file → Upload files**, arrastrar TODO el contenido de esta carpeta (incluida la subcarpeta css) y **Commit changes**.
   - Opción con Git: `git init`, `git add .`, `git commit -m "Sitio inicial"`, `git remote add origin <url>`, `git push -u origin main`.

## Paso 2 — Conectar Netlify

1. Crear cuenta en https://app.netlify.com (podés entrar con la cuenta de GitHub).
2. **Add new site → Import an existing project → GitHub** → autorizar → elegir `fundalma-web`.
3. Build settings: dejar todo vacío (no hay build). Publish directory: `.` (raíz). **Deploy site**.
4. En 1 minuto vas a tener el sitio en una URL tipo `https://algo-aleatorio.netlify.app`. Revisalo completo ahí antes de tocar el dominio.
5. (Opcional) En **Site configuration → Site details → Change site name** poné `fundalma` para tener `fundalma.netlify.app`.

Desde ahora, cada cambio que subas a GitHub se publica solo en ~1 minuto.

## Paso 3 — Apuntar fundalma.org.ar a Netlify

1. En Netlify: **Domain management → Add a domain** → escribir `fundalma.org.ar` → verificar. Agregar también `www.fundalma.org.ar`.
2. Netlify te va a mostrar los DNS a configurar. En el panel donde administran los DNS del dominio (nic.ar delega a un servidor DNS; puede ser el de Wix, un hosting, o Cloudflare):
   - Registro **A** para `fundalma.org.ar` → `75.2.60.5` (IP del load balancer de Netlify; confirmar la que muestre el panel).
   - Registro **CNAME** para `www` → `<tu-sitio>.netlify.app`.
   - Alternativa recomendada: usar **Netlify DNS** (Netlify te da 4 nameservers tipo `dns1.p05.nsone.net`) y cargarlos en https://nic.ar → Panel → Delegaciones del dominio.
3. **Ojo con el email**: `@fundalma.org.ar` usa Google Workspace. Si cambiás los nameservers, hay que volver a cargar los registros **MX de Google** en el nuevo DNS antes de migrar, o el correo deja de llegar. Copiá TODOS los registros DNS actuales (MX, TXT/SPF, etc.) antes de tocar nada.
4. La propagación DNS tarda de 1 a 48 hs. Mientras tanto el sitio Wix sigue respondiendo.
5. En Netlify: **Domain management → HTTPS → Verify DNS → Provision certificate** (certificado SSL gratis, automático).

## Paso 4 — Baja de Wix (recién cuando todo funcione)

1. Verificar que el sitio nuevo carga en fundalma.org.ar con candado (HTTPS).
2. Verificar que el correo sigue funcionando.
3. Descargar del Wix todas las fotos/PDF pendientes (ver sección Imágenes).
4. Cancelar el plan de Wix.

## Cómo actualizar el sitio después

- **Novedades**: editar `novedades.html` — hay un comentario `<!-- PLANTILLA -->` que indica qué bloque copiar y pegar para agregar una novedad.
- **Aranceles**: agregar la novedad con link al PDF o imagen que subas a la carpeta `docs/` o `img/`.
- **Colores**: todos están definidos al inicio de `css/styles.css` en `:root`.
- Cualquier archivo editado y subido a GitHub (o arrastrado de nuevo en "Upload files") se publica automáticamente.
