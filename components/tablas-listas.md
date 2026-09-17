# Tablas, listas y rankings

## Tabla

```css
.table { width: 100%; border-collapse: collapse; }
.table th {
  font: 700 10.5px Inter, sans-serif; text-transform: uppercase; letter-spacing: .12em;
  color: var(--text-muted); text-align: left;
  padding: 10px 12px; border-bottom: 1px solid var(--border-2);
  position: sticky; top: 0; background: var(--bg-base); z-index: 2;
}
.table td {
  font-size: 13px; color: var(--text-primary);
  padding: 12px; border-bottom: 1px solid var(--border);
}
.table tbody tr { transition: background .15s ease; }
.table tbody tr:hover { background: rgba(255,255,255,0.03); }
[data-theme="light"] .table tbody tr:hover { background: rgba(15,20,30,0.025); }
.table .col-hl { background: rgba(255,255,255,0.035); }   /* columna destacada */
.table .num { text-align: right; font-variant-numeric: tabular-nums; }
```

Las cifras siempre a la derecha y con `tabular-nums`. Sin esto, una tabla de views
es ilegible.

## Fila de lista (clip, pago, usuario)

```css
.row {
  display: flex; align-items: center; gap: 14px;
  padding: 14px 20px;
  background: var(--bg-card); border: 1px solid var(--border); border-radius: 10px;
  border-left: 3px solid transparent;          /* ← la única señal de color */
  transition: transform .18s cubic-bezier(.22,.8,.28,1), border-color .18s, box-shadow .18s;
}
@media (hover: hover) {
  .row:hover { transform: translateY(-1px); border-color: var(--hover-border);
               box-shadow: 0 10px 24px rgba(0,0,0,0.30); }
}
.row.is-hot      { border-left-color: var(--c-red); }     /* viral / urgente */
.row.is-pending  { border-left-color: var(--c-amber2); }
.row.is-ok       { border-left-color: var(--brand); }
```

**El borde izquierdo es la única señal cromática de una lista.** Los chips de estado
que van dentro son grises.

## Ranking

La tabla más importante de la app. Tres zonas: podio, resto, tú.

```
┌─ .frame (puntitos) ──────────────────────────────────────┐
│  RANKING SEMANAL          [pill-live · SEMANA ACTIVA]    │
│                                                          │
│  ┌ #1 ─────────────┐ ┌ #2 ────────┐ ┌ #3 ────────┐       │  ← podio: 3 cards opacas
│  │ avatar 46px     │ │            │ │            │       │
│  │ @nombre         │ │            │ │            │       │
│  │ 1,2M views      │ │            │ │            │       │
│  │ [chip 24 clips] │ │            │ │            │       │
│  └─────────────────┘ └────────────┘ └────────────┘       │
│                                                          │
│  ── tabla del 4 al 50 ──────────────────────────────     │
│                                                          │
│  ┌ .sub dashed · TU POSICIÓN ─────────────────────┐      │  ← sticky al fondo
│  │ #17  @tu_nombre   184K views   6 clips          │     │
│  └─────────────────────────────────────────────────┘     │
└──────────────────────────────────────────────────────────┘
```

- **Podio:** tres cards opacas dentro del marco, la #1 un 8 % más alta.
  El número de puesto va en un cuadrado 26 px radius 8, `font-weight: 800`,
  fondo `--bg-surface-2`. **Nada de medallas de colores ni emojis.**
- **Diferenciar el #1** con el borde en `--brand-border` — y eso consume el acento
  de la vista, así que la pill-live de cabecera pasa a `.pill` neutra.
- **Tabla del 4 en adelante:** avatar 26 px, `@handle`, views, clips, engagement, delta.
  `tabular-nums` obligatorio. Filas de 44 px.
- **Tu fila:** `.sub` discontinua, `position: sticky; bottom: 0`, con el mismo layout de
  columnas que la tabla. Siempre visible aunque estés el 240.

```css
.rank-pos {
  width: 26px; height: 26px; border-radius: 8px;
  display: inline-flex; align-items: center; justify-content: center;
  background: var(--bg-surface-2); border: 1px solid var(--border);
  font: 800 12px Inter, sans-serif; font-variant-numeric: tabular-nums;
  color: var(--text-primary);
}
.rank-me { position: sticky; bottom: 0; z-index: 3; backdrop-filter: blur(10px); }
```

## Avatares

```css
.avatar { border-radius: 999px; object-fit: cover; border: 1px solid var(--border-2);
          background: var(--bg-surface-2); }
```
Tamaños: 22 px (tabla densa) · 26 px (fila) · 32 px (cabecera) · 46 px (podio) · 72 px (perfil).
Sin foto → iniciales en 700, `--text-secondary`, sobre `--bg-surface-2`.

## Grid de clips

```css
.clip-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 16px; }
.clip {
  background: var(--bg-card); border: 1px solid var(--border); border-radius: 14px;
  overflow: hidden; transition: transform .22s cubic-bezier(.22,.8,.28,1), border-color .22s, box-shadow .22s;
}
.clip-thumb { aspect-ratio: 16/9; background: var(--bg-surface-2); position: relative; }
.clip-thumb::after {                    /* velo para que el texto encima sea legible */
  content:''; position:absolute; inset:0;
  background: linear-gradient(0deg, rgba(0,0,0,.72) 0%, rgba(0,0,0,0) 55%);
}
.clip-dur { position:absolute; right:8px; bottom:8px; z-index:1; }   /* .pill */
.clip-body { padding: 12px 14px; }
```

Nunca texto sobre miniatura sin velo. El velo arranca del lado donde está el texto y
termina en transparente.

## Paginación

Texto simple: `‹ 1 2 3 … 12 ›`, 12 px, `--text-secondary`, activo en `--text-primary`
con fondo `--bg-surface-2` y radius 8. Sin botones grandes.
En listas largas, scroll infinito con un sentinel y skeletons del alto exacto.
