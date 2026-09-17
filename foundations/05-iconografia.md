# 05 · Iconografía

**Sin emojis. Sin iconos de color. Sin ilustraciones 3D.** Nunca, en ningún sitio:
ni en la UI, ni en estados vacíos, ni en notificaciones, ni en emails.

## Sistema

- **App (React):** `lucide-react`.
- **Web y documentos:** SVG inline a mano, estilo Feather.

```html
<svg viewBox="0 0 24 24" width="16" height="16" fill="none" stroke="currentColor"
     stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">…</svg>
```

`stroke="currentColor"` siempre. Nunca `fill` de color. Si un SVG de terceros trae `fill`,
se fuerza `fill: currentColor`.

## Tamaños

| Contexto | Tamaño | Grosor |
|---|---|---|
| Dentro de chips y listas | 13–14 px | 2 |
| Navegación, botones | 15–16 px | 2 |
| Cabecera de sección | 18–20 px | 1.6–1.8 |
| Dock (modo modern) | 22 px | 1.8 |
| Botón de icono suelto | 12 px en caja 24×24 | 2 |

## Icono en cuadrado (cabecera de tarjeta)

El patrón de la casa: icono de 15–16 px centrado en un cuadrado de 30–32 px.

```css
.ic {
  width: 32px; height: 32px; border-radius: 10px;
  display: inline-flex; align-items: center; justify-content: center;
  border: 1px solid var(--border-2);
  background: var(--bg-surface);
  color: var(--text-primary);
}
.ic svg { width: 15px; height: 15px; opacity: .7; }
```

Variante circular para modales: `border-radius: 50%`, `padding: 7px`, fondo `--bg-surface-2`.

## Trazos de referencia (copiar tal cual)

```html
<!-- Play / clip -->
<path d="M5 3.5v17l14-8.5z"/>
<!-- Subir -->
<path d="M12 19V5"/><path d="m5 12 7-7 7 7"/>
<!-- Trofeo / ranking -->
<path d="M8 21h8M12 17v4M6 4h12v5a6 6 0 0 1-12 0z"/><path d="M6 6H4a2 2 0 0 0 0 4h2"/><path d="M18 6h2a2 2 0 0 1 0 4h-2"/>
<!-- Vistas -->
<path d="M2 12s3.5-7 10-7 10 7 10 7-3.5 7-10 7-10-7-10-7z"/><circle cx="12" cy="12" r="3"/>
<!-- Tendencia -->
<path d="m3 17 6-6 4 4 8-8"/><path d="M17 7h4v4"/>
<!-- Euro / pago -->
<path d="M4 10h10M4 14h9"/><path d="M18.5 5.5A7.5 7.5 0 0 0 7 12a7.5 7.5 0 0 0 11.5 6.5"/>
<!-- Check en círculo -->
<circle cx="12" cy="12" r="9"/><path d="M8 12.5l2.6 2.6L16.5 9"/>
<!-- Ajustes -->
<circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 1 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 1 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 1 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.6 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 1 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 1 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.6a1.65 1.65 0 0 0 1-1.51V3a2 2 0 1 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 1 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9c.14.55.6.96 1.17 1H21a2 2 0 1 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/>
```

## Marcas de plataformas

Monocromas, en blanco (`fill="currentColor"`), nunca con su color corporativo en la UI.
El color de TikTok / YouTube / Instagram solo vive en las **series de las gráficas**
(ver `02-color.md`).

## Chevrons y checks

En CSS, con bordes rotados — nunca caracteres tipográficos ni SVG dentro de texto traducible.

```css
.chev { width: 6px; height: 6px; border-right: 1.5px solid currentColor; border-bottom: 1.5px solid currentColor;
        transform: rotate(45deg); transition: transform .25s ease; }
.open .chev { transform: rotate(225deg); }
```
