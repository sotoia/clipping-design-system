# Prompts de arranque

Dos prompts. El primero es para **Claude (web/app)**: genera las previews de diseño
de todas las pantallas. El segundo es para **Claude Code en VS Code**: construye la
app de verdad, con el mismo stack con el que está hecho Wismify.

Antes de pegarlos, sustituye `[NOMBRE]` por el nombre de la app y `[STREAMER]` por
el handle de tu amigo.

---

## 1 · Claude — previews de diseño

> Pégalo en una conversación nueva de Claude. Genera artifacts HTML, uno por pantalla.

```
Voy a construir [NOMBRE], una app de clipping para la comunidad de [STREAMER]:
los clippers suben clips de sus directos, el creador los revisa y aprueba, hay
rankings semanales e incentivos en metálico (200 € al clip más viral de la semana,
200 € a quien más clips apruebe).

El sistema de diseño completo está aquí, es público:
https://github.com/sotoia/clipping-design-system

Léelo entero antes de dibujar nada. En concreto:
- tokens/tokens.css es la ÚNICA fuente de color, espaciado y textura. Cópialo tal
  cual en cada artifact. No inventes ni un hex.
- foundations/01-filosofia.md son 10 reglas innegociables.
- pages/ tiene una ficha por pantalla con su esquema y sus reglas propias.
- preview/index.html es la referencia viva de cómo se ve bien.

Quiero que me hagas las previews en HTML, UNA PANTALLA POR ARTIFACT, en este orden:

 1. Login                      (pages/01-login.md)
 2. Registro por invitación    (pages/02-register.md)
 3. Dashboard del clipper      (pages/03-dashboard-clipper.md)
 4. Dashboard del creador      (pages/04-dashboard-creador.md)
 5. Subir clip                 (pages/08-clips.md §8.1)
 6. Mis clips                  (pages/08-clips.md §8.2)
 7. Cola de revisión           (pages/08-clips.md §8.3)
 8. Ranking                    (pages/06-ranking.md)
 9. Incentivos                 (pages/07-incentivos.md)
10. Analítica                  (pages/05-analitica.md)
11. Ajustes                    (pages/09-ajustes.md)
12. Landing pública            (pages/10-web-publica.md)

Requisitos de cada artifact:
- HTML autocontenido con el CSS inline. Inter desde Google Fonts.
- Un botón arriba a la derecha que conmuta data-theme="dark|light" y otro que
  conmuta data-shell="classic|modern". Las dos combinaciones tienen que verse bien.
- Datos de ejemplo REALISTAS y en español: handles de clipper, views de 6-7 cifras
  con separador de miles, fechas relativas, importes en euros.
- Cero emojis. Cero iconos de color. SVG de trazo fino con currentColor, stroke-width 1.8.
- tabular-nums en toda cifra.
- EXACTAMENTE un elemento con var(--brand) por pantalla. Al final de cada artifact
  dime en una línea cuál has elegido y por qué.
- Responsive: sin scroll horizontal a 390px de ancho.

Antes de empezar con la 1, dime en 5 líneas qué has entendido del sistema y qué
elemento se va a llevar el acento en cada una de las 12 pantallas. Si algo de las
fichas te parece que choca con la app de clipping, dímelo entonces, no a mitad.

Luego vamos pantalla a pantalla: hazme la 1, la reviso, y sigues.
```

---

## 2 · Claude Code (VS Code) — construir la app

> Pégalo en Claude Code dentro de la carpeta vacía del proyecto.

