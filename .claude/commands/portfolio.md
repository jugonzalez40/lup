---
description: Construye el portafolio a partir de un diseño de Claude Design y lo despliega en Vercel
argument-hint: <url-del-diseño-de-claude-design>
allowed-tools: WebFetch, Read, Write, Edit, Glob, Grep, Bash, Skill
---

# Construir portafolio desde un diseño de Claude Design

URL del diseño: $ARGUMENTS

## Paso 0 — Conseguir la URL

Si `$ARGUMENTS` está vacío, pregunta **únicamente** esto y no avances hasta tenerla:

> Pásame la URL del diseño de Claude Design.

No preguntes nada más: ni framework, ni colores, ni secciones, ni nombre de dominio.
Todo lo demás se deduce del diseño o ya está decidido en `CLAUDE.md`.

## Paso 1 — Leer el diseño

Usa `WebFetch` sobre la URL para extraer el diseño. Las URLs de Claude Design
(`claude.ai/artifact/…`, `claude.ai/code/artifact/…`, `preview.claude.ai/…`) son
accesibles con `WebFetch` — no uses `curl` ni un navegador headless, devuelven el
shell de la SPA o un 403, no el contenido.

Extrae y anota antes de escribir código:

- Secciones y su orden (hero, sobre mí, proyectos, experiencia, contacto…).
- Paleta de colores exacta, en hex.
- Tipografías, tamaños, pesos y escala de espaciado.
- Textos y copy reales del diseño.
- Componentes repetidos (tarjeta de proyecto, ítem de timeline, chip de skill…).
- Comportamiento responsive y estados hover/focus si el diseño los muestra.

Si la URL no carga o el contenido no parece un diseño, dilo y pide la URL correcta.
No inventes un diseño.

## Paso 2 — Implementar en React

Stack ya definido en `CLAUDE.md`: Next.js (App Router) + TypeScript + Tailwind.

- Un componente por sección en `src/components/`, compuestos en `src/app/page.tsx`.
- Los colores y tipografías del diseño van como tokens en `src/app/globals.css`,
  nunca hardcodeados por toda la app.
- Server Components por defecto; `"use client"` solo donde haya interactividad real.
- Imágenes con `next/image`, fuentes con `next/font`.
- Fidelidad al diseño primero: respeta espaciados, jerarquía y copy tal cual.

Invoca la skill `vercel-react-best-practices` mientras escribes los componentes.

## Paso 3 — Revisar la UI

Invoca la skill `web-design-guidelines` sobre `src/` y corrige lo que reporte
(accesibilidad, focus states, contraste, semántica, jerarquía de headings).

Luego verifica que compila y que no hay errores de lint:

```bash
npm run lint && npm run build
```

Arregla lo que salga antes de seguir. No despliegues con el build roto.

## Paso 4 — Desplegar a Vercel

Invoca la skill `deploy-to-vercel`. Despliega como **preview** y entrega el link.
Solo despliega a producción si te lo piden explícitamente.

Si no hay sesión interactiva de Vercel disponible, usa la skill
`vercel-cli-with-tokens` con `VERCEL_TOKEN`.

## Paso 5 — Reportar

Cierra con:

- El link del deploy.
- Las secciones que se construyeron.
- Lo que `web-design-guidelines` reportó y cómo quedó.
- Los pasos para conectar el dominio propio (ver `README.md`), si aún no está conectado.
