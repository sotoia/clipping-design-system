# Modales, toasts y tooltips

## Modal

**Opaco.** Un modal con información crítica nunca es translúcido: el contenido de
detrás lo hace ilegible. El cristal se consigue con el blur del fondo, no con la caja.

```css
.overlay {
  position: fixed; inset: 0; z-index: 999;
  display: flex; align-items: center; justify-content: center;
  background: var(--overlay);                 /* rgba(3,4,5,.5) · light rgba(40,34,24,.28) */
  backdrop-filter: blur(6px);
  animation: fadeIn .2s ease;
}
.modal {
  width: 520px; max-width: calc(100vw - 32px); max-height: calc(100vh - 64px);
  display: flex; flex-direction: column; overflow: hidden;
  background: rgba(24,24,26,0.90);
  backdrop-filter: blur(22px) saturate(140%);
  border: 1px solid var(--border-2); border-radius: 16px;
  box-shadow: var(--glass-shadow);
  animation: fadeInScale .2s ease;
}
[data-theme="light"] .modal { background: rgba(255,255,255,0.92); }

.modal-header {
  display: flex; align-items: center; justify-content: space-between; gap: 12px;
  padding: 16px 20px; border-bottom: 1px solid var(--border);
}
.modal-header h3 { font: 800 15px Inter, sans-serif; color: var(--text-primary); }
.modal-body   { padding: 20px; overflow-y: auto; }
.modal-footer { padding: 14px 20px; border-top: 1px solid var(--border);
                display: flex; justify-content: flex-end; gap: 8px; }
```

Anchos: `420px` confirmación · `520px` estándar · `720px` detalle de KPI ·
`min(1100px, 92vw)` reproductor de clip.

Título con icono: SVG de 30 px en **círculo** de cristal
(`border-radius: 50%`, `padding: 7px`, borde `--border-2`, fondo `--bg-surface-2`).

Cierre: botón ✕ de 16 px arriba a la derecha, `Esc`, y click en el overlay.
Foco atrapado dentro del modal. Al cerrar, el foco vuelve al disparador.

**Nunca `alert()` / `confirm()` / `prompt()`.**

## Toast

```css
.toast {
  position: fixed; left: 50%; bottom: 24px; transform: translateX(-50%);
  z-index: 9999;
  display: flex; align-items: center; gap: 10px;
  padding: 11px 16px; border-radius: 12px;
  background: var(--bg-elevated); border: 1px solid var(--border-2);
  box-shadow: var(--shadow-lg);
  font: 600 13px Inter, sans-serif; color: var(--text-primary);
  animation: toast-in .2s cubic-bezier(.16,1,.3,1);
}
@keyframes toast-in { from { opacity:0; transform: translate(-50%, 12px) } to { opacity:1; transform: translate(-50%,0) } }
```

Uno a la vez, 4 s, con acción opcional ("Deshacer") a la derecha en `--text-secondary`.
En modo modern el toast sube a `bottom: 92px` para no chocar con el dock.

## Tooltip

Por portal, `z-index` por encima de todo. Aparece a los 400 ms, desaparece al instante.

```css
.tooltip {
  padding: 10px 13px; min-width: 240px; max-width: 360px;
  background: var(--bg-elevated); border: 1px solid var(--border); border-radius: 8px;
  box-shadow: 0 12px 40px rgba(0,0,0,0.55), 0 0 0 1px rgba(255,255,255,0.04);
}
.tooltip b { display: block; font: 700 12.5px Inter, sans-serif; color: var(--text-primary); margin-bottom: 4px; }
.tooltip p { font-size: 11.5px; color: var(--text-secondary); line-height: 1.5; }
```

En los KPIs el tooltip se dispara desde el **título** (`cursor: help`) y explica qué
mide exactamente ese dato. Es el sitio donde vive la definición del KPI.

## Popover / menú

```css
.popover {
  background: var(--bg-elevated); border: 1px solid var(--border-2); border-radius: 12px;
  box-shadow: var(--shadow-lg); padding: 6px; min-width: 200px;
  animation: fadeInScale .16s ease;
}
.popover-item {
  display: flex; align-items: center; gap: 9px;
  padding: 8px 10px; border-radius: 8px;
  font: 500 12.5px Inter, sans-serif; color: var(--text-secondary); cursor: pointer;
}
.popover-item:hover { background: var(--bg-hover); color: var(--text-primary); }
.popover-sep { height:1px; background: var(--border); margin: 5px 0; }
```

## Banner de cookies

Fijo abajo, centrado, cristal, `width: min(680px, calc(100vw - 32px))`, radius 14,
entra a los 0.6 s. Un solo botón "Aceptar" en pill. Ver `pages/01-login.md`.
