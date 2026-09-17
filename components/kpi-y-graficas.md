# KPIs y gráficas

Cómo se pinta la analítica. Es la única zona del sistema donde el color significa algo.

## Anatomía de un KPI

```
┌──────────────────────────────────────┐
│ Views totales           ☆ ⤢ ⟳ ✕      │  ← título 11.5px 600 secondary + acciones 12px
│ [Meta 1M] [+12,4%]                   │  ← chips: target, delta, comparativa
│                                      │
│    847.2K                            │  ← valor 26/30/38px 800, tracking -.03em
│    ▁▂▄▆█▇▅▃                          │  ← viz: sparkline / barras / donut / gauge
│                                      │
│ Últimos 30 días · 12 clippers        │  ← sub 10.5px muted
└──────────────────────────────────────┘
```

```css
.kpi {
  background: var(--bg-surface); border: 1px solid var(--border);
  border-radius: 14px; padding: 12px 16px 14px;
  position: relative; height: 100%; overflow: hidden;
  display: flex; flex-direction: column;
  transition: border-color .15s ease;
}
.kpi:hover { border-color: var(--accent-border); }

/* Dentro de un .frame con puntitos → OPACA + lift */
.frame .kpi {
  background: var(--frame-bg);
  transition: transform .18s cubic-bezier(.22,.8,.28,1), border-color .18s, box-shadow .18s;
}
@media (hover: hover) {
  .frame .kpi:hover { transform: translateY(-3px); border-color: var(--hover-border);
                      box-shadow: 0 14px 32px rgba(0,0,0,0.32); }
  .frame .kpi:hover svg { transform: scale(1.14) rotate(-5deg); }
}
[data-theme="light"] .frame .kpi:hover { box-shadow: 0 14px 32px rgba(60,50,35,0.12); }
```

## Densidad por altura

El card mide su contenedor y elige variante. Tres variantes, tres tamaños de número:

| Variante | Alto | Número | Qué muestra |
|---|---|---|---|
| `compact` | < 140 px | 26 px | solo el valor, o sparkline + 1 número |
| `medium` | 140–220 px | 30 px | valor + gráfica con eje Y |
| `full` | > 220 px | 38 px | valor + ejes + leyenda + desglose + pie |

```css
.kpi-value { font-weight: 800; letter-spacing: -0.03em; line-height: 1; color: var(--text-primary);
             font-variant-numeric: tabular-nums; }
.kpi-unit  { font-size: 14px; margin-left: 3px; color: var(--text-muted); }  /* 18px en full */
.kpi-label { font-size: 11.5px; font-weight: 600; letter-spacing: .02em; color: var(--text-secondary); }
.kpi-sub   { font-size: 10.5px; color: var(--text-muted); margin-top: 6px; line-height: 1.4; }
```

## Chips del KPI

Van en una fila bajo el título, `gap: 6px`, `flex-wrap: wrap`.

- **Target** — "Meta 1M". Neutro. Solo el icono cambia según el estado
  (`CheckCircle2` / `AlertTriangle` / `XCircle`), 11 px.
- **Delta** — "+12,4 %" con flecha `TrendingUp` / `TrendingDown`.
  Neutro por defecto. Si la vista necesita señalar, se permite texto
  `var(--c-green2)` / `var(--c-red2)` — **nunca fondo de color**.
- **Comparativa** — "vs 754K periodo anterior". Siempre `--text-muted`.

## El marco de KPIs

La fila de KPIs de cabecera vive dentro de un `.frame` con puntitos.
Es el patrón "cajas que engloban cajas" (ver `cajas.md`).

```html
<section class="frame">
  <div class="kpi-row">
    <article class="kpi">…</article>
    <article class="kpi">…</article>
  </div>
</section>
```
```css
.kpi-row { display: grid; grid-template-columns: repeat(auto-fit, minmax(190px, 1fr)); gap: 14px; }
```

## Tipos de visualización

| Tipo | Cuándo | Nota |
|---|---|---|
| `value` | un número y ya | el 60 % de los KPIs |
| `sparkline` | tendencia sin escala | área tenue + punto final marcado |
| `line` | serie temporal con escala | etiquetas solo en los extremos |
| `bar` horizontal | ranking de categorías (top clippers) | barras monocromas al 100/70/40 % |
| `bar` vertical | volumen por día/semana | |
| `donut` | reparto por plataforma | máximo 6 segmentos + "otros" |
| `gauge` | progreso hacia una meta | `stroke-width: 9`, track `rgba(255,255,255,.08)` |
| `stars` | valoración | 5 estrellas, relleno parcial |
| `heat/timeline` | actividad por mes | celdas monocromas por intensidad |

## Gauge / anillo de progreso

Único sitio de la analítica donde el color es un **semáforo**:

```js
const color = pct < 30 ? '#ef4444' : pct < 70 ? '#fbbf24' : '#22c55e';
```
`stroke-width: 9`, track `rgba(255,255,255,0.08)`, arco con
`filter: drop-shadow(0 0 8px currentColor)` al 40 % de opacidad.

## Paleta de series (app de clipping)

```js
const PLATFORM = {
  tiktok:    '#38bdf8',
  youtube:   '#ef4444',
  instagram: '#e1306c',
  twitch:    '#a78bfa',
  x:         '#94a3b8',
};
const METRIC = { views: '#60a5fa', engagement: '#fbbf24', clips: '#10b981', pagos: '#34d399' };
const STATUS = { aprobado: '#10b981', rechazado: '#ef4444', pendiente: '#fbbf24' };
```

Y para donuts genéricos, en este orden exacto:
```js
['#60a5fa','#10b981','#fbbf24','#a78bfa','#22c55e','#f97316','#e1306c','#ef4444']
```

## Reglas de gráfica

- **Una escala por gráfica.** Dos ejes Y es una gráfica de más.
- Etiquetas solo en **los dos extremos** del eje X, no en cada punto.
- Área tenue bajo la línea (`opacity: .12` del color de serie), punto final enfatizado.
- `tabular-nums` en todo número, siempre.
- Colores de texto y grid desde tokens: nunca un gris hardcodeado.
- Si la gráfica **no** compara categorías semánticas, va monocroma:
  blanco al 100 / 70 / 40 %.
- Las gráficas no miden con `preserveAspectRatio="none"`: miden su contenedor real.
  Estirar un SVG deforma el grosor de las líneas.

## Modal de detalle

Click en un KPI → modal de cristal **opaco** con la serie completa, el desglose,
el enlace a los registros que lo originan ("ver los 84 clips") y el botón de export.
El KPI entero es el área clicable (`role="button"`, `tabIndex`, Enter/Espacio).
