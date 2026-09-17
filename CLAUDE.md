# Instrucciones para Claude

Este repositorio es un **sistema de diseño**, no una app. Cuando trabajes con él:

## Antes de escribir una sola línea

1. Lee `foundations/01-filosofia.md`. Son 10 reglas; romper una se nota al instante.
2. Lee la ficha de la página que vas a construir en `pages/`.
3. Lee el componente que vas a usar en `components/`.
4. `tokens/tokens.css` es la fuente de verdad. **Nunca escribas un hex** salvo
   el acento de marca y los colores de serie de una gráfica.

## Errores que se cometen siempre

| Error | Qué pasa |
|---|---|
| Usar `#94a3b8` / `#64748b` como gris | Deriva azul: la pantalla deja de parecer de este producto |
| Poner verde en más de un sitio | Se pierde la jerarquía; el acento deja de significar nada |
| Botón primario verde | El primario es **invertido**: claro sobre oscuro |
| Cristal sin el `::before` del sheen | Queda un glassmorphism genérico |
| Card translúcida sobre un marco con puntos | Los puntos sangran bajo el texto: la card va **opaca** |
| Invertir el tema oscuro para hacer el claro | El claro tiene lienzo blanco puro y sombras cálidas |
| Emojis en estados vacíos o notificaciones | Prohibidos en todo el producto |
| Olvidar `tabular-nums` | Los números bailan al actualizarse en vivo |
| `alert()` / `confirm()` | Modal propio siempre |
| Hover sin `@media (hover: hover)` | En táctil el estado se queda pegado |

## Cómo se decide el acento de una pantalla

Cada pantalla tiene **exactamente un** elemento con `--brand`. Elígelo el primero
y apunta cuál es. Si luego necesitas otro, quita el anterior.

Sitios legítimos: la pill que late de la cabecera, el borde de la card #1 del ranking,
el foco de los inputs de auth, la CTA de auth, una barra de progreso que es "la tuya".

## Jerarquía de contenedores

```
main.dots  →  .frame (opaco + puntos)  →  .card (opaca)  →  .sub (discontinua)
```

Nunca más de tres niveles. Si necesitas un cuarto, es un modal o una pestaña.

## Al añadir una pantalla nueva

1. Crea `pages/NN-nombre.md` con: esquema ASCII, reglas propias, y qué elemento
   se lleva el acento.
2. Si inventas un componente, documéntalo en `components/` con su CSS completo.
3. Añade el ejemplo a `preview/index.html` para que se pueda ver en ambos temas.
4. Actualiza el índice de `README.md`.

## Al revisar una pantalla

- ¿Un solo elemento con el acento?
- ¿Grises neutros?
- ¿Funciona en claro **y** en oscuro? Ábrelo en los dos.
- ¿Cero emojis, cero iconos de color?
- ¿`tabular-nums` en todas las cifras?
- ¿Sin scroll horizontal en móvil?
- ¿`prefers-reduced-motion` respetado?
