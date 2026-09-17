# Clipping — Sistema de diseño

Sistema de diseño para la app de **clipping de comunidad**: panel del clipper,
panel del creador, rankings, incentivos y web pública.

Está **extraído del código de producción del panel de Wismify CX** (`cx.wismify.com`,
skin mono) el 17-sep-2026 y reetiquetado para este producto. Todo lo que hay aquí son
valores reales que están funcionando, no propuestas.

## Qué es esto

Cristal monocromo sobre negro neutro. Fondos de puntos. Cajas de líneas discontinuas.
Cajas que engloban cajas. Un único acento de color por pantalla. Inter. Iconos de trazo
fino. **Cero emojis.**

```
Lienzo con puntos  →  marco opaco con puntos  →  card opaca  →  sub discontinua
```

## Cómo se usa

1. Copia `tokens/tokens.css` al proyecto y ponlo el primero de todo.
2. `<html data-theme="dark" data-shell="classic">` — aplica ambos antes del primer
   pintado con el script inline de `pages/00-shell-y-modos.md`.
3. Abre `preview/index.html` en el navegador: es la referencia viva. Cambia de tema
   y de modo de panel desde la topbar.
4. Antes de crear cualquier pantalla, lee `foundations/01-filosofia.md` y la ficha
   de la página en `pages/`.

## Índice

### Fundamentos
| | |
|---|---|
| [`01-filosofia.md`](foundations/01-filosofia.md) | Las 10 reglas y las prohibiciones |
| [`02-color.md`](foundations/02-color.md) | Neutros, marca y colores de datos |
| [`03-tipografia.md`](foundations/03-tipografia.md) | Inter, escala y degradado de tinta |
| [`04-layout.md`](foundations/04-layout.md) | Shell, rejillas, radios, sombras |
| [`05-iconografia.md`](foundations/05-iconografia.md) | SVG de trazo fino, sin emojis |
| [`06-texturas.md`](foundations/06-texturas.md) | Puntos, dashed, cristal, orbes, ants |
| [`07-movimiento.md`](foundations/07-movimiento.md) | Duraciones, curvas, escalonado |

### Componentes
| | |
|---|---|
| [`botones.md`](components/botones.md) | Primario invertido, marca, social |
| [`pills-chips.md`](components/pills-chips.md) | Pill de cristal, pill que late, chips |
| [`cajas.md`](components/cajas.md) | Cristal, marco, card, sub, vacíos |
| [`inputs.md`](components/inputs.md) | Campos, contraseña, select, drop |
| [`kpi-y-graficas.md`](components/kpi-y-graficas.md) | Anatomía del KPI y paleta de series |
| [`tablas-listas.md`](components/tablas-listas.md) | Tablas, filas, ranking, grid de clips |
| [`navegacion.md`](components/navegacion.md) | **Sidebar y dock** — los dos modos |
| [`modales-y-feedback.md`](components/modales-y-feedback.md) | Modales, toasts, tooltips |

### Páginas
| | |
|---|---|
| [`00-shell-y-modos.md`](pages/00-shell-y-modos.md) | Modo clásico vs modo modern |
| [`01-login.md`](pages/01-login.md) | ⭐ Extraída 1:1 de `cx.wismify.com/login` |
| [`02-register.md`](pages/02-register.md) | ⭐ Extraída 1:1 de `cx.wismify.com/register` |
| [`03-dashboard-clipper.md`](pages/03-dashboard-clipper.md) | |
| [`04-dashboard-creador.md`](pages/04-dashboard-creador.md) | |
| [`05-analitica.md`](pages/05-analitica.md) | |
| [`06-ranking.md`](pages/06-ranking.md) | |
| [`07-incentivos.md`](pages/07-incentivos.md) | |
| [`08-clips.md`](pages/08-clips.md) | Subir, listar, revisar |
| [`09-ajustes.md`](pages/09-ajustes.md) | Incluye el selector de modo de panel |
| [`10-web-publica.md`](pages/10-web-publica.md) | |

### Preview
- [`preview/index.html`](preview/index.html) — la galería completa, con toggle de tema y de shell
- [`preview/login.html`](preview/login.html) — login funcionando, con spotlight y tilt
- [`preview/register.html`](preview/register.html) — registro por invitación

## Los dos modos de panel

Los dos existen para clipper y para creador; se eligen en Ajustes → Apariencia.

- **Clásico** — sidebar de 260 px, sticky, con blur. Denso.
- **Modern** — dock flotante abajo estilo macOS: cristal con sheen, magnificación al
  hover, punto de pestaña abierta, etiqueta emergente. Máximo 7 iconos.

## Lo que hay que respetar sí o sí

1. Un solo elemento con el color de marca por pantalla.
2. Grises **neutros** (R≈G≈B). Nunca `#94a3b8` ni `#64748b` en la UI.
3. Botón primario **invertido**, nunca de color.
4. El cristal lleva el sheen en **dos esquinas diagonalmente opuestas**.
5. Discontinuo = secundario. Sólido = principal.
6. Sin emojis, sin iconos de color.
7. `tabular-nums` en toda cifra.
8. El tema claro se diseña, no se invierte.

Detalle completo en [`foundations/01-filosofia.md`](foundations/01-filosofia.md).

## Cambiar el color de marca

Una línea en `tokens/tokens.css`:

```css
--brand: #10B981;  --brand-2: #059669;  --brand-rgb: 16,185,129;
```

El resto del sistema es monocromo, así que cambiar estas tres variables cambia la
personalidad de la app entera sin tocar un componente.

---

Extraído de `cx.wismify.com` · 17-sep-2026
