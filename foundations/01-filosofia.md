# 01 · Filosofía

Diez reglas. No son estilo, son estructura: si se rompe una, la pantalla deja de parecer del mismo producto.

1. **Monocromo casi total.** Blanco/gris sobre negro neutro, o negro/gris sobre blanco.
   Los grises son SIEMPRE neutros (R≈G≈B): `rgba(255,255,255,α)` sobre oscuro,
   `rgba(15,20,30,α)` o `rgba(20,24,31,α)` sobre claro.
   **Prohibidos** los grises con deriva azul (`#94a3b8`, `#64748b`, `#475569`) en la UI.

2. **Un solo acento por vista.** `--brand` aparece en UN elemento: un pill, un punto que late,
   un borde fino, un dato. Nunca en checkmarks, precios, hovers, glows ni CTAs.
   Excepciones vivas y acotadas: la CTA de login/registro y el botón de la navbar pública.

3. **CTA primaria = invertida.** Botón blanco con texto negro sobre fondo oscuro; negro con
   texto blanco sobre fondo claro. Pill (radius 999) en web; radius 8–11 en la app.

4. **Minimalista, pequeño y espaciado.** No se quitan elementos, se hacen compactos:
   cuerpo 12–14 px, labels 10–11 px en mayúsculas con tracking, márgenes generosos
   entre secciones (24–32 px en app, 80–120 px en web).

5. **Cristal, no cartón.** Los contenedores son paneles de cristal: degradado diagonal muy sutil,
   borde de 1 px al 10 %, `backdrop-filter: blur(14px)`, sombra profunda + highlight interior,
   y **brillo en dos esquinas diagonalmente opuestas** (sup-izq + inf-der). Es la firma.

6. **Textura de fondo.** Puntos muy tenues en lienzos y zonas de trabajo; orbes blancos
   difuminados que flotan lentísimo; guías punteadas alineadas con la columna de contenido.

7. **Líneas discontinuas = "extra / zona secundaria".** Sub-contenedores, zonas de drop, notas,
   resúmenes de IA, marcos de configuración: `1px dashed` al 16 %. Lo principal va sólido.

8. **Sin emojis y sin iconos de color.** SVG de trazo fino, `currentColor`, `stroke-width` 1.6–2.

9. **Los dos temas se diseñan.** Oscuro es el principal. El claro NO es una inversión:
   lienzo blanco puro, "blanco roto" `#f4f2ee` solo para cajas-marco, sombras cálidas
   `rgba(60,50,35,…)`.

10. **El color se reserva a los datos.** Gráficas, KPIs de analítica y la señal de urgencia
    conservan color. Todo lo demás (chips, estados, badges) se neutraliza a gris.

---

## Prohibiciones

- Más de un elemento con `--brand` por vista. Cualquier otro color saturado en UI. Sombras de color.
- Grises azulados en la skin mono.
- `feTurbulence` / `fractalNoise` en backdrops: posteriza y mancha.
- Emojis. Iconos de color. Ilustraciones stock.
- `alert()` / `confirm()` del navegador: modales propios siempre.
- `max-height` para acordeones (usar `grid-template-rows: 0fr → 1fr`).
- Rotación 3D en hover de tarjetas de lista (solo se admite en la tarjeta de auth).
- Texto sobre imagen sin velo: siempre degradado desde el lado del texto hasta el color de fondo.
- Titulares que explican tras dos puntos. Exclamaciones. Copy de anuncio.
