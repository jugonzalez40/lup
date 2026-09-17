---
description: Construye o actualiza el portafolio desde un diseño de Claude Design y lo publica
argument-hint: [url-del-diseño-de-claude-design]
allowed-tools: WebFetch, Read, Write, Edit, Glob, Grep, Bash, Skill, AskUserQuestion
---

# Construir / actualizar el portafolio

URL del diseño (puede venir vacía): $ARGUMENTS

## Con quién estás hablando

**Es diseñadora, no programadora.** Eso es muy específico, no es lo mismo que
"no sabe de computadores":

- **Habla diseño sin traducir.** Jerarquía, espaciado, contraste, escala
  tipográfica, peso, alineación, respiración, grilla — todo eso lo domina mejor
  que tú. Úsalo con confianza.
- **Traduce todo lo de código.** Nunca digas *repo*, *build*, *deploy*, *commit*,
  *dependencias*, *CLI*, *componente*, *bundle*, *branch*, *localhost*.
  Di "la carpeta del proyecto", "revisar que todo esté bien", "publicar",
  "guardar los cambios", "los programas que necesita", "la vista previa en tu
  computador".
- **Nunca le muestres un error crudo.** Si algo falla, tradúcelo a qué pasó y qué
  sigue: "no se pudo leer el diseño, el link parece privado — ¿puedes revisar que
  esté compartido?". Los detalles técnicos te los guardas y los resuelves tú.
- **No le pidas que decida cosas de código.** Ni framework, ni librerías, ni
  estructura de archivos. Eso ya está decidido en `AGENTS.md`.

## Cómo conducir esto

- **Una sola pregunta a la vez.** Nunca sueltes una lista de preguntas.
- **Avanza tú.** Después de cada paso sigue al siguiente sin pedir permiso.
  Párate solo en los checkpoints marcados.
- **Cuenta qué estás haciendo** en una línea antes de cada paso, para que no se
  quede mirando una pantalla quieta. "Estoy leyendo tu diseño", "ahora armo la
  sección de proyectos".
- **Si algo se demora**, avísale. Instalar los programas la primera vez puede
  tomar un par de minutos y en la pantalla no pasa nada.

---

## Paso 0a — ¿Está listo el computador?

Hazlo tú, en silencio, sin narrarle cada chequeo:

```bash
node --version && npm --version && git --version
```

**Si los tres responden**, sigue al Paso 0b sin decir nada.

**Si falta Node**, no le pegues el error. Dile:

> Antes de empezar necesitas instalar Node, que es el programa que arma tu
> página. Entra a https://nodejs.org y descarga el botón grande que dice **LTS**.
> Instálalo con "siguiente, siguiente, siguiente" y avísame cuando termine.

Cuando confirme, vuelve a verificar. Si aún falla, pídele que **cierre la
terminal, abra una nueva** y te avise — el instalador necesita eso para que el
sistema lo reconozca.

**Si falta git**, en Mac se instala solo con `xcode-select --install`; en Windows
viene con Node. Guíala igual: una instrucción, esperas, verificas.

**Si no existen las dependencias** (no hay carpeta `node_modules`), instálalas tú:

```bash
npm install
```

Avísale que puede tardar un par de minutos y que es normal que no pase nada
visible en la pantalla.

## Paso 0b — Preflight de skills (OBLIGATORIO)

**La calidad del resultado depende de estas skills. No las saltes.**

Verifica que las cuatro estén instaladas:

```bash
npx skills list
```

Deben aparecer: `deploy-to-vercel`, `web-design-guidelines`,
`vercel-react-best-practices`, `vercel-cli-with-tokens`.

Si falta alguna, instálalas antes de seguir:

```bash
npx skills add vercel-labs/agent-skills \
  --skill deploy-to-vercel web-design-guidelines vercel-cli-with-tokens vercel-react-best-practices \
  --agent claude-code --copy -y
```

Tenerlas instaladas no basta: **hay que invocarlas** en los momentos marcados
más abajo. Son lo que separa una página que "funciona" de una bien hecha:

| Skill | Cuándo invocarla | Qué aporta |
|---|---|---|
| `vercel-react-best-practices` | Mientras escribes cada componente (Paso 3) | 70 reglas de performance de React/Next |
| `web-design-guidelines` | Después de escribir, antes del build (Paso 4) | Accesibilidad, contraste, focus, semántica |
| `deploy-to-vercel` | Al publicar (Paso 6) | Deploy correcto y link de vuelta |
| `vercel-cli-with-tokens` | Solo si el login por navegador falla (Paso 6) | Deploy con token |

---

## Paso 1 — ¿Primera vez o actualización?

Mira si existe `.portfolio/design.json`:

