# 02 · Registro

Misma cáscara que login (`AuthShell`). Solo cambia el contenido de la tarjeta.
Todo lo del lienzo, orbes, cristal, spotlight, cookies y reduced-motion está en
`01-login.md` y **no se repite ni se modifica**.

## Diferencias respecto a login

| | Login | Registro |
|---|---|---|
| Título | "Iniciar sesión" | "Crear cuenta" |
| Botón social | "Continuar con Google" | "Registrarse con Google" |
| Campos | email, contraseña | **nombre**, email, contraseña |
| Placeholder de contraseña | `••••••••` | "Mínimo 8 caracteres" |
| `autoComplete` | `current-password` | `new-password` |
| Enlace extra | "¿Has olvidado tu contraseña?" | — |
| Mensaje de éxito | — | sí (`.msg-ok`) |
| CTA | "Entrar al panel" | "Crear cuenta" |
| Alternativa | "¿No tienes cuenta? Crear" | "¿Ya tienes cuenta? Iniciar sesión" |

## Escalonado

Un campo más ⇒ la cadena se desplaza:

```
título 0.20 · Google 0.26 · divisor 0.32 · nombre 0.38 · email 0.44
· contraseña 0.50 · CTA 0.56 · alternativa 0.60
```

## Validación

- Contraseña **mínimo 8 caracteres**, comprobado en cliente antes de enviar.
  Mensaje: "La contraseña debe tener al menos 8 caracteres."
- Nombre opcional en el formulario, pero se recorta con `.trim()` antes de enviar.
- Email duplicado → el backend devuelve `already_registered` y el mensaje es
  **accionable**: "Este email ya está registrado. Inicia sesión." No "error 409".

## Dos caminos tras registrar

```js
if (data.signedIn) {
  // Supabase ya dejó sesión abierta → email de bienvenida en background + al panel
  fetch('/api/auth/welcome-email', { method:'POST', keepalive:true, body: JSON.stringify({ locale }) });
  window.location.href = '/dashboard';
} else {
  // Hace falta confirmar el email → mensaje de éxito, NO redirige
  setSuccess('Revisa tu email para confirmar tu cuenta y luego inicia sesión.');
}
```

`keepalive: true` en el fetch del email: si no, la navegación cancela la petición
y el usuario nunca recibe la bienvenida.

## Mensaje de éxito

```css
.msg-ok { padding: 10px 14px; border-radius: 11px; font-size: 13px;
          background: var(--brand-soft); border: 1px solid var(--brand-border); color: var(--brand); }
```

Sin `shake` — el éxito no vibra. Y sin icono: el color y el texto bastan.

## Adaptación al clipping

Registro **por invitación del creador** (el caso normal): el enlace trae `?invite=<token>`.
Entonces la tarjeta muestra, encima del título, un `.sub` discontinuo con quién invita:

```html
<div class="sub" style="display:flex;align-items:center;gap:10px;margin-bottom:18px">
  <img class="avatar" src="…" width="32" height="32" alt="">
  <div>
    <b style="font-size:12.5px">Te invita @nombre_del_creador</b>
    <p style="font-size:11.5px;color:var(--text-secondary)">Community de clipping · 240 clippers</p>
  </div>
</div>
```

Y un tercer paso opcional tras crear la cuenta: **elegir rol** (`clipper` / `creador`),
en dos cards de cristal seleccionables dentro de la misma tarjeta, sin cambiar de página.
La card seleccionada se marca con `border-color: var(--hover-border)` +
`box-shadow: var(--hover-shadow)`, **no** con el color de marca.
