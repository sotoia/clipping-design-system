# Inputs y formularios

## Input base

```css
.input {
  width: 100%; box-sizing: border-box;
  background: var(--bg-input); color: var(--text-primary);
  border: 1px solid var(--border); border-radius: 11px;
  padding: 11px 14px; font: inherit; font-size: 14px; outline: none;
  transition: border-color .25s, box-shadow .25s, background .25s;
}
.input::placeholder { color: rgba(255,255,255,0.28); }
.input:focus {
  border-color: rgba(255,255,255,0.32);
  box-shadow: 0 0 0 3px rgba(255,255,255,0.05);
}
```

En la app el foco es **neutro**. El foco con acento de marca se reserva a
login y registro:

```css
.input.brand-focus:focus {
  border-color: rgba(var(--brand-rgb),0.55);
  background: rgba(255,255,255,0.065);
  box-shadow: 0 0 0 4px rgba(var(--brand-rgb),0.12), inset 0 1px 0 rgba(255,255,255,0.08);
}
```

En la app, radio `8px` y padding `9px 12px` (más compacto). En auth, `11px` / `11px 14px`.

## Label

```css
.label {
  display: block; margin-bottom: 6px;
  font: 600 11px Inter, sans-serif; text-transform: uppercase; letter-spacing: .05em;
  color: var(--text-muted);
}
```

## Campo de contraseña

Input con `padding-right: 44px` + botón de ojo absoluto a la derecha.
El botón cambia entre `Eye` / `EyeOff` (16 px) y **debe** llevar `aria-label` y `title`
traducidos, porque no tiene texto.

```css
.pass { position: relative; }
.pass .input { padding-right: 44px; }
.pass .eye {
  position: absolute; right: 10px; top: 50%; transform: translateY(-50%);
  background: transparent; border: none; cursor: pointer; padding: 6px; border-radius: 7px;
  color: var(--text-muted);
  display: inline-flex; align-items: center; justify-content: center;
  transition: color .2s, background .2s;
}
.pass .eye:hover { color: var(--text-primary); background: var(--bg-hover); }
```

## Select nativo

El popup nativo de `<option>` **no** acepta fondos translúcidos: Windows/Chrome los
pinta blancos y el texto claro desaparece. Hay que forzar opacos:

```css
select.input option { background-color: #0b0d10; color: #f2f4f6; }
[data-theme="light"] select.input option { background-color: #fff; color: #14181f; }
select.input option:checked, select.input option:hover { background-color: #2a2a2e; }
[data-theme="light"] select.input option:checked { background-color: #eef2f7; color: #14181f; }
```

## Textarea borrador

Discontinuo en reposo (es una zona por rellenar), sólido al foco:

```css
textarea.draft { border: 1px dashed var(--border-2); resize: vertical; min-height: 96px; }
textarea.draft:focus { border-style: solid; }
```

## Zona de subida de clip (drop)

El patrón dashed en su expresión más literal. Con marching ants cuando está activa.

```css
.drop {
  position: relative;
  border: 1.5px dashed var(--border-2); border-radius: 16px;
  background: radial-gradient(var(--dots) 1px, transparent 1px) -1px -1px / 12px 12px, var(--frame-bg);
  padding: 36px 24px; text-align: center;
  transition: border-color .2s, background .2s;
}
.drop.is-over { border-color: var(--hover-border); background-color: var(--bg-hover); }
```

## Divisor "o con email"

```html
<div class="divider"><i></i><span>o con email</span><i></i></div>
```
```css
.divider { display:flex; align-items:center; gap:10px; margin-bottom:20px; }
.divider i { flex:1; height:1px; background: var(--border); }
.divider span { font-size:11px; color: var(--text-muted); }
```

## Mensajes de error y éxito

Sin fondo saturado. El error **vibra una vez** al aparecer (`key` distinta → re-anima).

```css
.msg { padding: 10px 14px; border-radius: 11px; font-size: 13px; }
.msg-error {
  background: rgba(var(--c-red-rgb),.10); border: 1px solid rgba(var(--c-red-rgb),.25);
  color: var(--c-red2); animation: shake .45s ease;
}
.msg-ok {
  background: var(--brand-soft); border: 1px solid var(--brand-border); color: var(--brand);
}
```

## Reglas de formulario

- `gap: 16px` entre campos. `gap: 6px` entre label e input.
- `autoComplete` correcto siempre (`email`, `current-password`, `new-password`, `name`).
- Validación **al enviar**, no al teclear. El mensaje sustituye, no añade.
- El botón de envío ocupa el ancho completo en auth; en la app va alineado a la derecha.
- Nunca `alert()` ni `confirm()`. Modal propio o toast.
