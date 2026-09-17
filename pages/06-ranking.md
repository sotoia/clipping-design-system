# 06 · Ranking

La pantalla más emocional del producto. Tiene que dar ganas de subir otro clip.

```
[kick] COMPETICIÓN
[page-title] Quién manda esta semana.
[tabs · dashed abajo]  Semanal · Mensual · Histórico · Por plataforma
                                                    [pill · Cierra en 3d 04h]

┌─ .frame · puntitos ────────────────────────────────────────────────┐
│                                                                    │
│      ┌── #2 ──┐   ┌───── #1 (8% más alta) ─────┐   ┌── #3 ──┐      │
│      │ av 46  │   │  av 46 · borde brand        │   │ av 46  │     │
│      │ @nombre│   │  @nombre                    │   │ @nombre│     │
│      │ 940K   │   │  1.24M views                │   │ 780K   │     │
│      │[18 cl.]│   │  [24 clips] [+12%]          │   │[15 cl.]│     │
│      └────────┘   └─────────────────────────────┘   └────────┘     │
│                                                                    │
│  ┌ tabla del #4 al #50 ──────────────────────────────────────────┐ │
│  │ #   CLIPPER          VIEWS    CLIPS   ENG.    Δ               │ │
│  │ 4   ▣ @nombre        712K     14      6.2%    ↑2              │ │
│  │ …                                                             │ │
│  └───────────────────────────────────────────────────────────────┘ │
│                                                                    │
│  ┌ .sub dashed · sticky bottom · TU POSICIÓN ───────────────────┐  │
│  │ #17  ▣ @tú           184K     6       5.8%    ↑4             │  │
│  └──────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────┘
```

## Reglas duras

- **Sin medallas, sin emojis, sin oro/plata/bronce.** El puesto se lee por el número
  y por la altura de la card. Es lo que separa esto de una app de fantasy.
- La card #1 lleva `border-color: var(--brand-border)` — y **eso consume el acento**.
  Por tanto la pill del contador de cierre es `.pill` neutra, no `.pill-live`.
- El número de puesto va en `.rank-pos` (cuadrado 26 px, radius 8, 800, tabular).
- Todas las cifras con `tabular-nums`. El ranking se actualiza en vivo y sin esto
  los números bailan.
- Tu fila siempre visible: `.sub` discontinua, `position: sticky; bottom: 0`,
  `backdrop-filter: blur(10px)`, mismas columnas que la tabla.
- Cambio de posición: flecha SVG + número, neutro. Nada de verde/rojo.
- Empates: mismo número de puesto, desempate por fecha del primer clip de la semana,
  indicado en el tooltip del puesto.

## Actualización en vivo

Realtime de Supabase. Cuando una fila cambia de posición:
- la fila se desplaza con `transition: transform .5s cubic-bezier(.22,.8,.28,1)`
- y hace **un** flash de `background: var(--bg-surface-2)` de 600 ms

Nunca parpadeo, nunca sonido, nunca confeti.

## Filtros

Tabs con separador discontinuo. El periodo activo se marca con
`border-bottom: 2px solid var(--text-primary)`.

## Vacío

Semana recién abierta, nadie ha subido: el podio muestra tres cards con
**marching ants** y "Puesto libre" en 12 px muted. Es el mejor gancho que tiene la app.
