# 07 · Incentivos y premios

Los botes de la semana. Una card por incentivo, con su regla, su bote y quién va ganando.

```
[kick] PREMIOS
[page-title] Lo que hay en juego esta semana.
[page-desc] Se paga el lunes por transferencia o PayPal.   [pill-live · BOTE TOTAL 600 €]

grid auto-fit minmax(300px, 1fr) · gap 16

┌─ .glass · Clip más viral ─────────────┐ ┌─ .glass · Más clips subidos ────────┐
│ [ic]  Clip más viral        [pill 1º] │ │ [ic]  Más clips subidos             │
│                                       │ │                                     │
│ 200 €                                 │ │ 200 €                               │
│ al clip con más views de la semana    │ │ a quien más clips apruebe           │
│                                       │ │                                     │
│ ┌ .sub dashed · VA GANANDO ─────────┐ │ │ ┌ .sub dashed · VA GANANDO ───────┐ │
│ │ ▣ @nombre · 1.24M views           │ │ │ │ ▣ @nombre · 24 clips            │ │
│ │ ──── barra de progreso ────       │ │ │ │ 2º @otro · 18 (te faltan 7)     │ │
│ └───────────────────────────────────┘ │ │ └─────────────────────────────────┘ │
│                                       │ │                                     │
│ Cierra en 3d 04h 12m                  │ │ Cierra en 3d 04h 12m                │
└───────────────────────────────────────┘ └─────────────────────────────────────┘
```

## Anatomía de una card de incentivo

1. **Cabecera**: icono en cuadrado 32 px + nombre del premio + `.pill` con tu puesto
   en ese incentivo (o "No participas").
2. **Bote**: número 30 px 800 tracking −0.03em + unidad `€` en 18 px muted.
   Es el dato grande; no lleva color.
3. **Regla**: una frase, 12 px `--text-secondary`. Sin jerga y sin asteriscos.
4. **Quién va ganando**: `.sub` discontinua con avatar, handle y métrica.
   Si el que mira participa, se añade su distancia ("te faltan 7 clips") — esto es
   lo que hace que suban otro clip.
5. **Cuenta atrás**: 11 px muted, `tabular-nums`, se actualiza cada minuto (no cada segundo:
   un contador de segundos en una card es ruido).

## Barra de progreso

```css
.prog { height: 6px; border-radius: 999px; background: var(--bg-surface-2); overflow: hidden; }
.prog i { display:block; height:100%; border-radius:999px; background: var(--text-primary);
          transition: width .6s cubic-bezier(.22,.8,.28,1); }
```

Monocroma. Si un incentivo es "el tuyo" (vas primero), su barra puede ir en `--brand`
— y entonces la pill del bote total baja a `.pill` neutra.

## Estados

| Estado | Cómo se ve |
|---|---|
| Activo | card normal, cuenta atrás visible |
| Cerrado, sin pagar | `.chip` "Pendiente de pago", cuenta atrás → "Cerrado el 21 sep" |
| Pagado | `.chip` "Pagado", ganador fijado, card con `opacity: .8` |
| Próximo | card con **marching ants** y "Empieza el lunes" |

## Historial

Debajo, tabla de incentivos cerrados: semana · premio · ganador · importe · estado.
`tabular-nums` en importes, alineados a la derecha.
