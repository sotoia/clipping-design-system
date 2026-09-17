# 01 · Login

Extraído de `cx.wismify.com/login` (componente `AuthShell` + `app/login/page.tsx`).
Es la pantalla más trabajada del sistema y la referencia de "cómo se ve bien".

## Anatomía

```
┌──────────────────────────────────────────────────┐
│  · · · · retícula de puntos 28px con máscara · · │
│      ◯ orbe verde 55vmax          ◯ orbe blanco  │
│                                                  │
│                   [logo 32px]  CX                │  ← logo + wordmark
│           Plataforma de clipping                 │  ← subtítulo 13px muted
│                                                  │
│      ┌──── tarjeta de cristal 420×— ────────┐    │
│      │  sheen esquinas + spotlight + tilt   │    │
│      │                                      │    │
│      │  Iniciar sesión            17px 700  │    │
│      │                                      │    │
│      │  [ G  Continuar con Google       ]   │    │
│      │  ──────── o con email ──────────     │    │
│      │  EMAIL                               │    │
│      │  [ tu@email.com                  ]   │    │
│      │  CONTRASEÑA                          │    │
│      │  [ ••••••••                  👁  ]   │    │
│      │               ¿Has olvidado…?  →     │    │
│      │  [  Entrar al panel              ]   │    │  ← CTA de marca
│      │                                      │    │
│      │       ¿No tienes cuenta? Crear       │    │
│      └──────────────────────────────────────┘    │
│                                                  │
│      Clipping — panel de la comunidad            │  ← pie 11px al 20 %
│                                                  │
│      ┌──── banner de cookies (cristal) ─────┐    │
└──────────────────────────────────────────────────┘
```

## Lienzo

```css
.au-page {
  min-height: 100vh; min-height: 100dvh;   /* dvh: barras del navegador móvil */
  display: flex; position: relative; overflow: hidden;
  background: var(--bg-base);
}
.au-wrap { position: relative; z-index: 10; width: 100%; max-width: 420px;
           padding: 44px 24px; margin: auto; }
```

`margin: auto` en el wrap (no `align-items: center` en el page): centra sin recortar
cuando el contenido supera el viewport. Con `center` el formulario se corta arriba
en móviles bajos.

### Retícula de puntos

```css
.au-dots {
  position: absolute; inset: 0; pointer-events: none;
  background-image: radial-gradient(circle, rgba(255,255,255,0.07) 1px, transparent 1px);
  background-size: 28px 28px;
  mask-image: radial-gradient(ellipse at center, #000 30%, transparent 80%);
  -webkit-mask-image: radial-gradient(ellipse at center, #000 30%, transparent 80%);
}
[data-theme="light"] .au-dots { background-image: radial-gradient(circle, rgba(15,23,42,0.10) 1px, transparent 1px); }
```

### Orbes

Tres, con tinte de marca en los dos primeros. **Esto consume el acento de la vista**
junto con la CTA — por eso en login no hay ninguna pill verde.

```css
.au-o1 { top:-22%; left:-8%;     width:55vmax; height:55vmax; filter:blur(70px);
         background: radial-gradient(circle, rgba(16,185,129,0.07), transparent 62%);
         animation: au-float-a 24s ease-in-out infinite alternate; }
.au-o2 { bottom:-25%; right:-10%; width:50vmax; height:50vmax; filter:blur(80px);
         background: radial-gradient(circle, rgba(16,185,129,0.045), transparent 62%);
         animation: au-float-b 32s ease-in-out infinite alternate; }
.au-o3 { top:30%; right:20%;      width:30vmax; height:30vmax; filter:blur(60px);
         background: radial-gradient(circle, rgba(255,255,255,0.04), transparent 60%);
         animation: au-float-c 28s ease-in-out infinite alternate; }
@keyframes au-float-a { to { transform: translate( 7vmax,  6vmax) scale(1.12); } }
@keyframes au-float-b { to { transform: translate(-6vmax, -7vmax) scale(0.92); } }
@keyframes au-float-c { to { transform: translate(-8vmax,  5vmax) scale(1.08); } }
```

## Logo

