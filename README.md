# 📅 Mi Horario de Clase

Tablero semanal de clases + seguimiento de asignaturas en una sola página HTML. Sin build, sin dependencias, sin servidor: se abre en el navegador y funciona.

> **🔗 Ver la página en vivo:** [https://heo-80.github.io/mihorariodeclase/](https://heo-80.github.io/mihorariodeclase/)

---

## Qué incluye

### 🗓️ Horario semanal (`index.html`)

- **Rejilla semanal** de lunes a viernes. El sábado y el domingo aparecen solos si pones clases en esos días.
- **Clases solapadas** repartidas en columnas automáticamente, sin que se tapen entre sí.
- **Línea de hora actual** sobre la columna del día de hoy, con la hora marcada en el margen. Se refresca cada minuto.
- **Franja de estado** arriba: la clase en curso con los minutos que le quedan, y la siguiente clase de la semana.
- **Edición en la propia página**: botón *Añadir clase*, o clic sobre cualquier bloque para editarlo o eliminarlo.
- **Impresión** con los colores intactos.
- **Contadores**: número de clases, horas semanales y materias distintas.
- **Tema claro y oscuro**, siguiendo la preferencia del sistema.

### 📚 Vista de asignaturas (`asignatura.html`)

Panel de seguimiento de cada módulo que imparto, con:

- **Temario** de cada asignatura organizado por unidades didácticas, con estado (done / current / pending)
- **Actividades y tareas** con tipo (tarea, práctica, proyecto, examen) y estado de evaluación
- **Fechas clave** de entregas y exámenes
- **Enlaces a materiales** (PDFs, DOCX, presentaciones)

Módulos incluidos: ASO, SRI, IAW, PSP, Acceso a Datos, Digitalización, Sostenibilidad, Ofimática, SOM.

### ✅ Panel de corrección (`correccion.html`)

Herramienta para corregir entregas de alumnos con rúbricas configurables.

---

## Páginas

| Página | Descripción |
|--------|-------------|
| [`index.html`](https://heo-80.github.io/mihorariodeclase/) | Horario semanal interactivo |
| [`asignatura.html`](https://heo-80.github.io/mihorariodeclase/asignatura.html) | Seguimiento de asignaturas, temario y tareas |
| [`clasico.html`](https://heo-80.github.io/mihorariodeclase/clasico.html) | Vista clásica del horario |
| [`correccion.html`](https://heo-80.github.io/mihorariodeclase/correccion.html) | Panel de corrección de entregas |

---

## Datos de una clase

| Campo | Obligatorio | Límite |
|---|---|---|
| Materia | sí | 60 caracteres |
| Profesor(a) | no | 40 caracteres |
| Aula | no | 24 caracteres |
| Día | sí | lunes a domingo |
| Entrada / Salida | sí | la salida debe ser posterior a la entrada |
| Color | sí | 8 colores (`c1`–`c8`) |

## Dónde se guardan los datos

Dos niveles, en este orden:

1. **`localStorage`** del navegador, bajo la clave `horario-clase-v1`. Siempre activo, es el respaldo.
2. **Almacenamiento compartido del artifact** (capacidad `db` de claude.ai), en el documento `horario/semana`. Solo se activa cuando la página se abre publicada como artifact.

El indicador en la esquina inferior derecha dice cuál de los dos está en uso: *Guardado en la nube* o *Solo en este navegador*.

## Uso

Abrir directamente en el navegador:

```
index.html
```

O servir en local:

```bash
python -m http.server 8000
# http://localhost:8000
```

## Estructura

Todo vive en archivos HTML independientes: estilos, marcado y lógica inline. Sin frameworks, sin bundlers.

- **Tokens CSS** en `:root`, redefinidos para tema oscuro en `@media (prefers-color-scheme: dark)` y en `:root[data-theme="dark"]`.
- Los 8 colores de clase son tríos de tokens (`--cN-bg`, `--cN-fg`, `--cN-rail`).

## Tipografías

Cargadas desde Google Fonts:

- **Barlow Condensed** — títulos, cabeceras de día y nombres de materia.
- **IBM Plex Sans** — interfaz y texto corrido.
- **IBM Plex Mono** — horas y cifras, con `tabular-nums`.

Sin conexión, cada una cae a su pila de reserva y la página sigue siendo legible.
