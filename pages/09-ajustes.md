# 09 · Ajustes y perfil

Lienzo con puntos, navegación lateral de secciones, y cada sección en una `.glass`.

```
main.dots
  [kick] CUENTA
  [page-title] Ajustes.

  ┌─ 200px ──┬─ contenido ──────────────────────────────────┐
  │ Perfil   │  ┌─ .glass · Perfil ───────────────────────┐ │
  │ Apariencia│  │ avatar 72 · [Cambiar] [Quitar]         │ │
  │ Pagos    │  │ NOMBRE     [ … ]                        │ │
  │ Redes    │  │ HANDLE     [ @… ]                       │ │
  │ Notific. │  │ BIO        [ textarea.draft ]           │ │
  │ Seguridad│  │                            [ Guardar ]  │ │
  │ ──────   │  └─────────────────────────────────────────┘ │
  │ Salir    │                                              │
  └──────────┴──────────────────────────────────────────────┘
```

La navegación de secciones es una columna de `.menu .item` de 200 px, sticky.
En móvil se convierte en `.tabs` horizontales con scroll.

## Apariencia — la sección que define el shell

Es donde se elige el modo de panel. Dos cards **de previsualización**, no un select.

```html
<div class="shell-picker">
  <button class="shell-opt is-selected">
    <span class="shell-preview shell-preview--classic" aria-hidden></span>
    <b>Clásico</b><small>Barra lateral fija. Más denso.</small>
  </button>
  <button class="shell-opt">
    <span class="shell-preview shell-preview--modern" aria-hidden></span>
    <b>Modern</b><small>Dock flotante abajo. Más aire.</small>
  </button>
</div>
```

```css
.shell-picker { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
.shell-opt {
  text-align: left; cursor: pointer; padding: 12px;
  background: var(--bg-card); border: 1px solid var(--border); border-radius: 14px;
  transition: transform .18s cubic-bezier(.22,.8,.28,1), border-color .18s, box-shadow .18s;
}
.shell-opt:hover { transform: translateY(-2px); border-color: var(--hover-border); box-shadow: var(--hover-shadow); }
.shell-opt.is-selected { border-color: var(--hover-border); box-shadow: var(--hover-shadow); }
.shell-opt b { display:block; font-size:12.5px; margin-top:10px; color: var(--text-primary); }
.shell-opt small { font-size:11.5px; color: var(--text-secondary); }

/* Maqueta en miniatura, dibujada con CSS — nada de capturas */
.shell-preview {
  display:block; height: 92px; border-radius: 10px;
  border: 1px solid var(--border);
  background: radial-gradient(var(--dots) 1px, transparent 1px) -1px -1px / 8px 8px, var(--frame-bg);
  position: relative; overflow: hidden;
}
.shell-preview--classic::before { content:''; position:absolute; left:0; top:0; bottom:0; width:26%;
  background: var(--bg-surface-2); border-right: 1px solid var(--border); }
.shell-preview--modern::after { content:''; position:absolute; left:50%; bottom:8px; transform:translateX(-50%);
  width:52%; height:16px; border-radius:8px; background: var(--bg-surface-3); border: 1px solid var(--border-2); }
```

La selección se marca con **borde + sombra**, nunca con el color de marca:
el acento de esta vista se lo lleva otra cosa (o nada).

Debajo, el toggle de tema con la misma mecánica: dos cards, `Oscuro` / `Claro`,
y una tercera `Sistema` que escucha `prefers-color-scheme`.

## Pagos

- Método: PayPal o transferencia. Radio cards, no select.
- IBAN/email en `input` con fuente **mono**.
- Un `.sub` discontinua explicando cuándo se paga y el mínimo para cobrar.
- Historial de pagos en tabla con `tabular-nums`.

## Redes

Una fila por plataforma: icono monocromo 16 px + handle + `.chip` de estado
(`Conectada` / `Sin conectar`) + botón `.btn-secondary`. Sin logos de color.

## Seguridad

Cambio de contraseña, sesiones activas (dispositivo, IP truncada, última actividad)
y 2FA. **Cerrar sesión en todos los dispositivos** es `.btn-danger` y abre modal
de confirmación propio.

## Zona de peligro

Al final, en un `.sub` con `border-color: rgba(var(--c-red-rgb),.25)`:
eliminar cuenta. Texto plano, un botón `.btn-danger`, y modal que pide escribir
el handle para confirmar. Sin rojo de fondo, sin iconos de aviso.
