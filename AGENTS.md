<!-- BEGIN:nextjs-agent-rules -->

# This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` (resolved from this file's directory; in monorepos the `next` package may not be visible from the repo root) before writing any code. Heed deprecation notices.

This block is written and re-added by `next dev` — verify at `node_modules/next/dist/server/lib/generate-agent-files.js`. Removing it from a diff only re-creates the uncommitted change; committing it with your work keeps the tree clean.

<!-- END:nextjs-agent-rules -->

# lup — portafolio web

Portafolio personal construido a partir de un diseño hecho en Claude Design.
El flujo completo está en `README.md`.

## Stack

| Pieza | Elección |
|---|---|
| Framework | Next.js 16 (App Router) |
| Lenguaje | TypeScript |
| UI | React 19 |
| Estilos | Tailwind CSS v4 |
| Hosting | Vercel |

Estas decisiones ya están tomadas. No las re-preguntes ni propongas cambiarlas
salvo que alguien lo pida explícitamente.

Next.js 16 y Tailwind v4 traen cambios de API respecto a versiones anteriores:
consulta `node_modules/next/dist/docs/` antes de escribir código, como indica el
bloque de arriba.

## Estructura

```
src/app/             rutas y layout (App Router)
src/app/page.tsx     home: compone las secciones en orden
src/app/globals.css  tokens de color y tipografía del diseño
src/components/      un componente por sección del diseño
```

## Convenciones

- **Un componente por sección** del diseño, nombrado como la sección
  (`Hero.tsx`, `Projects.tsx`, `Contact.tsx`).
- **Server Components por defecto.** `"use client"` solo donde haya
  interactividad real (formularios, animaciones con estado, listeners).
- **Colores y tipografías como tokens** en `globals.css`. Nunca hex sueltos
  repartidos por los componentes.
- **`next/image` para imágenes** y **`next/font` para fuentes**, siempre.
- **Fidelidad al diseño manda.** Respeta espaciados, jerarquía y copy tal como
  vienen del diseño; no "mejores" el diseño por tu cuenta.

## Antes de dar algo por terminado

```bash
npm run lint && npm run build
```

Ambos tienen que pasar. No se despliega con el build roto.

## Skills — invócalas, no basta con tenerlas

Viven en `.claude/skills/`, vienen de `vercel-labs/agent-skills`. La calidad del
resultado depende de que se **invoquen** en el momento correcto:

| Skill | Invocar cuando | Aporta |
|---|---|---|
| `vercel-react-best-practices` | Escribes o refactorizas componentes | 70 reglas de performance React/Next |
| `web-design-guidelines` | Terminaste de escribir, antes del build | Accesibilidad, contraste, focus, semántica |
| `deploy-to-vercel` | Vas a publicar | Deploy correcto y link |
| `vercel-cli-with-tokens` | El login por navegador falló | Deploy con token |

Si falta alguna:

```bash
npx skills add vercel-labs/agent-skills \
  --skill deploy-to-vercel web-design-guidelines vercel-cli-with-tokens vercel-react-best-practices \
  --agent claude-code --copy -y
```

Se actualizan con `npx skills update`.

## El diseño es incremental

`.portfolio/design.json` es el snapshot de qué se construyó y desde qué diseño:
URL, secciones → archivo, tokens, copy, y los ajustes manuales pedidos fuera del
diseño (`manualEdits`).

Cuando el diseño cambie, **no reconstruyas**: trae el diseño, compáralo contra el
snapshot y aplica solo la diferencia.

- Cambio de color → una línea en `globals.css`, no un refactor.
- Sección sin cambios → no la abras, ni para reformatear.
- El diseño cambia una sección con un `manualEdit` registrado → **para y
  pregunta** cuál gana. Nunca pises un ajuste manual en silencio.
- Sección eliminada del diseño → confirma antes de borrarla.

Después de aplicar cambios, actualiza el snapshot.

## Comando principal

`/portfolio [url-del-diseño]` — construye el portafolio desde un diseño de Claude
Design y lo publica; en corridas posteriores actualiza solo lo que cambió.
Definido en `.claude/commands/portfolio.md`.
