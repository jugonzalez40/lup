# Empezar aquí

Esta guía es para publicar tu portafolio en internet a partir del diseño que ya
hiciste en Claude Design. No necesitas saber programar.

Son 3 pasos y la primera vez toma unos 15 minutos.

---

## Lo que necesitas antes de empezar

**1. Tu diseño de Claude Design, con su link a mano.**
Ábrelo y copia el link de la barra de direcciones.

**2. Node instalado en tu computador.**
Es el programa que arma la página. Si no lo tienes:
entra a [nodejs.org](https://nodejs.org), descarga el botón grande que dice
**LTS**, e instálalo dando "siguiente" hasta el final.

**3. Claude Code instalado.**
Es con quien vas a hablar para construir la página.

---

## Paso 1 — Abre la terminal

La terminal es esa ventana de texto donde le escribes a tu computador.

- **En Mac:** aprieta `Cmd + Espacio`, escribe `Terminal`, dale Enter.
- **En Windows:** busca `Terminal` en el menú de inicio.

Se va a abrir una ventana con texto. Es normal que se vea intimidante. Solo vas
a escribir dos cosas en toda la guía.

## Paso 2 — Escribe esto y dale Enter

```
claude
```

Eso abre a Claude dentro de la terminal.

## Paso 3 — Pega este mensaje

Copia esto tal cual, pégalo, y dale Enter:

```
Hola! Soy diseñadora, no programadora. Hice el diseño de mi portafolio en
Claude Design y quiero publicarlo en internet.

Descarga https://github.com/jugonzalez40/lup en una carpeta llamada
"portafolio", prepara todo lo que haga falta, y después lee el archivo
.claude/commands/portfolio.md y sigue esas instrucciones.

Guíame tú paso a paso: pregúntame una cosa a la vez y explícame en palabras
normales, sin términos técnicos. Empieza.
```

Listo. A partir de ahí Claude te va guiando.

---

## Qué te va a ir preguntando

En este orden, de a una cosa a la vez:

1. **El link de tu diseño** ← lo único que necesitas tener listo
2. Datos que le falten al diseño (tu email, tu GitHub, tu LinkedIn)
3. **Que revises la página** y digas qué ajustar — aquí revísala como revisas
   cualquier diseño tuyo: espaciados, jerarquía, cómo se ve en celular
4. **Tu cuenta de Vercel** para publicarla — te abre el navegador, entras con
   Google o GitHub, y vuelves a la terminal. Es gratis.
5. Si quieres un dominio propio (`tunombre.com`) o te sirve el link gratis

Todo lo demás lo hace solo.

---

## Si cambias el diseño después

Cambia lo que quieras en Claude Design, vuelve a abrir la terminal en la carpeta
`portafolio`, escribe `claude` y luego:

```
/portfolio
```

**No va a rehacer la página desde cero.** Compara tu diseño nuevo con lo que ya
está hecho y cambia solo lo que moviste. Si cambiaste un color, cambia ese color
y no toca nada más.

Y si antes le pediste algún ajuste a mano, te va a preguntar antes de pisarlo
en vez de borrártelo sin avisar.

---

## Si algo sale mal

**Escríbele a Claude lo que ves.** En serio — copia lo que salió en pantalla y
pégaselo. Está hecho para resolver eso y explicarte qué pasó.

Cosas que pasan seguido y no son tu culpa:

| Qué ves | Qué es |
|---|---|
| "command not found: node" | Falta instalar Node, o hay que cerrar la terminal y abrir una nueva |
| "command not found: claude" | Falta instalar Claude Code |
| No puede leer tu diseño | El link del diseño puede estar privado — revisa que esté compartido |
| Se queda quieto varios minutos | Normal la primera vez, está descargando lo que necesita |

Nada de lo que hagas aquí rompe tu diseño original. Si algo queda mal, se
vuelve a correr y ya.

---

## Para quien sí programa

Los detalles técnicos —stack, convenciones, cómo funciona el comando, cómo se
conecta un dominio— están en [`README.md`](./README.md).
