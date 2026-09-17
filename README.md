# lup — portafolio web

Portafolio personal. El diseño se hace en **Claude Design**, el código lo genera
**Claude Code** a partir de ese diseño, y se publica en **Vercel**.

> ### ¿Eres diseñador/a y no programas?
> Este README es la parte técnica. Tu guía es **[EMPEZAR-AQUI.md](./EMPEZAR-AQUI.md)** —
> 3 pasos, sin términos técnicos.

## Cómo se usa

Abre Claude Code en esta carpeta y escribe:

```
/portfolio
```

Eso es todo. A partir de ahí Claude va pidiendo lo que necesita, de a una cosa
a la vez, y va haciendo el resto: lo primero que pregunta es el link del diseño.

Si ya tienes el link a mano, puedes pasarlo directo: `/portfolio <url>`.

### Qué pasa, paso a paso

| Paso | Qué hace | Te pregunta |
|---|---|---|
| 0 | Verifica que las skills de calidad estén instaladas | — |
| 1 | — | El link de tu diseño de Claude Design |
| 2 | Lee el diseño: secciones, colores, tipografías, textos | — |
| 3 | Construye cada sección como componente React | Solo lo que falte en el diseño (tu email, tu GitHub…) |
| 4 | Audita accesibilidad y contraste, y verifica que compila | — |
| 5 | Levanta la página en local para que la veas | Qué quieres ajustar antes de publicar |
| 6 | Publica en Vercel | Tu cuenta de Vercel (te abre el navegador para entrar) |
| 7 | — | Si quieres conectar un dominio propio |

Solo se detiene a esperarte en los pasos 5 y 6; el resto lo hace de corrido.

El comando está definido en `.claude/commands/portfolio.md` — se puede editar ahí.

## Si cambias el diseño después

Cambiar el diseño en Claude Design y volver a correr `/portfolio` **no rehace la
página**: compara el diseño nuevo contra lo que ya está construido y aplica solo
la diferencia.

```
/portfolio
```

Por ejemplo, si cambiaste el color principal y agregaste una sección:

> En tu diseño veo dos cambios: el color principal ahora es azul y agregaste una
> sección de testimonios. Aplico eso y dejo el resto igual.

Un cambio de color toca una línea en `globals.css`; las secciones que no
cambiaron ni se abren.

### Tus ajustes manuales se respetan

Los cambios que pides a mano (que no vienen del diseño) quedan anotados en
`.portfolio/design.json`. Si más adelante el diseño cambia esa misma sección,
el comando **se detiene y te pregunta** cuál de los dos quieres, en vez de pisar
tu ajuste en silencio.

### El snapshot

`.portfolio/design.json` guarda qué se construyó y desde qué diseño: la URL,
las secciones, los tokens de color y tipografía, el copy, y tus ajustes manuales.
Es lo que hace posible la comparación. Se versiona con el repo — no lo borres.

## Stack

- **Next.js 16** (App Router) + **React 19**
- **TypeScript**
- **Tailwind CSS v4**
- **Vercel** para hosting

Las convenciones de código están en `AGENTS.md` (que `CLAUDE.md` importa).

## Desarrollo local

```bash
npm install
npm run dev      # http://localhost:3000
npm run lint
npm run build
```

## Skills — de esto depende la calidad

Estas cuatro skills de [`vercel-labs/agent-skills`](https://github.com/vercel-labs/agent-skills)
son la diferencia entre una página que "funciona" y una bien hecha. Ya vienen
incluidas en `.claude/skills/`, así que al clonar el repo están listas.

| Skill | Cuándo actúa | Qué aporta |
|---|---|---|
| `vercel-react-best-practices` | Al escribir cada componente | 70 reglas de performance de React/Next de Vercel Engineering |
| `web-design-guidelines` | Antes del build | Accesibilidad, contraste, focus states, semántica, jerarquía |
| `deploy-to-vercel` | Al publicar | Deploy correcto y link de vuelta |
| `vercel-cli-with-tokens` | Si el login por navegador falla | Deploy con token |

El comando las invoca solo, y el Paso 0 verifica que estén antes de empezar.

Si alguna falta:

```bash
npx skills add vercel-labs/agent-skills \
  --skill deploy-to-vercel web-design-guidelines vercel-cli-with-tokens vercel-react-best-practices \
  --agent claude-code --copy -y
```

Gestionarlas:

```bash
npx skills list              # ver instaladas
npx skills update            # actualizar a la última versión
npx skills find <query>      # buscar más
```

> Se instalaron con `--copy` a propósito: los archivos quedan reales dentro del
> repo en vez de symlinks, así sobreviven al clonar en otra máquina.

## Publicar en un dominio propio

El deploy a Vercel ya da una URL (`*.vercel.app`). Para usar un dominio propio:

1. **Compra el dominio** en un registrador. Cloudflare Registrar lo vende a precio
   de costo sin markup; Namecheap y Porkbun también sirven.
2. **Despliega a producción**: `vercel --prod` (o pídeselo al comando).
3. **Agrega el dominio en Vercel**: en el dashboard del proyecto, Settings →
   Domains → Add, y escribe el dominio.
4. **Apunta el DNS** donde compraste el dominio, con lo que Vercel te indique:
   - Dominio raíz (`midominio.com`) → registro `A` a `76.76.21.21`
   - Subdominio (`www.midominio.com`) → registro `CNAME` a `cname.vercel-dns.com`
5. Espera la propagación (minutos normalmente, hasta 48h en el peor caso). El
   certificado HTTPS lo emite Vercel solo.

> Verifica los valores de DNS en el panel de Vercel al momento de configurarlo —
> son los que manda el dashboard, no los de esta tabla si llegan a diferir.

## Deploy con token (CI / sin login interactivo)

```bash
export VERCEL_TOKEN=<token>   # se genera en vercel.com/account/tokens
npx vercel --token "$VERCEL_TOKEN"          # preview
npx vercel --prod --token "$VERCEL_TOKEN"   # producción
```

No commitees el token. Va como variable de entorno o como secret del CI.
