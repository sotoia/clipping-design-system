# Navegación — dos modos

La app tiene **dos modos de shell** que el usuario elige en Ajustes y se guardan en
`localStorage` + perfil. El contenido no cambia; cambia el contenedor.

```
localStorage: "shell-mode" → "classic" | "modern"
<html data-shell="classic|modern">
```

Ambos modos existen para los dos roles (clipper y creador). Lo que cambia entre roles
son los ítems del menú, no el modo.

---

## Modo CLÁSICO — sidebar

```css
.sidebar {
  width: var(--sidebar-width);            /* 260px · colapsado 72px */
  min-width: var(--sidebar-width);
  height: 100vh; position: sticky; top: 0; overflow-y: auto;
  background: var(--sidebar-bg);
  border-right: 1px solid var(--border);
  padding: 20px 16px;
  display: flex; flex-direction: column;
  backdrop-filter: blur(12px);
  z-index: 10; flex-shrink: 0;
}
.sidebar-header {
  display: flex; align-items: center; gap: 10px;
  margin-bottom: 24px; padding-bottom: 20px;
  border-bottom: 1px solid var(--border);
}
.sidebar-header .logo { width: 28px; height: 28px; filter: var(--logo-filter); }
.sidebar-header .title { font: 700 14px Inter, sans-serif; letter-spacing: .5px; color: var(--text-primary); }

.menu { display: flex; flex-direction: column; gap: 3px; margin-top: 4px; }
.menu .item {
  display: flex; align-items: center; gap: 10px;
  padding: 9px 12px; border-radius: 8px;
  font: 500 13px Inter, sans-serif; color: var(--text-secondary);
  cursor: pointer; border: 1px solid transparent;
  transition: background .15s ease, color .15s ease;
}
.menu .item:hover  { background: var(--nav-hover-bg); color: var(--text-primary); }
.menu .item.active {
  background: var(--nav-active-bg);
  border-color: var(--accent-border);
  color: var(--text-primary);
  box-shadow: inset 0 1px 0 rgba(255,255,255,0.09);
}
.menu .item.disabled { opacity: .35; pointer-events: none; }
.menu .sep { font: 700 10px Inter, sans-serif; text-transform: uppercase; letter-spacing: .14em;
             color: var(--text-muted-2); padding: 14px 12px 6px; }
```

- Iconos de 15 px. El ítem activo se marca con **fondo + borde + highlight interior**,
  nunca con el color de marca.
- Colapsado (72 px): solo iconos, centrados, con tooltip por portal a la derecha.
- Pie del sidebar: tarjeta de usuario (avatar 28 px + nombre + rol), toggle de tema
  (`Sun` / `Moon`, 15 px) y salir.
- Móvil: el sidebar sale de scroll y se sustituye por bottom nav de 5 ítems.

---

## Modo MODERN — dock flotante

Barra horizontal flotante abajo, estilo macOS: iconos por pestaña, cristal, con
magnificación al hover y etiqueta emergente.

```html
<nav class="dock" data-shell="modern">
  <button class="dock-item is-active"><svg …/><span class="dock-tip">Dashboard</span></button>
  <button class="dock-item"><svg …/><span class="dock-tip">Mis clips</span></button>
  <i class="dock-sep"></i>
  <button class="dock-item"><svg …/><span class="dock-tip">Ranking</span></button>
  …
</nav>
```

