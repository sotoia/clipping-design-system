# 06 · Texturas de marca (recetas copy-paste)

Las cuatro texturas que hacen que una pantalla "parezca del producto":
**puntos**, **líneas discontinuas**, **cristal** y **orbes**.

---

## 6.1 · Fondo de puntos

Un único patrón, cuatro densidades según el tamaño del contenedor:

```css
.dots    { background-image: radial-gradient(var(--dots) 1px, transparent 1px); background-size: 14px 14px; }
.dots-sm { background-size: 9px 9px;  background-position: -1px -1px; }  /* cajas pequeñas */
.dots-md { background-size: 12px 12px; background-position: -1px -1px; } /* marcos */
.dots-lg { background-size: 16px 16px; }                                 /* lienzos grandes */
```

El `background-position: -1px -1px` no es decorativo: alinea el primer punto con el borde
interior de la caja y evita la media-luna cortada de la esquina.

**Dónde:** lienzos de trabajo (`.main` de analítica, dashboard, ajustes), marcos que engloban
cajas, columnas de kanban, cajas de resumen.
**Dónde NO:** dentro de una card de KPI (los puntos sangran bajo el número y ensucian).
Las cards que viven sobre un marco con puntos deben tener fondo **opaco**.

### Retícula de auth (login / registro)

Más separada (28 px) y desvanecida hacia los bordes con una máscara elíptica:

```css
.dots-auth {
  background-image: radial-gradient(circle, rgba(255,255,255,0.07) 1px, transparent 1px);
  background-size: 28px 28px;
  mask-image: radial-gradient(ellipse at center, #000 30%, transparent 80%);
  -webkit-mask-image: radial-gradient(ellipse at center, #000 30%, transparent 80%);
}
[data-theme="light"] .dots-auth { background-image: radial-gradient(circle, rgba(15,23,42,0.10) 1px, transparent 1px); }
```

### Puntos sobre foto / miniatura

Concentrados en una esquina, con deriva lenta:

```css
.photo::before {
  content:''; position:absolute; inset:0; pointer-events:none;
  background-image: radial-gradient(rgba(255,255,255,0.5) 1px, transparent 1.7px);
  background-size: 14px 14px;
  mask-image: radial-gradient(150% 135% at 0% 100%, #000 22%, rgba(0,0,0,.6) 50%, transparent 78%);
  animation: drift 70s linear infinite;
}
@keyframes drift { to { background-position: 14px 14px; } }
```

---

## 6.2 · Líneas discontinuas

**Significado fijo: "esto es secundario, extra, o una zona que se rellena".**
Si algo es principal, va sólido. Si el borde es discontinuo, el usuario debe poder
ignorarlo sin perder nada crítico.

```css
/* Sub-contenedor dentro de una card */
.sub { border: 1px dashed var(--border-2); border-radius: 12px;
       background: rgba(255,255,255,0.015); padding: 12px 14px; }
[data-theme="light"] .sub { background: rgba(15,20,30,0.02); }

/* Caja discontinua CON puntitos dentro (resumen IA, marco de configuración) */
.frame-dashed {
  border: 1px dashed var(--border-2); border-radius: 10px; padding: 8px 12px;
  background-color: rgba(255,255,255,0.035);
  background-image: radial-gradient(var(--dots) 1px, transparent 1px);
  background-size: 12px 12px;
}

/* Zona de drop / lienzo de grupo: 1.5px, radius 16 */
.group { border: 1.5px dashed var(--border-2); border-radius: 16px; background: rgba(128,128,128,0.045); }

/* Textarea borrador: dashed en reposo, sólido al foco */
textarea.draft { border: 1px dashed var(--border-2); }
textarea.draft:focus { border-style: solid; }

/* Chip apagado */
.chip.off { border-style: dashed; color: var(--text-muted); }

/* Separador de pestañas */
.tabs-strip { border-bottom: 1px dashed var(--border-2); }
```

### Marching ants (borde discontinuo ANIMADO)

Para el tile "añadir módulo", zonas de drop activas y estados vacíos.
Overlay SVG absoluto; el contenedor debe ser `position: relative` y el SVG, primer hijo.

```html
<div style="position:relative">
  <svg class="ants" aria-hidden="true"><rect style="rx:14"/></svg>
  …contenido…
</div>
```
```css
.ants { position:absolute; inset:0; width:100%; height:100%; pointer-events:none; z-index:0; overflow:visible; }
.ants rect {
  x:1px; y:1px; width:calc(100% - 2px); height:calc(100% - 2px);
  fill:none; stroke:var(--border-2); stroke-width:1.5; stroke-dasharray:5 7;
  animation: ants-move 22s linear infinite;
}
@keyframes ants-move { to { stroke-dashoffset: -240; } }
@media (prefers-reduced-motion: reduce) { .ants rect { animation: none; } }
```

22 segundos por vuelta: lo bastante lento como para que no distraiga y lo bastante
vivo como para que se note que es una zona interactiva. El contenedor lleva
`border-color: transparent` — el borde lo pone el SVG.

---

## 6.3 · Cajas que engloban cajas

El patrón más característico del panel: un **marco opaco con puntitos** que contiene
cards **opacas**. Da jerarquía sin usar color ni líneas gruesas.