```css
.au-logo { text-align:center; margin-bottom:32px; }
.au-logo-row { display:flex; align-items:center; justify-content:center; gap:8px; margin-bottom:8px; }
.au-logo-row img { height:32px; opacity:.85; transition: transform .3s ease, opacity .3s ease; }
.au-logo:hover .au-logo-row img { transform: scale(1.05); opacity:1; }
.au-logo-row span {
  font: 800 22px Inter, sans-serif; letter-spacing:-0.02em; color: var(--brand);
  text-shadow: 0 0 24px rgba(var(--brand-rgb),0.4);
}
.au-logo p { margin:6px 0 0; font-size:13px; color: var(--text-muted); }
```

## Tarjeta de cristal

La pieza clave. Cuatro capas: degradado, sheen de esquinas, spotlight y tilt.

```css
.au-card {
  position: relative; border-radius: 20px;
  border: 1px solid rgba(255,255,255,0.12);
  background: linear-gradient(165deg, rgba(255,255,255,0.07) 0%, rgba(255,255,255,0.02) 40%, rgba(6,8,11,0.45) 100%);
  backdrop-filter: blur(18px) saturate(140%);
  -webkit-backdrop-filter: blur(18px) saturate(140%);
  box-shadow:
    0 30px 80px rgba(0,0,0,0.55),
    inset 0 1px 0 rgba(255,255,255,0.12),
    inset 0 -1px 0 rgba(0,0,0,0.3);
  transition: transform .35s cubic-bezier(.22,.8,.28,1);
  will-change: transform;
  animation: au-in .7s cubic-bezier(.16,1,.3,1) .12s backwards;
}
/* FIRMA: brillo en esquinas diagonales opuestas */
.au-card::before {
  content:''; position:absolute; inset:0; border-radius:inherit; pointer-events:none; z-index:0;
  background:
    radial-gradient(180px 110px at 0% 0%,     rgba(255,255,255,0.12), rgba(255,255,255,0.04) 45%, transparent 75%),
    radial-gradient(180px 110px at 100% 100%, rgba(255,255,255,0.09), rgba(255,255,255,0.03) 45%, transparent 75%);
}
/* Spotlight que sigue al ratón */
.au-card::after {
  content:''; position:absolute; inset:0; border-radius:inherit; pointer-events:none; z-index:0;
  opacity:0; transition: opacity .35s ease;
  background: radial-gradient(240px circle at var(--mx,50%) var(--my,0%), rgba(255,255,255,0.06), transparent 65%);
}
.au-card:hover::after { opacity: 1; }
.au-card-in { position: relative; z-index: 1; padding: 32px; }

@keyframes au-in {
  from { opacity:0; transform: translateY(18px) scale(.965); filter: blur(10px); }
  to   { opacity:1; transform:none; filter:blur(0); }
}
```

### Spotlight + tilt (JS)

Solo en dispositivos con hover y sin `prefers-reduced-motion`. Tilt máximo ±2.4°.

```js
const card = cardRef.current;
if (!window.matchMedia('(hover: hover)').matches) return;
if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;
const move = (e) => {
  const r = card.getBoundingClientRect();
  const x = e.clientX - r.left, y = e.clientY - r.top;
  card.style.setProperty('--mx', x + 'px');
  card.style.setProperty('--my', y + 'px');
  const rx = ((y / r.height) - 0.5) * -2.4;
  const ry = ((x / r.width)  - 0.5) *  2.4;
  card.style.transform = `perspective(1200px) rotateX(${rx}deg) rotateY(${ry}deg)`;
};
card.addEventListener('mousemove', move);
card.addEventListener('mouseleave', () => { card.style.transform = ''; });
```

### Tema claro de la tarjeta

El cristal oscuro está hardcodeado, así que el tema claro **se sobrescribe entero**:

```css
[data-theme="light"] .au-card {
  border: 1px solid rgba(15,23,42,0.10);
  background: linear-gradient(165deg, rgba(255,255,255,0.94) 0%, rgba(255,255,255,0.78) 45%, rgba(241,245,249,0.88) 100%);
  box-shadow: 0 30px 80px rgba(15,23,42,0.14), inset 0 1px 0 rgba(255,255,255,0.9), inset 0 -1px 0 rgba(15,23,42,0.05);
}
[data-theme="light"] .au-card::before {
  background:
    radial-gradient(180px 110px at 0% 0%,     rgba(255,255,255,0.95), rgba(255,255,255,0.4) 45%, transparent 75%),
    radial-gradient(180px 110px at 100% 100%, rgba(16,185,129,0.05), transparent 75%);
}
[data-theme="light"] .au-card::after {
  background: radial-gradient(240px circle at var(--mx,50%) var(--my,0%), rgba(15,23,42,0.045), transparent 65%);
}
```

