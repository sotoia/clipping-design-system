# 03 · Tipografía

**Inter** en todo. Pesos 400 / 500 / 600 / 700 / 800 (850 y 900 solo display).
Mono para IDs, tokens, timestamps y URLs: `ui-monospace, "SF Mono", Menlo, Consolas`.

```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
```

## Escala

| Rol | Tamaño | Peso | Tracking | Color |
|---|---|---|---|---|
| Base / cuerpo | 13 px / 1.5 | 400 | — | `--text-primary` |
| Titular de página | 26 px | 850 | −0.02em | degradado de tinta (ver abajo) |
| Título de sección | 15–17 px | 700 | — | `--text-primary` |
| Título de tarjeta | 12.5–13.5 px | 700 | — | `--text-primary` |
| Cuerpo secundario | 11.5–12.5 px | 400 | — | `--text-secondary` |
| Kicker / label | 10–11 px | 700 | .12–.16em, UPPER | `--text-muted` |
| Label de input | 11 px | 600 | .05em, UPPER | `--text-muted` |
| Número KPI | 26 / 30 / 38 px | 800 | −0.03em, línea 1 | `--text-primary` |
| Unidad del KPI | 14–18 px | 400 | — | `--text-muted` |
| Pie de KPI | 10.5 px | 400 | — | `--text-muted` |
| Mono (ids, tokens) | 11–12 px | 400 | — | `--text-secondary` |

Tres tamaños de número KPI según el alto del contenedor:
`compact (<140px) → 26` · `medium (140–220) → 30` · `full (>220) → 38`.

## Titular con degradado de tinta

La firma tipográfica de la casa. El titular se desvanece de arriba a abajo:

```css
.page-title {
  font-size: 26px; font-weight: 850; letter-spacing: -0.02em; text-wrap: balance;
  background: linear-gradient(178deg, var(--text-primary) 32%, var(--text-muted));
  -webkit-background-clip: text; background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

## Reglas

- `text-wrap: balance` en todos los titulares.
- `font-variant-numeric: tabular-nums` en **cualquier** columna de cifras y en todo KPI.
  Sin esto, los números bailan al actualizarse en vivo.
- `letter-spacing` negativo en todo lo ≥ 20 px.
- Nunca dos pesos distintos en la misma línea salvo dato + unidad.
- Frases cortas con punto final. Kickers en mayúsculas con tracking. Sin exclamaciones.