```css
.frame {
  position: relative;
  border: 1px solid var(--border);
  border-radius: 18px;
  padding: 16px 16px 18px;
  background:
    radial-gradient(var(--dots) 1px, transparent 1px) -1px -1px / 9px 9px,
    var(--frame-bg);                 /* #1c1c1e oscuro · #f4f2ee claro */
  box-shadow: var(--glass-shadow);
}
```

Las cajas de dentro:

```css
.frame > .card {
  background: var(--frame-bg);       /* OPACO — si no, sangran los puntos */
  border: 1px solid var(--border);
  border-radius: 14px;
  transition: transform .18s cubic-bezier(.22,.8,.28,1), border-color .18s, box-shadow .18s;
}
@media (hover: hover) {
  .frame > .card:hover {
    transform: translateY(-3px);
    border-color: var(--hover-border);
    box-shadow: 0 14px 32px rgba(0,0,0,0.32);
  }
}
[data-theme="light"] .frame > .card:hover { box-shadow: 0 14px 32px rgba(60,50,35,0.12); }
```

**Tres niveles máximo:** lienzo con puntos → marco con puntos → card opaca.
Un cuarto nivel se lee como ruido.

---

## 6.4 · Cristal

```css
.glass {
  position: relative; overflow: hidden;
  background: var(--glass-grad);
  backdrop-filter: blur(14px) saturate(120%);
  -webkit-backdrop-filter: blur(14px) saturate(120%);
  border: 1px solid var(--glass-border);
  border-radius: 16px;
  box-shadow: var(--glass-shadow);
  transition: transform .22s cubic-bezier(.22,.8,.28,1), border-color .22s, box-shadow .22s;
}
/* FIRMA OBLIGATORIA: brillo en dos esquinas diagonalmente opuestas */
.glass::before {
  content:''; position:absolute; inset:0; border-radius:inherit; pointer-events:none;
  background: var(--sheen); z-index: 0;
}
.glass > * { position: relative; z-index: 1; }
@media (hover:hover) {
  .glass:hover { transform: translateY(-2px); border-color: var(--hover-border); box-shadow: var(--hover-shadow); }
}
```

El `::before` con el sheen es lo que diferencia el cristal de la casa de un
glassmorphism genérico. Sin él, la tarjeta es un rectángulo translúcido cualquiera.

**Modales: cristal OPACO.** Un modal con contenido crítico nunca es translúcido.

```css
.modal   { background: rgba(24,24,26,0.90); backdrop-filter: blur(22px) saturate(140%);
           border: 1px solid var(--border-2); border-radius: 16px; box-shadow: var(--glass-shadow); }
.overlay { background: var(--overlay); backdrop-filter: blur(6px); }
[data-theme="light"] .modal { background: rgba(255,255,255,0.92); }
```

**Prohibido** `feTurbulence` / `fractalNoise` en cualquier backdrop: posteriza y mancha.

---

## 6.5 · Orbes ambientales

Dos o tres círculos enormes, difuminados y casi invisibles, que se mueven lentísimo.
Dan la sensación de que el fondo está vivo sin que nadie sepa por qué.

```css
.amb   { position: absolute; inset: 0; overflow: hidden; pointer-events: none; }
.amb i { position: absolute; display: block; border-radius: 50%; will-change: transform; }
.amb .o1 { top:-22%; left:-8%;     width:55vmax; height:55vmax; filter: blur(70px);
           background: radial-gradient(circle, rgba(var(--brand-rgb),0.07), transparent 62%);
           animation: float-a 24s ease-in-out infinite alternate; }
.amb .o2 { bottom:-25%; right:-10%; width:50vmax; height:50vmax; filter: blur(80px);
           background: radial-gradient(circle, rgba(var(--brand-rgb),0.045), transparent 62%);
           animation: float-b 32s ease-in-out infinite alternate; }
.amb .o3 { top:30%; right:20%;      width:30vmax; height:30vmax; filter: blur(60px);
           background: radial-gradient(circle, rgba(255,255,255,0.04), transparent 60%);
           animation: float-c 28s ease-in-out infinite alternate; }
@keyframes float-a { to { transform: translate( 7vmax,  6vmax) scale(1.12); } }
@keyframes float-b { to { transform: translate(-6vmax, -7vmax) scale(0.92); } }
@keyframes float-c { to { transform: translate(-8vmax,  5vmax) scale(1.08); } }
```

Duraciones **distintas y primas entre sí** (24 / 32 / 28 s) para que nunca se sincronicen.
El contenido va en `z-index: 1`.

En las pantallas de auth los dos primeros orbes llevan tinte de marca — y eso
**cuenta como el acento de la vista**.

## 6.6 · Glows del lienzo

Fijos, sin animación, en `body`. No confundir con los orbes.

```css
body {
  background-color: var(--bg-base);
  background-image:
    radial-gradient(620px 440px at 12% -6%,   rgba(255,255,255,0.045), transparent 70%),
    radial-gradient(520px 400px at 100% 106%, rgba(255,255,255,0.035), transparent 70%);
  background-attachment: fixed;
}
[data-theme="light"] body {
  background-image:
    radial-gradient(620px 440px at 12% -6%,   rgba(120,105,70,0.07), transparent 70%),
    radial-gradient(520px 400px at 100% 106%, rgba(120,105,70,0.06), transparent 70%);
}
```
