# 00 · Shell y modos de panel

Toda pantalla autenticada vive dentro del shell. El shell tiene **dos modos**
y el usuario elige el suyo en Ajustes → Apariencia.

```
<html data-theme="dark|light" data-shell="classic|modern">
```

Ambos se persisten en `localStorage` y en el perfil de Supabase, para que
el modo viaje entre dispositivos. Se aplican **antes del primer pintado**
(script inline en el `<head>`) — si no, hay flash de tema.

```html
<script>
  (function(){
    try {
      var t = localStorage.getItem('theme') || 'dark';
      var s = localStorage.getItem('shell-mode') || 'classic';
      document.documentElement.dataset.theme = t;
      document.documentElement.dataset.shell = s;
    } catch(e) {}
  })();
</script>
```

## Modo clásico

```
┌──────────┬──────────────────────────────┐
│          │ topbar 56px                  │
│ sidebar  ├──────────────────────────────┤
│ 260px    │                              │
│ sticky   │  main .dots                  │
│ blur 12  │  padding 28px 32px           │
│          │                              │
│  ────    │   [kick]                     │
│  usuario │   [page-title]               │
│  tema    │   [contenido]                │
│  salir   │                              │
└──────────┴──────────────────────────────┘
```

Es el modo por defecto. Más denso, más ítems visibles, mejor para el creador
que pasa horas revisando clips.

## Modo modern — dock

```
┌────────────────────────────────────────┐
│  ┌─ topbar flotante ────────────────┐  │  sticky top:12px, radius 14, cristal
│  └──────────────────────────────────┘  │
│                                        │
│   main .dots (ancho completo)          │
│   padding: 28px 32px 104px             │
│                                        │
│                                        │
│          ┌────────────────┐            │
│          │ ▣ ▤ ▥ │ ▦ ▧ ▨ │            │  dock fijo bottom:20px
│          └────────────────┘            │
└────────────────────────────────────────┘
```

Más aire, más "sistema operativo". Mejor para el clipper, que entra a subir,
mirar su puesto y salir.

Detalles obligatorios del dock (ver `components/navegacion.md`):
- cristal con el **sheen en dos esquinas**, igual que las cards
- magnificación al hover: el icono sube 9 px y escala 1.18; los vecinos, 4 px y 1.08
- punto de "pestaña abierta" bajo el icono activo
- etiqueta emergente encima, no dentro
- máximo 7 iconos + 1 separador; el resto va al menú del avatar

## Estructura de una página

Todas iguales:

```html
<main class="main dots">
  <header class="page-head">
    <span class="kick">Clips</span>
    <h1 class="page-title">Todo lo que ha subido la comunidad.</h1>
    <p class="page-desc">Revisa, aprueba y paga desde aquí.</p>
    <div class="page-actions"> … botones … </div>
  </header>

  <section class="frame">  <!-- fila de KPIs -->
  <section>                <!-- contenido -->
</main>
```

```css
.page-head { margin-bottom: 24px; }
.page-head .kick { margin-bottom: 10px; }
.page-desc { font-size: 12.5px; color: var(--text-secondary); margin-top: 6px; max-width: 62ch; }
.page-actions { display:flex; gap:8px; margin-top:16px; }
```

El acento de la vista (`--brand`) se decide **aquí**, en la cabecera: o la `.pill-live`
de la cabecera, o un dato del contenido. Nunca los dos.
