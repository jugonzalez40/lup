# lup — portafolio web

Portafolio personal. El diseño se hace en **Claude Design**, el código lo genera
**Claude Code** a partir de ese diseño, y se publica en **Vercel**.

## Cómo se usa

Abre Claude Code en esta carpeta y corre:

```
/portfolio <url-del-diseño-de-claude-design>
```

Eso es todo lo que hay que escribir. El comando no pregunta nada más: lee el
diseño desde la URL, lo implementa en React, audita la UI, verifica el build y
lo despliega a Vercel devolviendo el link.

Si corres `/portfolio` sin URL, lo único que va a preguntar es la URL.

### Qué hace por dentro

| Paso | Qué pasa |
|---|---|
| 1 | Lee el diseño desde la URL de Claude Design y extrae secciones, paleta, tipografías y copy |
| 2 | Implementa cada sección como un componente en `src/components/` |
| 3 | Audita la UI con `web-design-guidelines` y corrige accesibilidad/contraste/semántica |
| 4 | Corre `npm run lint && npm run build` y arregla lo que falle |
| 5 | Despliega a Vercel como preview y devuelve el link |

El comando está definido en `.claude/commands/portfolio.md` — se puede editar ahí.

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

## Skills instaladas

Vienen de [`vercel-labs/agent-skills`](https://github.com/vercel-labs/agent-skills)
y viven en `.claude/skills/`:

| Skill | Para qué |
|---|---|
| `deploy-to-vercel` | Desplegar y devolver el link |
| `web-design-guidelines` | Auditar UI: accesibilidad, contraste, semántica |
| `vercel-react-best-practices` | Patrones de performance de React/Next |
| `vercel-cli-with-tokens` | Deploy por CLI con token, sin login interactivo |

Gestionarlas:

```bash
npx skills list              # ver instaladas
npx skills update            # actualizar
npx skills find <query>      # buscar más
```

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
