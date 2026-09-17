# Cajas: cristal, marcos y anidamiento

Cuatro tipos de contenedor. La jerarquía se lee por **textura**, no por color.

```
┌─ lienzo .main.dots ─────────────────────────────────────┐
│  fondo de puntos 14px                                    │
│                                                          │
│   ┌─ .frame · marco opaco + puntitos 9px ────────────┐   │
│   │                                                   │   │
│   │   ┌─ .card OPACA ──┐  ┌─ .card OPACA ──┐          │   │
│   │   │  KPI           │  │  KPI           │          │   │
│   │   │  ┌ .sub dashed┐│  │                │          │   │
│   │   │  └────────────┘│  │                │          │   │
│   │   └────────────────┘  └────────────────┘          │   │
│   └───────────────────────────────────────────────────┘   │
│                                                          │
│   ┌─ .glass · cristal con sheen ──────────────────────┐   │
│   └───────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────┘
```

## 1 · `.glass` — el contenedor de marca

Translúcido, con el sheen en dos esquinas opuestas y lift al hover.
Para cards que **flotan sobre el lienzo**: tarjeta de auth, paneles de resumen,
tarjeta de perfil, módulos del dashboard.

Receta completa en `foundations/06-texturas.md` §6.4.

## 2 · `.frame` — la caja que engloba cajas

Opaca, con puntitos dentro, sombra de cristal. Agrupa un conjunto de cards que
pertenecen a la misma idea (la fila de KPIs, los favoritos, una columna de kanban).

```css
.frame {
  position: relative;
  border: 1px solid var(--border); border-radius: 18px; padding: 16px 16px 18px;
  background: radial-gradient(var(--dots) 1px, transparent 1px) -1px -1px / 9px 9px, var(--frame-bg);
  box-shadow: var(--glass-shadow);
}
```

**Regla dura:** las cards de dentro llevan fondo **opaco** (`var(--frame-bg)`),
no translúcido. Si no, los puntos del marco se ven a través del número del KPI.

## 3 · `.card` — la caja de contenido

Sobre el lienzo, va de cristal. Dentro de un marco, va opaca.

```css
.card {
  background: var(--bg-card); border: 1px solid var(--border);
  border-radius: 16px; padding: 18px 20px;
  transition: transform .22s cubic-bezier(.22,.8,.28,1), border-color .22s, box-shadow .22s;
}
.card-header {
  display: flex; align-items: center; gap: 8px;
  padding-bottom: 12px; margin-bottom: 12px;
  border-bottom: 1px solid var(--border);
  font: 700 12.5px Inter, sans-serif; color: var(--text-primary);
}
.card-desc { font-size: 11.5px; color: var(--text-secondary); }
```

Cabecera canónica: icono en cuadrado 32 px + título 12.5 px 700 + `.pill` a la derecha.

## 4 · `.sub` — la zona secundaria discontinua

Dentro de una card, para lo que es extra: una nota, un resumen, un desglose,
una zona que el usuario puede rellenar.

```css
.sub { border: 1px dashed var(--border-2); border-radius: 12px;
       background: rgba(255,255,255,0.015); padding: 12px 14px; }
[data-theme="light"] .sub { background: rgba(15,20,30,0.02); }
```

Variante con puntitos dentro (`.frame-dashed`) para resúmenes generados por IA
y marcos de configuración. Ver `foundations/06-texturas.md` §6.2.

## Profundidad: nunca más de tres niveles

`lienzo con puntos` → `marco con puntos` → `card opaca` → (`.sub` dashed).
Un cuarto contenedor anidado se lee como ruido: sácalo a un modal o a una pestaña.

## Estado vacío

Icono de 15 px en cuadrado + una frase de 12 px `--text-muted`, dentro de un `.sub`
o de un contenedor con **marching ants**. Sin ilustración, sin emoji.

```html
<div class="sub" style="text-align:center;padding:28px 20px">
  <span class="ic"><svg …/></span>
  <p style="margin-top:10px;font-size:12px;color:var(--text-muted)">Todavía no has subido ningún clip.</p>
  <button class="btn-primary" style="margin-top:14px">Subir el primero</button>
</div>
```

## Skeleton de carga

Bloques `rgba(255,255,255,0.06)` con shimmer, del tamaño exacto del contenido final.
Nunca un spinner a pantalla completa.

```css
.skel { background: linear-gradient(90deg, var(--bg-surface) 25%, var(--bg-surface-2) 37%, var(--bg-surface) 63%);
        background-size: 400% 100%; animation: shimmer 1.4s ease infinite; border-radius: 8px; }
@keyframes shimmer { 0% { background-position: 100% 50% } 100% { background-position: 0 50% } }
```
