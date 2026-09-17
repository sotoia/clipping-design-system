# 05 · Analítica

La pantalla con más densidad de datos. Lienzo de puntos, cabecera con filtros,
marco de favoritos, y rejilla de 12 columnas de módulos redimensionables.

```
main.dots (14px)
  [kick] ANALÍTICA
  [page-title · degradado de tinta] Qué funciona y qué no.
  ┌ barra de filtros ─────────────────────────────────────────────────┐
  │ [7d][30d][90d][Personalizado ▾]  [Plataforma ▾] [Clipper ▾]  ⟳ ⤓ │
  └───────────────────────────────────────────────────────────────────┘

  ┌─ .frame-lg · FAVORITOS (puntitos 12px) ───────────────────────────┐
  │  ┌ mini ┐ ┌ mini ┐ ┌ mini ┐ ┌ ⊹ añadir (ants) ┐                   │
  └───────────────────────────────────────────────────────────────────┘

  ┌─ rejilla 12 col · gap 14 ─────────────────────────────────────────┐
  │ ┌ span 3 ┐ ┌ span 3 ┐ ┌ span 3 ┐ ┌ span 3 ┐                       │
  │ ┌───────── span 8 ─────────────┐ ┌─ span 4 ─┐                     │
  │ ┌───────── span 12 ────────────────────────┐                      │
  └───────────────────────────────────────────────────────────────────┘
```

## Lienzo

```css
.ap-main { background-image: radial-gradient(var(--dots) 1px, transparent 1px); background-size: 14px 14px; }
[data-theme="light"] .ap-main { background-image: radial-gradient(rgba(20,24,31,0.055) 1px, transparent 1px); }
```

## Título

El único de la app con `font-weight: 850` y degradado de tinta (`.page-title`).

## Marco de favoritos

```css
.ap-favframe {
  position: relative; padding: 12px; border-radius: 16px;
  background: radial-gradient(var(--dots) 1px, transparent 1px) -1px -1px / 12px 12px, var(--frame-bg);
}
[data-theme="light"] .ap-favframe {
  background: radial-gradient(rgba(20,24,31,0.07) 1px, transparent 1px) -1px -1px / 12px 12px, #f4f2ee;
}
.ap-fav { background: var(--bg-surface); transition: transform .16s cubic-bezier(.22,.8,.28,1), border-color .16s, box-shadow .16s; }
@media (hover: hover) { .ap-fav:hover { transform: translateY(-2px); border-color: var(--hover-border); box-shadow: 0 10px 24px rgba(0,0,0,0.28); } }
```

## Cards de KPI

Fondo **opaco** (`--frame-bg`) para que los puntos del lienzo no sangren bajo el número.
Lift de 3 px y el icono gira ligeramente al hover. Detalle completo en
`components/kpi-y-graficas.md`.

```css
.ap-card { background: var(--frame-bg) !important;
           transition: transform .18s cubic-bezier(.22,.8,.28,1), border-color .18s, box-shadow .18s; }
@media (hover: hover) { .ap-card:hover { transform: translateY(-3px); border-color: var(--hover-border); box-shadow: 0 14px 32px rgba(0,0,0,0.32); } }
```

## Panel estático

Tablas y secciones que no se reordenan: opacas, **sin lift**.

```css
.ap-panel { background: var(--frame-bg); border-color: var(--border); }
```

## Añadir módulo

Tile con `border-color: transparent` + **marching ants** + icono `+` centrado y
"Añadir módulo" en 12 px muted. Abre un modal con buscador y los KPIs por categoría.

## Responsive de la rejilla

```css
@media (max-width: 768px)                          { .ap-grid > * { grid-column: span 12 !important; } }
@media (min-width: 769px) and (max-width: 1100px)  { .ap-grid > * { grid-column: span 6  !important; } }
```

## KPIs de la app de clipping

**Volumen** — clips subidos · clips aprobados · tasa de aprobación · clips por clipper
**Alcance** — views totales · views medias por clip · mejor clip · views por plataforma
**Calidad** — engagement medio · ratio de rechazo · motivo de rechazo (barras)
**Comunidad** — clippers activos · nuevos esta semana · retención · distribución de puestos
**Dinero** — coste por 1.000 views · pagado esta semana · pendiente de pago · presupuesto consumido
**Tiempo** — tiempo medio de revisión · clips por hora del día (heatmap) · hora punta de subida
