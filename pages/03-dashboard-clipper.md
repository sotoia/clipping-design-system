# 03 · Dashboard del clipper

La primera pantalla tras entrar. Responde a tres preguntas en cinco segundos:
**cuánto he ganado**, **en qué puesto voy** y **qué clip debo subir ahora**.

```
[kick] TU PANEL
[page-title] Hola, @nombre.
[page-desc] Semana 38 · quedan 3 días para el cierre.        [pill-live · BOTE 400 €]

┌─ .frame · puntitos 9px ────────────────────────────────────────────┐
│ ┌ Views  ┐ ┌ Clips   ┐ ┌ Aprobados ┐ ┌ Puesto ┐ ┌ Por cobrar ┐     │
│ │ 184.2K │ │ 6       │ │ 5         │ │ #17    │ │ 62 €       │     │
│ │ ▁▃▅▇█  │ │ esta sem│ │ 1 pendien.│ │ ↑ 4    │ │ pago vie.  │     │
│ └────────┘ └─────────┘ └───────────┘ └────────┘ └────────────┘     │
└────────────────────────────────────────────────────────────────────┘

┌─ .glass · Subir un clip ─────────┐  ┌─ .glass · Tu posición ───────┐
│  ┌ .drop dashed + ants ────────┐ │  │  #17  ↑4 esta semana         │
│  │   ⤓  Arrastra tu clip       │ │  │  ──── mini ranking ────      │
│  │   o pega el enlace          │ │  │  #16 @otro      191K         │
│  └─────────────────────────────┘ │  │  #17 @tú        184K  ←      │
│  [ Subir clip ]                  │  │  #18 @otro      179K         │
└──────────────────────────────────┘  └──────────────────────────────┘

┌─ .glass · Tus últimos clips ───────────────────────────────────────┐
│  grid auto-fill minmax(240px,1fr)  ·  6 tarjetas de clip           │
└────────────────────────────────────────────────────────────────────┘
```

## Reglas

- El acento de la vista es la **pill del bote**. Por tanto el `#17` del puesto va
  en `--text-primary`, no en verde.
- La fila de KPIs vive dentro de un `.frame` con puntitos; las cinco cards son **opacas**.
- "Por cobrar" es el único KPI con unidad (`€`, 14 px, `--text-muted`).
- El delta del puesto (`↑ 4`) usa flecha SVG + `tabular-nums`, texto neutro.
- La zona de subida es la pieza con **marching ants** de la página. Una sola.
- Los clips se muestran con `.clip` (miniatura 16:9 con velo, duración en `.pill`,
  estado en `.chip` gris).
- Vacío total (clipper nuevo): en vez del grid, un `.sub` centrado con icono,
  una frase y el botón primario. Nunca una ilustración.

## Datos que se muestran por clip

`miniatura · título · plataforma (icono mono) · views · fecha · estado (.chip)`.
Si está aprobado y pagado, se añade el importe a la derecha en `tabular-nums`.
