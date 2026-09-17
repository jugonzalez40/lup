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

## Skills instaladas

Viven en `.claude/skills/`, vienen de `vercel-labs/agent-skills`:

- `deploy-to-vercel` — desplegar y devolver el link.
- `web-design-guidelines` — auditar la UI (accesibilidad, contraste, semántica).
- `vercel-react-best-practices` — patrones de performance de React/Next.
- `vercel-cli-with-tokens` — deploy vía CLI con token, sin login interactivo.

Se actualizan con `npx skills update`.

## Comando principal

`/portfolio <url-del-diseño>` — construye el portafolio desde un diseño de Claude
Design y lo despliega. Definido en `.claude/commands/portfolio.md`.