```css
.dock {
  position: fixed; left: 50%; bottom: 20px; transform: translateX(-50%);
  z-index: 60;
  display: flex; align-items: flex-end; gap: 6px;
  padding: 8px 10px;
  border-radius: 20px;
  background: var(--dock-bg);
  border: 1px solid var(--dock-border);
  box-shadow: var(--dock-shadow);
  backdrop-filter: blur(20px) saturate(150%);
  -webkit-backdrop-filter: blur(20px) saturate(150%);
}
.dock::before {                        /* el sheen de la casa, también aquí */
  content:''; position:absolute; inset:0; border-radius:inherit; pointer-events:none;
  background: var(--sheen);
}
.dock-item {
  position: relative; z-index: 1;
  width: 44px; height: 44px; border: none; border-radius: 13px;
  background: transparent; color: var(--text-secondary); cursor: pointer;
  display: inline-flex; align-items: center; justify-content: center;
  transition: transform .18s cubic-bezier(.22,.8,.28,1), background .18s, color .18s;
}
.dock-item svg { width: 22px; height: 22px; stroke-width: 1.8; }
@media (hover: hover) {
  .dock-item:hover { transform: translateY(-9px) scale(1.18); color: var(--text-primary);
                     background: var(--bg-surface-2); }
  /* magnificación de los vecinos — el detalle que lo hace parecer macOS */
  .dock-item:hover + .dock-item,
  .dock-item:has(+ .dock-item:hover) { transform: translateY(-4px) scale(1.08); }
}
.dock-item.is-active { color: var(--text-primary); background: var(--bg-surface-2); }
.dock-item.is-active::after {          /* el punto de "abierto", como en macOS */
  content:''; position:absolute; bottom:-5px; left:50%; transform:translateX(-50%);
  width:4px; height:4px; border-radius:50%; background: var(--text-muted);
}
.dock-sep { width:1px; align-self:stretch; margin:6px 4px; background: var(--border-2); }

/* Etiqueta emergente */
.dock-tip {
  position: absolute; bottom: calc(100% + 12px); left: 50%; transform: translateX(-50%) translateY(4px);
  padding: 5px 10px; border-radius: 8px; white-space: nowrap;
  background: var(--bg-elevated); border: 1px solid var(--border-2);
  box-shadow: var(--shadow-md);
  font: 600 11px Inter, sans-serif; color: var(--text-primary);
  opacity: 0; pointer-events: none;
  transition: opacity .18s ease, transform .18s ease;
}
@media (hover: hover) { .dock-item:hover .dock-tip { opacity: 1; transform: translateX(-50%) translateY(0); } }

/* Móvil: el dock ocupa el ancho, sin magnificación */
@media (max-width: 720px) {
  .dock { left: 12px; right: 12px; bottom: 12px; transform: none;
          justify-content: space-around; gap: 0; border-radius: 18px; }
  .dock-item:hover { transform: none; }
  .dock-tip { display: none; }
}
```

En modo modern:
- `.main` ocupa el ancho completo y lleva `padding-bottom: 104px`.
- La topbar se vuelve **flotante**: `position: sticky; top: 12px`, ancho
  `calc(100% - 32px)`, radius 14, cristal, con el título de la página, el buscador
  y el avatar.
- Los ítems secundarios (ajustes, salir, tema) viven en el menú del avatar de la topbar,
  no en el dock. **Máximo 7 iconos en el dock** + 1 separador.

## Ítems del menú

**Clipper**
`Dashboard · Subir clip · Mis clips · Ranking · Incentivos · Pagos · Ajustes`

**Creador**
`Dashboard · Revisión · Clips · Clippers · Analítica · Incentivos · Pagos · Campañas · Ajustes`

## Topbar

```css
.topbar {
  height: var(--topbar-h);
  display: flex; align-items: center; gap: 14px;
  padding: 0 20px;
  border-bottom: 1px solid var(--border);
  background: color-mix(in srgb, var(--bg-base) 82%, transparent);
  backdrop-filter: blur(12px);
  position: sticky; top: 0; z-index: 20;
}
/* Variante flotante (modo modern) */
[data-shell="modern"] .topbar {
  margin: 12px 16px 0; border-radius: 14px; border: 1px solid var(--border);
  box-shadow: var(--shadow-md);
}
```

## Tabs

```css
.tabs { display: flex; gap: 2px; border-bottom: 1px dashed var(--border-2); }
.tab {
  padding: 9px 14px; font: 600 12.5px Inter, sans-serif; color: var(--text-secondary);
  background: transparent; border: none; border-bottom: 2px solid transparent;
  cursor: pointer; transition: color .15s, border-color .15s;
}
.tab:hover { color: var(--text-primary); }
.tab.active { color: var(--text-primary); border-bottom-color: var(--text-primary); }
```

El separador de la tira de pestañas es **discontinuo** — es una zona secundaria.
