# 07 · Movimiento

Regla general: el movimiento **informa**, no decora. Si una animación no comunica
jerarquía, estado o causa-efecto, sobra.

## Duraciones

| Uso | Duración | Curva |
|---|---|---|
| Cambio de estado (color, borde) | 0.15 s | `ease` |
| Hover con lift | 0.22–0.25 s | `cubic-bezier(.22,.8,.28,1)` |
| Entrada de tarjeta | 0.3 s | `ease` |
| Entrada de modal | 0.2 s | `ease` |
| Entrada escalonada de auth | 0.55 s | `cubic-bezier(.33,1,.68,1)` |
| Entrada de la tarjeta de auth | 0.7 s | `cubic-bezier(.16,1,.3,1)` |
| Reveal on scroll | 0.6–0.9 s | `cubic-bezier(.22,.8,.28,1)` |
| Orbes | 24–38 s | `ease-in-out alternate` |
| Marching ants | 22 s | `linear` |

## Animaciones canónicas

```css
@keyframes fadeIn      { from { opacity:0; transform: translateY(6px) }  to { opacity:1; transform:none } }
@keyframes fadeInScale { from { opacity:0; transform: scale(.96) }        to { opacity:1; transform:none } }
@keyframes stagger     { from { opacity:0; transform: translateY(10px); filter: blur(6px) } to { opacity:1; transform:none; filter:blur(0) } }
@keyframes cardIn      { from { opacity:0; transform: translateY(18px) scale(.965); filter: blur(10px) } to { opacity:1; transform:none; filter:blur(0) } }
@keyframes shake       { 10%,90%{transform:translateX(-1px)} 20%,80%{transform:translateX(2px)}
                         30%,50%,70%{transform:translateX(-3px)} 40%,60%{transform:translateX(3px)} }
@keyframes pulse       { 0%,100%{opacity:1;transform:scale(1)} 50%{opacity:.55;transform:scale(.82)} }
@keyframes rotate      { to { transform: rotate(360deg) } }
```

El `filter: blur()` en la entrada es deliberado: hace que el elemento parezca
"enfocarse" en vez de deslizarse. Es la diferencia entre caro y barato.

## Entrada escalonada (auth y listas)

Cada elemento entra 60 ms después del anterior, empezando en 0.2 s:

```
título 0.20s → Google 0.26s → divider 0.32s → campo1 0.38s → campo2 0.44s
→ olvidé 0.47s → CTA 0.50s → alternativa 0.55s
```

En rejillas se usa stagger por índice: `delay = (i % 3) * 0.08s`.

## Hover

- **Lift**: `translateY(-2px)` en cards de cristal, `-3px` en cards dentro de marco,
  `-1px` en botones y filas.
- Siempre acompañado de `border-color` → `--hover-border` y sombra más profunda.
- Los iconos de una card de KPI pueden hacer `scale(1.14) rotate(-5deg)` al hover de la card.
- **Nunca rotación 3D** en tarjetas de lista. Se permite solo en la tarjeta de auth: ±2.4°.
- Todo hover va envuelto en `@media (hover: hover)`. En táctil no existe.

## Barrido de brillo (CTA)

```css
.btn-shine { position: relative; overflow: hidden; }
.btn-shine::after {
  content:''; position:absolute; top:0; left:-80%; width:60%; height:100%;
  transform: skewX(-18deg);
  background: linear-gradient(100deg, transparent, rgba(255,255,255,.22), transparent);
  transition: left .5s ease;
}
.btn-shine:hover::after { left: 120%; }
```

## Contadores

`requestAnimationFrame` con easing cúbico, `toLocaleString()` y `tabular-nums`.
Duración 0.9–1.2 s. Solo en la primera carga, nunca al refrescar datos en vivo.

## Reduced motion

Obligatorio, en todas las pantallas:

```css
@media (prefers-reduced-motion: reduce) {
  .amb i, .glass, .st, .fade-in, .ants rect, .pill-live .d { animation: none !important; }
  .glass, .lift { transition: none; }
}
```
