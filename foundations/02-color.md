# 02 · Color

## Cómo está montado

Tres capas, en este orden de prioridad:

| Capa | Variables | Dónde se usa |
|---|---|---|
| **Neutra** | `--bg-*`, `--border*`, `--text-*`, `--accent*` | El 95 % de la interfaz |
| **Marca** | `--brand`, `--brand-2`, `--brand-soft`, `--brand-border` | UN elemento por vista |
| **Datos** | `--c-*` (crudos) | Solo gráficas, KPIs y señales críticas |

Un componente **nunca** escribe un hex. Escribe `var(--…)`. Las dos únicas excepciones
son el acento de marca y los colores de serie de una gráfica.

## Neutros — oscuro

```
--bg-base        #161617   negro NEUTRO "no tan oscuro"
--bg-surface     rgba(255,255,255,0.045)
--bg-surface-2   rgba(255,255,255,0.07)
--bg-surface-3   rgba(255,255,255,0.10)
--bg-input       rgba(0,0,0,0.28)
--bg-elevated    #1c1c1e   modales, popovers
--frame-bg       #1c1c1e   marcos que engloban cajas
--border         rgba(255,255,255,0.09)
--border-2       rgba(255,255,255,0.16)   ← también el color de los DASHED
--text-primary   #f2f4f6
--text-secondary rgba(255,255,255,0.62)
--text-muted     rgba(255,255,255,0.40)
```

Por qué `#161617` y no negro absoluto: el cristal (`rgba(255,255,255,0.045)`) y el sidebar
necesitan separarse del lienzo. Sobre `#000` el cristal desaparece.

## Neutros — claro

```
--bg-base        #ffffff   lienzo BLANCO PURO
--bg-surface-3   #f4f2ee   "blanco roto" (LAB 95.6/0.45/1.21)
--frame-bg       #f4f2ee   ← solo para cajas que engloban otras cajas
--border         rgba(15,20,30,0.10)
--border-2       rgba(15,20,30,0.20)
--text-primary   #14181f
--text-secondary rgba(20,24,31,0.62)
--text-muted     rgba(20,24,31,0.42)
```

Las sombras del tema claro son **cálidas**: `rgba(60,50,35,α)`, nunca `rgba(0,0,0,α)` puro.
Un gris frío sobre blanco roto se ve sucio.

## Marca

```
--brand        #10B981    (Wismify usa esmeralda; cámbialo aquí y cambia toda la app)
--brand-2      #059669
--brand-soft   rgba(16,185,129,0.10)
--brand-border rgba(16,185,129,0.35)
```

Sitios legítimos donde aparece, **uno por vista**:
- el punto que late de un `.pill-live` ("EN DIRECTO", "SEMANA ACTIVA")
- el borde de foco de un input en login/registro
- la CTA de login/registro y de la navbar pública
- un único dato destacado (el bote de la semana, el puesto #1)

Sitios donde **no** aparece: checkmarks, iconos de menú, hovers de fila, bordes de tarjeta,
badges de estado, precios, links de navegación.

## Datos (colores crudos)

Paleta semántica fija. Es la única zona donde el color significa algo:

| Serie | Color |
|---|---|
| YouTube / vídeo largo | `#ef4444` |
| TikTok | `#38bdf8` |
| Instagram / Reels | `#e1306c` |
| Twitch / clips nativos | `#a78bfa` |
| X (Twitter) | `#94a3b8` |
| Views | `#60a5fa` |
| Engagement | `#fbbf24` |
| Aprobado / pagado | `#10B981` |
| Rechazado | `#ef4444` |
| Pendiente de revisión | `#fbbf24` (dashed) |
| Neutros de ejes/grid | texto `--text-secondary`, grid `--border` |

Reglas de gráfica: una escala por gráfica, etiquetas en cada extremo, área tenue bajo la línea,
punto final enfatizado, `tabular-nums`. Si la gráfica **no** compara categorías semánticas,
va monocroma: blanco al 100 / 70 / 40 %.

## Estados

En la skin mono TODO chip de estado se neutraliza:

```
--chip-bg     rgba(255,255,255,0.06)
--chip-text   rgba(255,255,255,0.72)
--chip-border rgba(255,255,255,0.16)
```

La única señal cromática que sobrevive en una lista es el **borde izquierdo** de la fila
(prioridad / estado crítico), 3 px sólidos.
