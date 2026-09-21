# Mi horario de clase

Tablero semanal de clases en una sola página HTML. Sin build, sin dependencias, sin servidor: se abre `index.html` en el navegador y funciona.

## Qué hace

- **Rejilla semanal** de lunes a viernes. El sábado y el domingo aparecen solos si pones clases en esos días.
- **Clases solapadas** repartidas en columnas automáticamente, sin que se tapen entre sí.
- **Línea de hora actual** sobre la columna del día de hoy, con la hora marcada en el margen. Se refresca cada minuto.
- **Franja de estado** arriba: la clase en curso con los minutos que le quedan, y la siguiente clase de la semana.
- **Edición en la propia página**: botón *Añadir clase*, o clic sobre cualquier bloque para editarlo o eliminarlo.
- **Impresión** con los colores intactos (`print-color-adjust: exact`).
- **Contadores**: número de clases, horas semanales y materias distintas.
- **Tema claro y oscuro**, siguiendo la preferencia del sistema.

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
2. **Almacenamiento compartido del artifact** (capacidad `db` de claude.ai), en el documento `horario/semana`. Solo se activa cuando la página se abre publicada como artifact; entonces los cambios sobreviven a cambios de navegador o dispositivo.

El indicador en la esquina inferior derecha dice cuál de los dos está en uso: *Guardado en la nube* o *Solo en este navegador*.

Al abrir la página por primera vez se muestra un **horario de ejemplo**, señalado con un aviso y un botón *Empezar de cero*. En cuanto se edita o se borra cualquier cosa, el ejemplo desaparece y ya no vuelve.

## Uso

Abrir el archivo directamente:

```
index.html
```

O servirlo en local, si se prefiere una URL:

```bash
python -m http.server 8000
# http://localhost:8000
```

Publicado como artifact: <https://claude.ai/artifact/UHUkVQmPrDbUJH3zHJbXKX>

## Estructura

Todo vive en `index.html`: estilos, marcado y lógica.

- **Tokens CSS** en `:root`, redefinidos para tema oscuro en `@media (prefers-color-scheme: dark)` y en `:root[data-theme="dark"]`. Los 8 colores de clase son tríos de tokens (`--cN-bg`, `--cN-fg`, `--cN-rail`).
- **`pack()`** — reparto en columnas de las clases que se solapan: agrupa por bloques contiguos y asigna la primera columna libre.
- **`bounds()`** — calcula el rango horario visible a partir de las clases, con un mínimo de 07:00–14:00.
- **`render()`** — dibuja la rejilla completa; `ROW_H` (58 px) es la altura de una hora.
- **`persist()`** — escribe en `localStorage` y encola la escritura en `db`.

## Tipografías

Cargadas desde Google Fonts:

- **Barlow Condensed** — títulos, cabeceras de día y nombres de materia.
- **IBM Plex Sans** — interfaz y texto corrido.
- **IBM Plex Mono** — horas y cifras, con `tabular-nums`.

Sin conexión, cada una cae a su pila de reserva y la página sigue siendo legible.
