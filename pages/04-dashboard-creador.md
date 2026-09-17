# 04 · Dashboard del creador

Panel de control de la comunidad. Tres bloques: salud de la semana,
lo que requiere acción, y el pulso de los clippers.

```
[kick] COMUNIDAD
[page-title] Tu comunidad en una pantalla.
[page-desc] Semana 38 · 240 clippers · cierre el domingo 23:59.   [pill-live · EN DIRECTO]

┌─ .frame ───────────────────────────────────────────────────────────────┐
│ Views totales │ Clips subidos │ Pendientes │ Aprobación │ Coste semana  │
│    4.82M      │      317      │     28     │    81 %    │    612 €      │
│   ▁▃▄▆█▇      │   ▁▂▅▇▆       │  ⏱ 2.4h    │  ◐ gauge   │  de 800 €     │
└────────────────────────────────────────────────────────────────────────┘

┌─ .glass · Cola de revisión ──────────────────┐ ┌─ .glass · Top semana ──┐
│  28 clips esperando                          │ │ 1 @clipper   1.2M      │
│  ┌ fila con borde-izq ámbar ───────────────┐ │ │ 2 @otro      940K      │
│  │ ▣  @clipper · TikTok · 84K · hace 2h    │ │ │ 3 @otro      780K      │
│  │                     [Rechazar][Aprobar] │ │ │ …                      │
│  └─────────────────────────────────────────┘ │ │ [Ver ranking completo] │
│  … 5 filas + "Ver las 28"                    │ └────────────────────────┘
└──────────────────────────────────────────────┘

┌─ .glass · Rendimiento ─────────────────────────────────────────────────┐
│  línea de views por día · donut por plataforma · barras top clippers   │
└────────────────────────────────────────────────────────────────────────┘
```

## Reglas

- **Cinco KPIs máximo** en la fila de cabecera. El sexto va al dashboard modular.
- "Coste semana" muestra progreso hacia el presupuesto con gauge
  (semáforo `#ef4444 / #fbbf24 / #22c55e` — es un KPI, tiene derecho al color).
- La cola de revisión es la única lista con **borde izquierdo ámbar** en toda la app.
- Los botones de la fila: `Aprobar` = `.btn-primary` (invertido), `Rechazar` = `.btn-ghost`.
  Rechazar abre modal pidiendo motivo — nunca `confirm()`.
- Las gráficas del bloque de rendimiento usan la paleta de plataformas
  (ver `components/kpi-y-graficas.md`). Es la única zona con color saturado.
- El dashboard es **modular**: 12 columnas, módulos arrastrables, tile de
  "añadir módulo" con marching ants al final del grid.
