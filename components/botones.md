# Botones

## Jerarquía

| Nivel | Clase | Aspecto |
|---|---|---|
| Primario | `.btn-primary` | **Invertido**: claro sobre oscuro / oscuro sobre claro |
| Secundario | `.btn-secondary` | Fondo `--accent-muted`, borde `--border-2` |
| Fantasma | `.btn-ghost` | Transparente, borde `--border`, texto secundario |
| Destructivo | `.btn-danger` | Texto `--c-red2`, borde rojo al 25 %, **sin fondo rojo** |
| Icono suelto | `.btn-icon` | 24×24, icono 12–14 px, sin borde en reposo |

El primario **nunca** lleva el color de marca en la app. El color de marca en un botón
se reserva a login/registro y a la CTA de la web pública.

```css
.btn-primary {
  background: var(--btn-primary-bg);   /* #f0f2f5 oscuro · #16181d claro */
  color: var(--btn-primary-text);      /* #0b0d10 oscuro · #f4f6f8 claro */
  border: 1px solid var(--btn-primary-bg);
  padding: 9px 16px; border-radius: 8px;
  font: 600 13px Inter, sans-serif; cursor: pointer;
  display: inline-flex; align-items: center; gap: 6px;
  transition: filter .15s ease, transform .15s ease;
}
.btn-primary:hover:not(:disabled) { filter: brightness(1.06); transform: translateY(-1px); }
.btn-primary:active:not(:disabled) { transform: translateY(0) scale(.99); }
.btn-primary:disabled { opacity: .45; cursor: not-allowed; transform: none; }

.btn-secondary {
  background: var(--accent-muted); border: 1px solid var(--border-2); color: var(--text-primary);
  padding: 9px 14px; border-radius: 8px; font: 500 13px Inter, sans-serif;
}
.btn-secondary:hover:not(:disabled) { background: var(--bg-hover); transform: translateY(-1px); }

.btn-ghost { background: transparent; border: 1px solid var(--border); color: var(--text-secondary); }
.btn-ghost:hover { color: var(--text-primary); background: var(--bg-hover); }

.btn-danger { background: transparent; border: 1px solid rgba(var(--c-red-rgb),.25); color: var(--c-red2); }
.btn-danger:hover { background: rgba(var(--c-red-rgb),.10); }

.btn-icon {
  width: 24px; height: 24px; padding: 0; border: none; background: transparent;
  color: var(--text-muted); border-radius: 6px; cursor: pointer;
  display: inline-flex; align-items: center; justify-content: center;
  transition: color .15s ease, background .15s ease;
}
.btn-icon:hover { color: var(--text-primary); background: var(--bg-hover); }
```

## CTA de marca (solo auth y web)

```css
.btn-brand {
  position: relative; overflow: hidden;
  width: 100%; padding: 12px; border: none; border-radius: 11px;
  background: linear-gradient(135deg, var(--brand), var(--brand-2));
  color: #fff; font: 600 14px Inter, sans-serif; cursor: pointer;
  display: flex; align-items: center; justify-content: center; gap: 9px;
  box-shadow: var(--brand-glow);
  transition: transform .2s ease, box-shadow .25s ease, filter .2s ease;
}
.btn-brand::after {          /* barrido de brillo */
  content:''; position:absolute; top:0; left:-80%; width:60%; height:100%;
  transform: skewX(-18deg);
  background: linear-gradient(100deg, transparent, rgba(255,255,255,.22), transparent);
  transition: left .5s ease;
}
.btn-brand:hover:not(:disabled) { transform: translateY(-1px); filter: brightness(1.05);
                                  box-shadow: 0 10px 30px rgba(var(--brand-rgb),.4); }
.btn-brand:hover:not(:disabled)::after { left: 120%; }
.btn-brand:active:not(:disabled) { transform: translateY(0) scale(.99); }
.btn-brand:disabled { opacity: .65; cursor: not-allowed; box-shadow: none; }
```

## Botón social (Google)

Único sitio donde un logo conserva su color, porque es marca de terceros y la
normativa de Google lo exige.

```css
.btn-social {
  width: 100%; padding: 11px 14px; border-radius: 11px;
  border: 1px solid var(--border-2); background: var(--bg-surface);
  color: var(--text-primary); font: 500 14px Inter, sans-serif; cursor: pointer;
  display: flex; align-items: center; justify-content: center; gap: 10px;
  transition: background .2s, transform .2s, border-color .2s, box-shadow .25s;
}
.btn-social:hover:not(:disabled) {
  background: var(--bg-surface-2); border-color: var(--hover-border);
  transform: translateY(-1px); box-shadow: 0 10px 26px rgba(0,0,0,0.35);
}
[data-theme="light"] .btn-social { background: #fff; }
[data-theme="light"] .btn-social:hover:not(:disabled) { background: #f8fafc; box-shadow: 0 10px 26px rgba(15,23,42,.10); }
```

## Estado de carga

Spinner de 14 px, borde de 2 px, dentro del botón, a la izquierda del texto.
El texto cambia ("Subiendo clip…"), el botón no cambia de tamaño.

```css
.spin { width:14px; height:14px; border-radius:999px;
        border:2px solid rgba(255,255,255,.35); border-top-color:#fff;
        animation: rotate .7s linear infinite; }
```
