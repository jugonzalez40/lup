---
description: Construye el portafolio paso a paso desde un diseño de Claude Design y lo publica
argument-hint: [url-del-diseño-de-claude-design]
allowed-tools: WebFetch, Read, Write, Edit, Glob, Grep, Bash, Skill, AskUserQuestion
---

# Construir el portafolio, paso a paso

URL del diseño (puede venir vacía): $ARGUMENTS

## Cómo conducir esto

La persona del otro lado **no es técnica**. Condúcela tú.

- **Una sola pregunta a la vez.** Nunca sueltes una lista de preguntas.
- **Pregunta solo lo que no puedas averiguar tú.** El stack, la estructura y las
  convenciones ya están decididos en `AGENTS.md`: no los consultes.
- **Sin jerga.** Di "la página" y no "el bundle"; "se ve bien en celular" y no
  "el breakpoint responsive".
- **Avanza tú.** Después de cada paso, sigue al siguiente sin pedir permiso.
  Solo párate en los checkpoints marcados abajo.
- **Cuenta qué estás haciendo** en una línea antes de cada paso, para que no
  se quede mirando una pantalla quieta.

## Paso 1 — La URL del diseño

Si `$ARGUMENTS` trae la URL, úsala y no preguntes nada.

Si viene vacía, pregunta **solo esto**:

> ¡Hola! Vamos a montar tu portafolio. Pásame el link de tu diseño de Claude Design.

No preguntes nada más en este punto. Ni framework, ni colores, ni secciones.

## Paso 2 — Leer el diseño

Usa `WebFetch` sobre la URL. Las URLs de Claude Design (`claude.ai/artifact/…`,
`claude.ai/code/artifact/…`, `preview.claude.ai/…`) se leen con `WebFetch` — no
uses `curl` ni un navegador headless, devuelven el shell de la SPA o un 403.

Extrae antes de escribir una sola línea de código:

- Secciones y su orden (hero, sobre mí, proyectos, contacto…).
- Paleta de colores en hex.
- Tipografías, tamaños, pesos, escala de espaciado.
- El copy real del diseño.
- Componentes que se repiten (tarjeta de proyecto, ítem de timeline, chip…).
- Estados hover/focus y comportamiento responsive, si el diseño los muestra.

Si la URL no carga o no parece un diseño, dilo claro y pide el link correcto.
**Nunca inventes un diseño.**

**Checkpoint:** cuéntale en lenguaje simple qué encontraste —"veo cinco
secciones: portada, sobre ti, proyectos, experiencia y contacto"— y sigue
derecho. No pidas aprobación aquí.

## Paso 3 — Construir

Stack ya definido en `AGENTS.md`: Next.js (App Router) + TypeScript + Tailwind.

- Un componente por sección en `src/components/`, compuestos en `src/app/page.tsx`.
- Colores y tipografías como tokens en `src/app/globals.css`, nunca hex sueltos.
- Server Components por defecto; `"use client"` solo con interactividad real.
- `next/image` para imágenes, `next/font` para fuentes.
- Fidelidad al diseño primero: respeta espaciados, jerarquía y copy tal cual.

Invoca la skill `vercel-react-best-practices` mientras escribes los componentes.

Si al diseño le falta algo que el sitio necesita de verdad (el email de contacto,
el link a su GitHub o LinkedIn, el CV), **pregúntalo en ese momento, de a uno**,
y sigue. No lo inventes ni pongas placeholders tipo `tu@email.com`.

## Paso 4 — Revisar

Invoca la skill `web-design-guidelines` sobre `src/` y corrige lo que reporte
(accesibilidad, contraste, focus states, semántica, jerarquía de headings).

Verifica que compila:

```bash
npm run lint && npm run build
```

Arregla lo que salga. **No sigas con el build roto.**

## Paso 5 — Que lo vea

Levanta `npm run dev` y dale el link local (`http://localhost:3000`).

**Checkpoint — aquí sí párate.** Pregúntale:

> Ya está armada, ábrela en http://localhost:3000. ¿Qué te gustaría ajustar antes de publicarla?

Aplica los cambios que pida, uno por uno, y vuelve a preguntar hasta que diga
que está conforme. **No despliegues sin su visto bueno.**

## Paso 6 — Publicar

Invoca la skill `deploy-to-vercel`. Preview primero; producción solo cuando
ella lo confirme.

### La cuenta de Vercel

Primero revisa si ya hay sesión:

```bash
vercel whoami
```

Si **ya hay sesión**, sigue sin molestarla.

Si **no hay sesión**, explícale en una línea qué es Vercel antes de pedirle nada:

> Para publicarla necesitas una cuenta en Vercel — es el servicio donde va a vivir
> tu página, es gratis para portafolios. Puedes entrar con tu cuenta de GitHub,
> Google o tu email.

Luego corre:

```bash
vercel login
```

Eso **le abre el navegador** para que confirme. Dile explícitamente que se le va
a abrir una pestaña y que vuelva a la terminal cuando termine. Espera a que
confirme que ya lo hizo; no sigas mientras tanto.

Si el login por navegador no funciona (terminal sin sesión interactiva), cae a
la skill `vercel-cli-with-tokens`: guíala para generar un token en
`vercel.com/account/tokens` y úsalo como `VERCEL_TOKEN`. Que lo pegue como
variable de entorno — **nunca lo escribas en un archivo del repo**.

### Después del deploy

Entrégale el link y dile que ya está en internet.

## Paso 7 — El dominio

Pregunta:

> ¿Quieres usar un dominio propio (tunombre.com) o te sirve el link de Vercel por ahora?

Si dice que sí, guíala con los pasos de `README.md` → *Publicar en un dominio
propio*: comprar el dominio, agregarlo en Vercel, y los registros DNS. Dale los
valores exactos que muestre el panel de Vercel, uno a uno, y confirma que cada
paso le funcionó antes de pasar al siguiente.

Si dice que no, cierra ahí: el link de Vercel ya sirve y el dominio se puede
conectar cuando quiera.

## Al cerrar

Dile en lenguaje simple:

- El link donde quedó publicada.
- Que para cambiar algo solo tiene que abrir Claude Code en la carpeta y pedirlo.
- Que cada cambio se vuelve a publicar solo con pedírtelo.
