# 08 · Clips: subir, listar, revisar

Tres pantallas que comparten el mismo objeto.

---

## 8.1 · Subir clip

```
[kick] NUEVO
[page-title] Sube tu clip.

┌─ .glass · máx 720px centrado ──────────────────────────────┐
│  ┌ .drop · 1.5px dashed + puntitos + ants al arrastrar ──┐ │
│  │              ⤓                                        │ │
│  │      Arrastra el vídeo o haz clic                     │ │
│  │      MP4 o MOV · máx 500 MB                           │ │
│  └───────────────────────────────────────────────────────┘ │
│  ──────────── o pega el enlace publicado ────────────────  │
│  [ https://tiktok.com/@…                               ]   │
│                                                            │
│  TÍTULO       [ …                                      ]   │
│  PLATAFORMA   [ TikTok ▾ ]   FECHA  [ 17/09/2026 ]         │
│  ┌ .sub dashed · NOTA PARA EL CREADOR (opcional) ───────┐  │
│  │ [ textarea.draft ]                                   │  │
│  └──────────────────────────────────────────────────────┘  │
│                                    [Cancelar] [Subir clip] │
└────────────────────────────────────────────────────────────┘
```

- Progreso de subida: barra monocroma + porcentaje en `tabular-nums` + miniatura
  extraída del primer frame. El botón muestra spinner y cambia a "Subiendo… 42 %".
- Validación **antes** de subir: formato, tamaño, duración mínima, enlace parseable.
- Si el clip ya existe (mismo enlace), el error es accionable:
  "Ya subiste este clip el 14 sep. Ver clip →".

---

## 8.2 · Mis clips / Todos los clips

Dos vistas conmutables con `.view-switch` (grid / lista), guardado en `localStorage`.

**Vista grid** — `.clip-grid` (ver `components/tablas-listas.md`).
**Vista lista** — `.row` con miniatura 64×36, título, plataforma, views, fecha,
`.chip` de estado y borde izquierdo de estado.

Filtros en la barra: estado · plataforma · rango de fechas · buscador.
La barra de filtros es sticky bajo la topbar, con `backdrop-filter: blur(12px)`.

---

## 8.3 · Revisión (solo creador)

La cola. Diseñada para despachar 30 clips sin levantar la mano del teclado.

```
┌─ split 380px | resto ────────────────────────────────────────────┐
│ COLA (28)              │  ┌─ reproductor 16:9 ────────────────┐  │
│ ┌ fila activa ───────┐ │  │                                   │  │
│ │ ▣ @clipper · 84K   │ │  └───────────────────────────────────┘  │
│ └────────────────────┘ │  @clipper · TikTok · 84K · hace 2h      │
│ ┌────────────────────┐ │                                        │
│ │ ▣ @otro · 12K      │ │  ┌ .frame-dashed · ANÁLISIS AUTOMÁTICO │
│ └────────────────────┘ │  │ Duración 34s · sin marca de agua ·  │
│ …                      │  │ audio original · 3 cortes           │
│                        │  └─────────────────────────────────────┘
│                        │  [ Rechazar ]        [ Aprobar ]        │
└──────────────────────────────────────────────────────────────────┘
```

- Atajos: `A` aprobar · `R` rechazar · `J`/`K` o `↑`/`↓` mover · `Espacio` play/pausa.
  Los atajos se muestran en un `.sub` discontinua al pie de la columna izquierda.
- Aprobar es instantáneo y muestra toast con "Deshacer" (4 s).
- Rechazar abre modal pidiendo motivo (select de motivos frecuentes + texto libre).
  El motivo se le enseña al clipper tal cual.
- La caja de análisis automático es `.frame-dashed` — discontinua con puntitos,
  porque es información secundaria generada, no un dato del clipper.
- Tras vaciar la cola: estado vacío con `.sub`, icono de check en círculo y
  "Cola limpia. 28 clips revisados hoy." Sin confeti.
