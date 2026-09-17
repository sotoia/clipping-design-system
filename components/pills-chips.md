# Pills, chips y badges

Tres piezas distintas. No se mezclan.

| Pieza | Para qué | Color |
|---|---|---|
| `.pill` | Metadato de cristal en cabeceras ("Hoy", "7 días", "v2") | Neutro |
| `.pill-live` | El acento de la vista. **Uno por pantalla.** | `--brand` |
| `.chip` | Estado de un registro (aprobado, pendiente, rechazado) | Siempre neutro |
| `.badge` | Contador numérico sobre un icono | Neutro, o `--brand` si es el único |

## Pill de cristal

```css
.pill {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 3px 11px; border-radius: 999px;
  background: linear-gradient(165deg, rgba(255,255,255,0.10), rgba(255,255,255,0.03));
  border: 1px solid rgba(255,255,255,0.14);
  color: var(--text-secondary);
  font: 700 10px/1 Inter, sans-serif; letter-spacing: .02em;
  backdrop-filter: blur(8px); white-space: nowrap;
}
[data-theme="light"] .pill {
  background: linear-gradient(165deg, rgba(255,255,255,0.9), rgba(255,255,255,0.55));
  border-color: rgba(15,20,30,0.12);
}
```

## Pill de acento — el punto que late

El elemento con más personalidad del sistema. **Uno por vista, ni uno más.**

```html
<span class="pill-live"><span class="d"></span> Semana activa</span>
```
```css
.pill-live {
  display: inline-flex; align-items: center; gap: 7px;
  padding: 6px 13px; border-radius: 999px;
  border: 1px solid var(--brand-border); background: var(--brand-soft); color: var(--brand);
  font: 700 11px Inter, sans-serif; letter-spacing: .09em; text-transform: uppercase;
}
.pill-live .d {
  width: 6px; height: 6px; border-radius: 50%; background: var(--brand);
  box-shadow: 0 0 10px rgba(var(--brand-rgb),.7);
  animation: pulse 2.4s ease-in-out infinite;
}
```

Usos en la app de clipping: "SEMANA ACTIVA" en rankings, "EN DIRECTO" cuando el streamer
está emitiendo, "BOTE 400 €" en la cabecera de incentivos. Una sola por pantalla.

## Chip de estado

**Siempre neutro.** El estado se lee por el texto, no por el color.

```css
.chip {
  display: inline-flex; align-items: center; gap: 4px;
  padding: 3px 9px; border-radius: 999px;
  background: var(--chip-bg); color: var(--chip-text); border: 1px solid var(--chip-border);
  font: 600 10.5px Inter, sans-serif; letter-spacing: .3px; white-space: nowrap;
}
.chip.off { border-style: dashed; color: var(--text-muted); }   /* pendiente / inactivo */
```

Estados de un clip: `Pendiente` (`.chip.off`) · `En revisión` · `Aprobado` · `Rechazado` ·
`Pagado`. Los cinco con el mismo gris. La única señal cromática está en el **borde
izquierdo** de la fila (ver `tablas-listas.md`).

## Kicker con línea

Encima de cada titular de página o bloque.

```css
.kick {
  display: inline-flex; align-items: center; gap: 10px;
  font: 700 10.5px Inter, sans-serif; letter-spacing: .16em;
  text-transform: uppercase; color: var(--text-muted);
}
.kick::before { content:''; width: 22px; height: 1px; background: currentColor; opacity: .6; }
```

## Tag

Para etiquetas libres (plataforma, categoría, campaña):

```css
.tag {
  display: inline-block; padding: 2px 8px; border-radius: 999px;
  background: var(--bg-surface-2); color: var(--text-secondary); border: 1px solid var(--border);
  font: 600 10px/1.4 Inter, sans-serif; text-transform: uppercase; letter-spacing: .3px;
}
```
