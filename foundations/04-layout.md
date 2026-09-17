# 04 · Layout y espaciado

## Shell de la app

La app tiene **dos modos de navegación** y el mismo contenido sirve para ambos.
Ver `components/navegacion.md` y `pages/00-shell-modos.md`.

```
Modo CLÁSICO                        Modo MODERN (dock)
┌──────────┬────────────────┐       ┌──────────────────────────┐
│ sidebar  │ topbar 56px    │       │ topbar flotante 56px     │
│ 260px    ├────────────────┤       ├──────────────────────────┤
│ sticky   │ main           │       │ main (ancho completo)    │
│          │ padding 28/32  │       │ padding 28px 32px 104px  │
│          │                │       │      ┌──────────┐        │
└──────────┴────────────────┘       └──────┤   dock   ├────────┘
                                           └──────────┘
```

```css
.app  { display: flex; height: 100vh; overflow: hidden; background: var(--bg-base); }
.main { padding: 28px 32px; overflow-y: auto; overflow-anchor: none;
        width: calc(100vw - var(--sidebar-current-width));
        transition: width 300ms ease-in-out; }
```

- Sidebar: `260px` (colapsado `72px`), `position: sticky`, `padding: 20px 16px`,
  `backdrop-filter: blur(12px)`, borde derecho de 1 px.
- Topbar: `56px`.
- Main en modo dock: `padding-bottom: 104px` para que el dock no tape contenido.

## Rejillas

| Contexto | Rejilla |
|---|---|
| Fila de KPIs | `repeat(auto-fit, minmax(190px, 1fr))`, `gap: 14px` |
| Dashboard modular | 12 columnas, `gap: 14px`; en ≤768px todo a `span 12`; 769–1100px a `span 6` |
| Grid de clips | `repeat(auto-fill, minmax(240px, 1fr))`, `gap: 16px` |
| Tabla de ranking | tabla real, no grid (necesita `tabular-nums` alineados) |

## Radios

| Elemento | Radio |
|---|---|
| Pills, chips, avatares | `999px` |
| Botones de app | `8–11px` |
| Tarjeta pequeña | `12–14px` |
| Tarjeta / card | `16px` |
| Marco que engloba cajas | `18px` |
| Modal | `16–20px` |
| Tarjeta de auth | `20px` |
| Dock | `20px` |
| Miniatura de clip (16:9) | `12px` |

## Espaciado

- Entre secciones de una página: `24–32px`.
- Padding interior de card: `18–22px` (12–16 en cards densas de KPI).
- `gap` de rejilla: `14–16px`.
- Entre label e input: `6px`. Entre campos de un formulario: `16px`.

## Sombras

Nunca sombras de color. Solo las cuatro del sistema:

```
--shadow-sm    0 1px 3px  rgba(0,0,0,0.40)
--shadow-md    0 4px 16px rgba(0,0,0,0.50)
--shadow-lg    0 16px 48px rgba(0,0,0,0.60)
--glass-shadow 0 20px 60px rgba(0,0,0,0.45), inset 0 1px 0 rgba(255,255,255,0.10), inset 0 -1px 0 rgba(0,0,0,0.28)
```

En tema claro todas se sustituyen por versiones cálidas `rgba(60,50,35,α)`.

## Responsive

- ≤ 768px: sidebar oculto → bottom nav (clásico) o el dock se hace ancho completo (modern).
- Nunca scroll horizontal en `body`. Tablas y gráficas anchas dentro de su propio
  contenedor con `overflow-x: auto`.
- Los `padding` de `.main` bajan a `20px 16px`.