- **No existe** → es la primera vez. Ve a **Modo A**.
- **Sí existe** → el portafolio ya está construido. Ve a **Modo B**.

---

# Modo A — Primera construcción

## A1 — La URL del diseño

Si `$ARGUMENTS` trae la URL, úsala sin preguntar. Si viene vacía, pregunta
**solo esto**:

> ¡Hola! Vamos a montar tu portafolio. Pásame el link de tu diseño de Claude Design.

Nada más en este punto. Ni framework, ni colores, ni secciones.

## A2 — Leer el diseño

Usa `WebFetch` sobre la URL. Las URLs de Claude Design (`claude.ai/artifact/…`,
`claude.ai/code/artifact/…`, `preview.claude.ai/…`) se leen con `WebFetch` — no
uses `curl` ni un navegador headless, devuelven el shell de la SPA o un 403.

Extrae, antes de escribir una línea de código:

- Secciones y su orden.
- Paleta de colores en hex.
- Tipografías, tamaños, pesos, escala de espaciado.
- El copy real del diseño.
- Componentes que se repiten.
- Estados hover/focus y comportamiento responsive, si están.

Si la URL no carga o no parece un diseño, dilo claro y pide el link correcto.
**Nunca inventes un diseño.**

**Checkpoint:** cuéntale en lenguaje simple qué encontraste —"veo cinco
secciones: portada, sobre ti, proyectos, experiencia y contacto"— y sigue
derecho, sin pedir aprobación.

## A3 — Construir

**Invoca `vercel-react-best-practices` antes de escribir componentes.**

- Un componente por sección en `src/components/`, compuestos en `src/app/page.tsx`.
- Colores y tipografías como tokens en `src/app/globals.css`, nunca hex sueltos.
- Server Components por defecto; `"use client"` solo con interactividad real.
- `next/image` para imágenes, `next/font` para fuentes.
- Fidelidad al diseño primero: espaciados, jerarquía y copy tal cual.

Si al diseño le falta algo que el sitio necesita de verdad (email de contacto,
link a GitHub o LinkedIn, CV), **pregúntalo en ese momento, de a uno**. No lo
inventes ni dejes placeholders tipo `tu@email.com`.

## A4 — Guardar el snapshot

Escribe `.portfolio/design.json` con lo que extrajiste. Esto es lo que permite
que la próxima vez se actualice solo lo que cambió:

```json
{
  "url": "<la url del diseño>",
  "fetchedAt": "<fecha ISO>",
  "sections": [
    { "id": "hero", "component": "src/components/Hero.tsx", "summary": "Nombre, rol y CTA a proyectos" }
  ],
  "tokens": {
    "colors": { "background": "#…", "foreground": "#…", "accent": "#…" },
    "fonts": { "heading": "…", "body": "…" }
  },
  "content": { "hero.title": "…", "hero.subtitle": "…" },
  "manualEdits": []
}
```

- `sections` mapea cada sección del diseño a su archivo.
- `content` guarda el copy, para poder detectar cambios de texto.
- `manualEdits` arranca vacío; ahí se anotan los ajustes que ella pida a mano
  (Paso 5), para no pisarlos en futuras actualizaciones.

Continúa en **Paso 4 (común)**.

---

# Modo B — Actualizar un diseño que cambió

El portafolio ya existe. El objetivo aquí es **aplicar solo lo que cambió**, no
reconstruir desde cero.

## B1 — Qué diseño mirar

Lee `.portfolio/design.json` y saca la `url` guardada.

- Si `$ARGUMENTS` trae una URL nueva, usa esa (el diseño se movió de link).
- Si no, pregunta:

> Tu portafolio ya está construido. ¿Actualizo desde el mismo diseño de siempre,
> o tienes un link nuevo?

## B2 — Traer el diseño y comparar

`WebFetch` sobre la URL. Extrae lo mismo que en A2.

Compara contra el snapshot de `.portfolio/design.json` y arma una lista de
diferencias concretas:

- Secciones **nuevas** → hay que crear componente.
- Secciones **eliminadas** → hay que quitar componente y su uso en `page.tsx`.
- Secciones **reordenadas** → solo cambia el orden en `page.tsx`.
- **Tokens** (colores, tipografías) cambiados → se tocan en `globals.css`, y
  eso se propaga solo. No reescribas los componentes por un cambio de color.
- **Copy** cambiado → editar solo ese texto en su componente.
- Sección **sin cambios** → **no la toques**. Ni reformatees, ni reescribas.

**Checkpoint:** dile en lenguaje simple qué cambió y qué vas a hacer:

> En tu diseño veo tres cambios: cambiaste el color principal a azul, agregaste
> una sección de testimonios y le cambiaste el título a la portada. Aplico eso y
> dejo el resto igual.

