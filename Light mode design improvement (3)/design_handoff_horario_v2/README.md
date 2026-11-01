# Handoff: Horario Docente v2 — Layout dos columnas

## Overview
Rediseño visual del horario docente con layout de **dos columnas** (horario semanal a la izquierda, sidebar con tareas/calendario/notas a la derecha). Estilo glassmorphism con cards semitransparentes sobre fondo blanco, sombras pronunciadas, hover con elevación, y modo dark/light.

## About the Design Files
Los archivos en este paquete son **prototipos de diseño en HTML** — referencias visuales para recrear en el proyecto existente. La tarea es **adaptar estos diseños al código actual del repo** (`HEO-80/mihorariodeclase`), no copiar el HTML directamente. El proyecto ya tiene un `index.html` con el diseño anterior — **NO borrar ese archivo**, dejarlo accesible desde un botón/enlace para poder volver a él.

## Fidelity
**High-fidelity (hifi)**: Colores, tipografía, espaciado, sombras, hovers e interacciones son finales. Recrear pixel-perfect.

## Instrucción importante: Preservar la versión anterior
- El `index.html` actual del repo NO se borra
- Añadir un botón/enlace en la nueva versión que diga "Vista clásica" y lleve al `index.html` original (o renombrarlo a `clasico.html`)
- La nueva versión puede ser el nuevo `index.html` o un `v2.html` con redirect, como prefieras

## Estructura de la página

### Topbar (nav sticky)
- **Fondo**: `var(--card)` con `border-bottom: 1px solid var(--line)` y `box-shadow: 0 1px 4px rgba(0,0,0,.04)`
- **Logo**: icono redondo verde (`border-radius: 12px`, `background: #0D9373`, 40×40px) + título "Horario Docente · Héctor" + badge "Curso 26–27"
- **Tabs**: Profesor | Bachelor | Agenda Mes — botones con fondo `var(--card2)`, tab activo con sombra
- **Acciones**: Botón tema 🌙/☀️, Imprimir, Nueva Clase (botón accent verde)
- **flex-wrap: wrap** para responsive

### Status Strip (3 cards horizontales)
1. **EN DIRECTO · AHORA** — card con borde izquierdo verde, punto animado (pulse), muestra clase actual o "Tiempo Libre"
2. **A CONTINUACIÓN** — gradient verde `linear-gradient(135deg, #0D9373, #0AAB7E)`, texto blanco, siguiente clase
3. **RESUMEN SEMANAL** — horas lectivas (número grande mono), módulos, bloques activos, tareas

### Layout principal: Grid 2 columnas
```css
grid-template-columns: 1fr 360px;
gap: 16px;
padding: 16px 24px 32px;
```

#### Columna izquierda: Horario Semanal
- **Card** con `border-radius: 14px`, sombra `0 2px 8px rgba(0,0,0,.06), 0 8px 24px -6px rgba(0,0,0,.1)`
- **Cabecera**: "📋 Horario Semanal Lectivo" + subtítulo semana + filtros (chips: Todos/ASIR/SMR/DAM/Ciberseguridad)
- **Tabla**: `<table>` con columna de horas a la izquierda, días en cabecera
  - Cabecera días: `JetBrains Mono`, 11px, uppercase, día actual en verde accent
  - Bloques de clase: cards con `border-radius: 7px`, rail lateral 3px de color, hover con `translateY(-1px)` + sombra
  - Fila de recreo: centrada, monospace, uppercase, fondo `var(--card2)`
  - Clases continuas: texto italic "Continúa [nombre]"
- **Footer**: "Sincronizado con base de datos institucional" + enlace "Restaurar horario oficial"

#### Columna derecha: Sidebar (360px)
3 cards apiladas:

1. **Tareas & Evaluaciones**
   - Filtros: Pendientes / Hechas
   - Input de nueva tarea + select módulo + date + botón verde "＋"
   - Lista de tareas con checkbox, tag de color del módulo, fecha vencimiento
   - Hover: `translateX(3px)` + fondo blanco translúcido
   - Footer: "🔔 Sin pendientes urgentes" + badge "Al día"

2. **Calendario mini**
   - Cabecera: mes/año + navegación ◀ Hoy ▶
   - Grid 7 columnas (L M X J V S D), domingos en rojo
   - Día actual: círculo verde, hover: `scale(1.1)` + tinte verde
   - Días fuera del mes: opacity `.4`

3. **Notas del Docente**
   - Textarea con `border-radius: 10px`, fondo blanco translúcido
   - Autoguardado en localStorage
   - Footer con "Guardado en este navegador" + enlace "Limpiar"