```
Vamos a construir [NOMBRE], una app de clipping para la comunidad del streamer
[STREAMER]. Mismo stack y mismas convenciones con las que está hecho mi panel
Wismify CX, que conozco bien y quiero replicar.

STACK (exacto, no lo cambies sin decírmelo antes)
- Next.js 14 App Router + React 18 + TypeScript 5
- Supabase: @supabase/supabase-js y @supabase/ssr (auth con cookies, RLS en todas
  las tablas). Auth por email/contraseña + Google OAuth.
- lucide-react para iconos
- Stripe para los pagos salientes a clippers (Connect) — más adelante, no en el MVP
- nodemailer para los emails de plataforma
- Workers sueltos ejecutados con tsx (así corren los de Wismify)
- Despliegue: pm2 en cluster + nginx como reverse proxy
- Sin librería de componentes. El diseño va en app/globals.css con CSS custom
  properties, igual que en Wismify.

DISEÑO — esto es lo primero que tienes que leer
El sistema está en https://github.com/sotoia/clipping-design-system (público).
- Clona o descarga el repo y copia tokens/tokens.css a app/globals.css como base.
- Lee CLAUDE.md del repo y cópialo a la raíz de este proyecto, porque son las
  reglas que quiero que sigas en cada componente.
- Lee foundations/01-filosofia.md. Son 10 reglas y ninguna es opcional.
- Cada vez que vayas a crear una pantalla, lee antes su ficha en pages/.
- Los componentes NUNCA escriben un hex. Solo var(--…). Las dos excepciones son
  var(--brand) (un elemento por vista) y los colores de serie de las gráficas.
- Dos modos de shell: data-shell="classic" (sidebar 260px) y data-shell="modern"
  (dock flotante estilo macOS). Ambos en components/navegacion.md.
- Cero emojis en todo el producto.

MODELO DE DATOS (punto de partida, discútelo conmigo antes de crear las tablas)
- profiles        id, handle, display_name, avatar_url, role ('clipper'|'creator'),
                  shell_mode, theme, payout_method, payout_ref, created_at
- clips           id, clipper_id, title, platform, url, thumb_url, duration_s,
                  published_at, views, likes, comments, status
                  ('pending'|'approved'|'rejected'|'paid'), reject_reason,
                  reviewed_by, reviewed_at, created_at
- clip_metrics    clip_id, captured_at, views, likes, comments   (histórico diario)
- weeks           id, starts_at, ends_at, budget_cents, status
- incentives      id, week_id, key, title, rule, prize_cents, winner_id, status
- payouts         id, clipper_id, week_id, amount_cents, status, paid_at, reference
RLS: un clipper solo ve sus clips y sus pagos; el creador ve todo lo de su comunidad.

ORDEN DE TRABAJO
Fase 0 — andamiaje: Next 14, TS estricto, globals.css con los tokens, layout raíz
  que aplica data-theme y data-shell antes del primer pintado (script inline),
  shell con sidebar y con dock, y las dos rutas de auth.
Fase 1 — auth: /login y /register calcados de pages/01-login.md y 02-register.md,
  incluido el AuthShell (cristal con sheen en dos esquinas, spotlight, tilt ±2.4°,
  escalonado con los delays exactos de la ficha, orbes, banner de cookies).
  Registro por invitación con ?invite=<token>.
Fase 2 — clipper: dashboard, subir clip, mis clips.
Fase 3 — creador: dashboard, cola de revisión con atajos de teclado (A/R/J/K).
Fase 4 — competición: ranking en vivo con Realtime de Supabase, incentivos.
Fase 5 — analítica y ajustes.
Fase 6 — landing pública.

CÓMO QUIERO QUE TRABAJES
- Una fase por vez. Al acabar cada una, párate y enséñame qué has hecho.
- Antes de escribir código de una pantalla, dime en 3 líneas qué vas a montar y
  qué elemento se lleva el acento de esa vista.
- Comentarios en español, en el estilo del código de Wismify: explican POR QUÉ,
  no qué hace la línea.
- Nada de alert() ni confirm(): modales propios.
- Todo hover dentro de @media (hover: hover). prefers-reduced-motion respetado.
- Todas las cifras con tabular-nums.
- Español por defecto en el panel; deja los textos preparados para ES/EN como en
  Wismify (t({ es, en })), aunque de momento solo usemos español.

Empieza por la Fase 0. Antes de tocar nada, léete el repo del sistema de diseño
y dime qué has entendido y qué decisiones te faltan por saber de mí.
```

---

## Cómo encadenarlos

1. Lanza el prompt 1 y valida las 12 pantallas como imágenes. Es barato iterar ahí.
2. Cuando una preview te convenza, guarda su HTML en `preview/` de este repo.
3. Lanza el prompt 2 en VS Code. Cuando Claude Code llegue a esa pantalla, dile
   "hazla como `preview/06-ranking.html`" y tendrá la referencia exacta en vez de
   una descripción.