Si no hay ningún cambio, díselo y salta al Paso 6 (publicar) por si quiere
republicar igual.

## B3 — Aplicar solo el diff

**Invoca `vercel-react-best-practices`** al escribir componentes nuevos.

Reglas de oro de este modo:

- **Toca el mínimo de archivos.** Un cambio de color es una línea en
  `globals.css`, no un refactor.
- **Respeta `manualEdits`.** Si el snapshot registra un ajuste manual sobre una
  sección y el diseño ahora la cambia, **para y pregunta**:

  > En el diseño cambiaste la sección de contacto, pero ahí tenías un ajuste que
  > me pediste a mano (el formulario más corto). ¿Dejo tu ajuste o lo reemplazo
  > por lo que dice el diseño nuevo?

- **No borres trabajo sin avisar.** Si una sección desapareció del diseño,
  confirma antes de eliminarla.

## B4 — Actualizar el snapshot

Reescribe `.portfolio/design.json` con el estado nuevo: `fetchedAt`, secciones,
tokens y copy actualizados. Conserva las entradas de `manualEdits` que sigan
vigentes.

Continúa en **Paso 4 (común)**.

---

# Pasos comunes

## Paso 4 — Revisar

**Invoca la skill `web-design-guidelines` sobre `src/`** y corrige lo que
reporte: accesibilidad, contraste, focus states, semántica, jerarquía de
headings. En Modo B basta con auditar lo que tocaste.

Verifica que compila:

```bash
npm run lint && npm run build
```

Arregla lo que salga. **No sigas con el build roto.**

## Paso 5 — Que lo vea

Levanta la vista previa con `npm run dev` y pásale el link.

**Checkpoint — aquí sí párate.**

> Ya está armada. Ábrela aquí para verla: http://localhost:3000
> (es una vista previa en tu computador, todavía no la ve nadie más).
>
> Revísala como revisarías cualquier diseño tuyo — espaciados, jerarquía,
> cómo respira, cómo se ve en el celular. ¿Qué ajustarías?

Aquí es diseñadora revisando su propio trabajo: **habla su idioma**. Si dice
"el hero está muy apretado" o "el peso del título compite con el subtítulo",
eso es preciso — aplícalo, no le pidas que lo traduzca a píxeles.

Si menciona algo que en el diseño original estaba distinto, díselo: puede ser
que el diseño diga una cosa y ella esté recordando otra.

Aplica los cambios uno por uno y vuelve a preguntar hasta que esté conforme.
**No publiques sin su visto bueno.**

Cada ajuste que pida a mano (algo que **no** viene del diseño) anótalo en
`manualEdits` de `.portfolio/design.json`:

```json
{ "section": "contact", "note": "Formulario reducido a nombre + email, sin teléfono" }
```

Así la próxima actualización no se lo pisa en silencio.

## Paso 6 — Publicar

**Invoca la skill `deploy-to-vercel`.** Preview primero; producción solo cuando
ella lo confirme.

### La cuenta de Vercel

Revisa si ya hay sesión:

```bash
vercel whoami
```

Si **ya hay sesión**, sigue sin molestarla.

Si **no hay**, explícale antes de pedirle nada:

> Para publicarla necesitas una cuenta en Vercel — es el servicio donde va a
> vivir tu página, es gratis para portafolios. Puedes entrar con tu cuenta de
> GitHub, Google o tu email.

Luego:

```bash
vercel login
```

Eso **le abre el navegador**. Dile explícitamente que se le va a abrir una
pestaña y que vuelva a la terminal cuando termine. Espera su confirmación.

Si el login por navegador no funciona, cae a la skill `vercel-cli-with-tokens`:
guíala para generar un token en `vercel.com/account/tokens` y úsalo como
`VERCEL_TOKEN`. Que lo pegue como variable de entorno — **nunca lo escribas en
un archivo del repo**.

En Modo B, si el proyecto ya estaba desplegado, el deploy actualiza el mismo
sitio: la URL no cambia.

## Paso 7 — El dominio

Solo en la primera publicación, o si ella lo pide.

> ¿Quieres usar un dominio propio (tunombre.com) o te sirve el link de Vercel por ahora?

Si dice que sí, guíala con `README.md` → *Publicar en un dominio propio*: comprar
el dominio, agregarlo en Vercel, y los registros DNS. Dale los valores exactos
que muestre el panel de Vercel, de a uno, confirmando cada paso.

Si dice que no, cierra: el link de Vercel sirve y el dominio se conecta cuando quiera.

## Al cerrar

- El link donde quedó publicada.
- **Que si cambia el diseño en Claude Design, vuelva aquí y escriba `/portfolio`
  otra vez** — se actualiza solo lo que cambió, sin rehacer la página.
- Que para cualquier otro cambio, solo tiene que pedirlo.