## Estilo visual: Cards glassmorphism
Todas las cards principales:
```css
background: rgba(230, 233, 239, 0.6);
backdrop-filter: blur(8px);
-webkit-backdrop-filter: blur(8px);
border: 1px solid var(--line);
border-radius: 14px;
box-shadow: 0 2px 8px rgba(0,0,0,.06), 0 8px 24px -6px rgba(0,0,0,.1);
```
Hover:
```css
box-shadow: 0 4px 16px rgba(0,0,0,.08), 0 16px 40px -10px rgba(0,0,0,.14);
transform: translateY(-2px);
transition: box-shadow .2s, transform .15s;
```

## Design Tokens

### Colores Light
| Token | Valor | Uso |
|-------|-------|-----|
| --bg | #FFFFFF | Fondo página |
| --card | #F0F2F6 | Cards |
| --card2 | #E8EAEF | Sub-superficies |
| --ink | #1A1F2B | Texto principal |
| --ink2 | #4A5568 | Texto secundario |
| --ink3 | #8896A6 | Texto terciario |
| --line | #E2E6EC | Bordes |
| --line2 | #EDF0F4 | Bordes suaves |
| --accent | #0D9373 | Acento verde |
| --accent-soft | #E6F5F0 | Fondo accent |
| --accent-fg | #065F4A | Texto sobre accent |
| --warn | #E8890C | Warning |
| --danger | #DC3545 | Danger/rojo |

### Colores Dark
| Token | Valor |
|-------|-------|
| --bg | #0E1117 |
| --card | #161B26 |
| --card2 | #1C2230 |
| --ink | #E4E9F0 |
| --ink2 | #8C97A5 |
| --ink3 | #5A6574 |
| --line | #262E3A |
| --accent | #2DD4A8 |

### Colores de módulos (8 pares bg/fg/rail)
```
c1: #E3EDFB / #1B3B5E / #4A86CC — Azul (Ofimáticas, Seguridad)
c2: #D8EDE9 / #15443C / #34957F — Verde (Digitalización)
c3: #FBF0DB / #593C14 / #C5922E — Ámbar (Acceso Datos)
c4: #FBE2E5 / #5C2027 / #CC5A6B — Rosa (PSP)
c5: #E8E4F8 / #312860 / #7E70CC — Púrpura (SAD, Sostenibilidad)
c6: #E0EDD6 / #2B4416 / #6BA33E — Verde (SRI, ASO, IAW)
c7: #E2E6EC / #28313B / #7888A0 — Gris (SOM)
c8: #FDE5D8 / #5F311E / #D47A4A — Naranja (Ciberseguridad)
```

### Tipografía
- **Principal**: `DM Sans` 400/500/600/700
- **Monospace**: `JetBrains Mono` 400/500/600 — para horas, badges, etiquetas, stats
- **Tamaños**: títulos 22px/18px, cuerpo 14px, labels 10-11px uppercase con letter-spacing

### Espaciado
- Padding cards: 18-22px
- Gap entre cards sidebar: 16px
- Border-radius cards: 14px, bloques/inputs: 7-10px
- Sombra normal: `0 2px 8px rgba(0,0,0,.06), 0 8px 24px -6px rgba(0,0,0,.1)`
- Sombra hover: `0 4px 16px rgba(0,0,0,.08), 0 16px 40px -10px rgba(0,0,0,.14)`

## Interacciones & Comportamiento

### Hovers y transiciones
- **Cards principales**: elevación -2px + sombra ampliada (0.2s ease)
- **Bloques horario**: `translateY(-1px)` + sombra (0.12s)
- **Tareas**: `translateX(3px)` + fondo blanco (0.12s)
- **Días calendario**: `scale(1.1)` + tinte verde (0.12s)
- **Botones**: `scale(0.97)` al click
- **Inputs/textarea**: borde accent al focus + sombra ring
- **Chips/filtros**: transición color/bg 0.12s

### Funcionalidad
- **Tema dark/light**: toggle con `data-theme` en `<html>`, guardado en localStorage (`theme-pref`)
- **Datos**: mismos localStorage keys que el index.html actual (`horario-hector-v2`, `horario-tareas-v1`, `horario-notas-v1`)
- **Filtros horario**: por grupo (ASIR/SMR/DAM/Ciberseguridad/Todos)
- **Filtros tareas**: Pendientes/Hechas
- **Click en bloque**: abre dialog de edición
- **Autoguardado notas**: debounce 500ms
- **Reloj status**: se actualiza cada minuto

### Dialog de edición
- `<dialog>` con backdrop blur, border-radius 10px, sombra grande
- Campos: módulo, profesor, aula, día (select), hora inicio/fin, color (swatches)
- Validación inline

## Responsive
- `@media (max-width: 1100px)`: grid 1 columna, sidebar debajo
- `@media (max-width: 700px)`: status strip 1 columna

## localStorage Keys (NO cambiar, compartidos con versión anterior)
- `horario-hector-v2` — clases
- `horario-tareas-v1` — tareas
- `horario-notas-v1` — notas
- `theme-pref` — tema dark/light

## Files
- `v2.html` — Diseño completo de referencia (HTML+CSS+JS en un solo archivo)
