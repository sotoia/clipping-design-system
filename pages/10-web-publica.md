# 10 · Web pública

La landing donde el streamer capta clippers. Más negra y más grande que la app.

## Tokens propios

```css
:root {
  --bg-deep: #030405; --bg: #050608; --bg-soft: #07090c;
  --glass-strong: rgba(8,10,14,0.85); --glass-soft: rgba(12,15,20,0.55);
  --line-soft: rgba(255,255,255,0.05); --line-strong: rgba(255,255,255,0.08);
  --text: #f5f7fa; --muted: #8b9098;
}
html { background: var(--bg-deep); color-scheme: dark; }   /* NUNCA quitar: evita la franja clara al rebotar el scroll */
```

## Navbar flotante

```css
.nav {
  position: fixed; top: 24px; left: 50%; transform: translateX(-50%);
  width: calc(100% - 48px); max-width: 1280px; height: 56px;
  display: flex; align-items: center; gap: 20px; padding: 0 18px;
  border-radius: 14px; border: 1px solid var(--line-soft);
  background: linear-gradient(165deg, rgba(255,255,255,0.055), rgba(255,255,255,0.02));
  backdrop-filter: blur(18px);
  box-shadow: 0 10px 30px rgba(0,0,0,.55), inset 0 1px 0 rgba(255,255,255,.08);
  z-index: 100;
}
.nav a { font-size: 13px; color: var(--muted); }
.nav a:hover { color: var(--text); }
.nav .cta { padding: 9px 18px; border-radius: 999px; background: var(--brand); color: #0b0d10; font-weight: 600; }
```

El CTA de la navbar es **el único verde de toda la página**.

## Hero

Puntos en toda la sección + desvanecido arriba y abajo:

```css
.hero {
  background:
    radial-gradient(rgba(255,255,255,0.11) 1.1px, transparent 1.5px) 0 0 / 16px 16px,
    radial-gradient(ellipse at top, #0a0c10 0%, #030405 70%);
}
.hero::before {
  content:''; position:absolute; inset:0; pointer-events:none;
  background: linear-gradient(180deg,#030405 0%, rgba(3,4,5,0) 26%),
              linear-gradient(0deg,  #030405 0%, rgba(3,4,5,0) 22%);
}
```

- H1 `clamp(28px, 3.8vw, 46px)` peso 900, línea 1.08, con degradado
  `#fff 55% → rgba(255,255,255,.55)` y `background-clip: text`.
- CTA primaria: pill **blanca** con texto negro, con barrido de brillo.
- Debajo, banda de estadísticas: 3–4 celdas separadas por `gap: 1px` sobre el color
  de línea, fondo `rgba(6,7,9,0.86)`, número `clamp(32px, 4.4vw, 50px)` peso 800.

## Secciones

```css
.section       { border-top: 1px dashed rgba(255,255,255,0.10);
                 border-bottom: 1px dashed rgba(255,255,255,0.10);
                 background: #08080b; padding: 100px 0; }
.section-inner { max-width: 1120px; margin: 0 auto; padding: 0 28px;
                 border-left: 1px dashed rgba(255,255,255,0.09);
                 border-right: 1px dashed rgba(255,255,255,0.09); }
```

Las **guías punteadas laterales** alineadas con la columna de contenido son la firma
de la web. Separación entre secciones: 100–120 px.

## Marco técnico sobre imagen

Guías verticales discontinuas, marcas "+" en las esquinas y metadatos en mayúsculas.
Se usa sobre capturas del panel y sobre miniaturas de clips destacados.

```css
.frame-tech i  { position:absolute; top:0; bottom:0; border-left: 1px dashed rgba(255,255,255,0.16); }
.frame-tech b  { position:absolute; width:13px; height:13px; }
.frame-tech b::before { content:''; position:absolute; left:0; right:0; top:6px; height:1px; background:rgba(255,255,255,.6); }
.frame-tech b::after  { content:''; position:absolute; top:0; bottom:0; left:6px; width:1px; background:rgba(255,255,255,.6); }
```

## Movimiento

- Intro con SplitText por líneas, `expo.out`, stagger .014–.05.
- Reveal con IntersectionObserver o `ScrollTrigger.batch` — `overwrite: 'auto'`, **nunca `true`**.
- Parallax de fotos por `background-position`, **nunca** `transform`: con scroll suave
  asoma 1 px del borde.
- Marquee de plataformas: track duplicado ×2, `translateX(-50%)` lineal infinito,
  pausa al hover, máscara de fundido lateral. Logos monocromos en blanco.

## Secciones de la landing

1. Hero — "Convierte sus directos en clips. Y los clips en dinero."
2. Banda de estadísticas — clips pagados, € repartidos, clippers activos
3. Cómo funciona — 3 pasos, cards de cristal con número en cuadrado
4. Incentivos — los botes de la semana, misma card que en la app
5. Ranking público — top 10 en vivo (el mejor gancho)
6. Reglas — acordeón con `grid-template-rows: 0fr → 1fr`
7. FAQ
8. CTA final + footer con degradado `.03 → .01`, `border-top .08`, blur 12
