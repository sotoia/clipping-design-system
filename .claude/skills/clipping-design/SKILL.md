---
name: clipping-design
description: Sistema de diseño de la app de clipping (panel del clipper, panel del creador, rankings, incentivos, web). Úsalo SIEMPRE antes de crear o tocar cualquier pantalla, componente o página que deba parecer de este producto — cristal monocromo sobre negro neutro, fondos de puntos, cajas de líneas discontinuas, cajas que engloban cajas, un solo acento por vista, Inter, iconos de trazo fino, cero emojis.
---

# Sistema de diseño — clipping

Extraído del panel de producción de Wismify CX (skin mono) y adaptado.
La documentación completa vive en este repositorio:

- `tokens/tokens.css` — **la fuente de verdad**. Cópialo tal cual al proyecto.
- `foundations/` — filosofía, color, tipografía, layout, iconos, texturas, movimiento
- `components/` — botones, pills, cajas, inputs, KPIs, tablas, navegación, modales
- `pages/` — una ficha por pantalla, empezando por login y registro
- `preview/index.html` — referencia viva con toggle de tema y de modo de panel

## Lo mínimo que hay que saber

1. **Monocromo neutro.** Grises con R≈G≈B: `rgba(255,255,255,α)` sobre oscuro,
   `rgba(15,20,30,α)` sobre claro. Nunca `#94a3b8` ni `#64748b`.
2. **Un acento por vista.** `--brand` en UN elemento. Nunca en checks, hovers,
   precios, iconos de menú ni botones primarios.
3. **Primario invertido.** Blanco sobre oscuro / negro sobre claro.
4. **Cristal con sheen en dos esquinas opuestas.** Es la firma; sin él no es el sistema.
5. **Discontinuo = secundario.** Sólido = principal.
6. **Tres niveles de caja:** lienzo con puntos → marco opaco con puntos → card opaca.
7. **Sin emojis. Sin iconos de color.** SVG de trazo fino con `currentColor`.
8. **`tabular-nums`** en toda cifra.
9. **Dos temas diseñados.** El claro no es una inversión: lienzo blanco puro,
   `#f4f2ee` para cajas-marco, sombras cálidas `rgba(60,50,35,α)`.
10. **Dos modos de panel:** `data-shell="classic"` (sidebar 260px) y
    `data-shell="modern"` (dock flotante estilo macOS).

Antes de escribir código, lee `foundations/01-filosofia.md` y la ficha de la
página correspondiente en `pages/`.