## Entrada escalonada

Cada elemento entra con `au-stagger` y un `animationDelay` propio:

| Elemento | delay |
|---|---|
| logo | 0.05 s |
| tarjeta (`au-in`) | 0.12 s |
| título | 0.20 s |
| botón de Google | 0.26 s |
| divisor | 0.32 s |
| campo email | 0.38 s |
| campo contraseña | 0.44 s |
| "¿olvidaste…?" | 0.47 s |
| CTA | 0.50 s |
| "¿no tienes cuenta?" | 0.55 s |
| banner de cookies | 0.60 s |

```css
.au-st { animation: au-stagger .55s cubic-bezier(.33,1,.68,1) backwards; }
@keyframes au-stagger {
  from { opacity:0; transform: translateY(10px); filter: blur(6px); }
  to   { opacity:1; transform:none; filter:blur(0); }
}
```

`backwards` es obligatorio: sin él, el elemento parpadea visible antes de su turno.

## Piezas del formulario

- **Título**: 17 px 700, `margin: 0 0 22px`.
- **Botón Google**: `.btn-social`, 11px 14px, radius 11, SVG oficial de 18 px
  (único logo con color de la app). `margin-bottom: 20px`.
- **Divisor**: `<i></i><span>o con email</span><i></i>`.
- **Labels**: 11 px 600 uppercase tracking .05em muted.
- **Inputs**: radius 11, fondo `rgba(255,255,255,0.045)`, borde `rgba(255,255,255,0.12)`.
  Foco con **acento de marca** (`.brand-focus`).
- **Ojo de contraseña**: 16 px, `aria-label` + `title` traducidos.
- **"¿Has olvidado tu contraseña?"**: 12 px, alineado a la derecha, `margin-top: -6px`.
- **CTA**: `.btn-brand`, ancho completo, degradado `--brand → --brand-2`,
  barrido de brillo al hover, spinner de 14 px al enviar.
- **Error**: `.msg-error` con `shake .45s`. Se le pasa `key={error}` para que
  re-anime si el usuario falla dos veces seguidas.
- **Alternativa**: 13 px centrado, link en `--brand` 600 con
  `border-bottom: 1px solid transparent` → `rgba(brand,.6)` al hover.
- **Pie**: 11 px, `rgba(255,255,255,0.2)`.

## Banner de cookies

```css
.au-cookie {
  position: fixed; bottom:16px; left:50%; transform: translateX(-50%); z-index:9999;
  width: min(680px, calc(100vw - 32px));
  display:flex; align-items:center; justify-content:center; gap:16px; flex-wrap:wrap;
  padding: 13px 20px; border-radius: 14px;
  border: 1px solid rgba(255,255,255,0.12);
  background: linear-gradient(165deg, rgba(26,26,28,0.75), rgba(10,10,12,0.85));
  backdrop-filter: blur(14px) saturate(130%);
  box-shadow: 0 20px 60px rgba(0,0,0,0.5), inset 0 1px 0 rgba(255,255,255,0.1);
  animation: au-cookie-in .5s cubic-bezier(.16,1,.3,1) .6s backwards;
}
.au-cookie p { flex: 1 1 280px; margin:0; font-size:13px; color: var(--text-secondary); }
.au-cookie button { padding: 8px 20px; border-radius: 999px; border: none;
                    background: linear-gradient(135deg, var(--brand), var(--brand-2));
                    color:#fff; font: 600 13px Inter, sans-serif; cursor:pointer; }
```

Se guarda en `localStorage` con marca de tiempo y **caduca a los 90 días**
(`cookies_accepted` + `cookies_accepted_at`).

## Comportamiento

- Idioma: `?lang=` → cookie `locale` → `Intl.DateTimeFormat().resolvedOptions().timeZone`
  (Madrid/Canary → es) → `navigator.languages` → `es`.
- Login correcto → `window.location.href = '/dashboard'`
  (recarga completa, no `router.push`: hay que releer la sesión de Supabase).
- Google → `/api/auth/google/start?next=/dashboard`.
- El CSS de la pantalla se inyecta con `dangerouslySetInnerHTML`, no como children de
  `<style>`: React escapa los apóstrofes de `content: ''` y rompe la hidratación.

## Reduced motion

```css
@media (prefers-reduced-motion: reduce) {
  .au-amb i, .au-card, .au-st, .au-cookie, .au-error { animation: none !important; }
  .au-card { transition: none; }
}
```
