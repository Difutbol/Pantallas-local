# Difútbol · Board de Partidos del Día

Pantalla para TV (1920×1080, se ajusta sola a cualquier resolución) que muestra todos los partidos del día del calendario **FCF COMET** de difútbol, **una competición a la vez**, rotando automáticamente.

## Cómo funciona

```
Calendario "FCF COMET"  →  Apps Script (Web App, caché cada 10 min)  →  index.html en GitHub Pages  →  Pantallas
```

- **index.html**: la pantalla. Muestra 14 partidos por página (2 columnas × 7 filas). Si una competición tiene más, pasa por varias páginas y luego sigue con la siguiente competición.
- **apps-script/Code.gs**: lee el calendario y entrega los partidos agrupados por competición. Hace falta porque FCF COMET es un calendario *importado* y no se puede leer directo desde el navegador con una API Key.

Estados de cada partido (según la hora del calendario): **PROGRAMADO**, **EN JUEGO**, **FINALIZADO**.

## Estructura

```
difutbol-board/
├── index.html              ← la pantalla (GitHub Pages)
├── README.md
└── apps-script/
    ├── Code.gs             ← se pega en un proyecto NUEVO de Apps Script
    └── appsscript.json     ← zona horaria y permisos de la Web App
```

## Instalación

### 1. Apps Script (proyecto nuevo)

1. Entrar a <https://script.google.com> con la cuenta que tiene el calendario FCF COMET → **Nuevo proyecto**. Nombre: `Difutbol Board`.
2. Borrar el contenido de `Código.gs` y pegar todo el contenido de `apps-script/Code.gs`.
3. **Configuración del proyecto** (ícono de engranaje) → Zona horaria: **(GMT-05:00) Bogotá**.
4. Seleccionar la función `probarBoard` → **Ejecutar** → aceptar permisos. Revisar en el registro el total de partidos por competición.
5. Seleccionar la función `crearTriggerBoard` → **Ejecutar** (una sola vez). Refresca la caché cada 10 minutos.
6. **Implementar → Nueva implementación** → tipo **Aplicación web**:
   - Ejecutar como: **Yo**
   - Quién tiene acceso: **Cualquier persona**
7. Copiar la URL que termina en `/exec`.

### 2. GitHub

1. Crear un repositorio nuevo (ej. `difutbol-board`) y subir todos los archivos de esta carpeta.
2. En `index.html`, pegar la URL `/exec` en `WEB_APP_URL` (bloque `CONFIG`).
3. **Settings → Pages** → Source: *Deploy from a branch* → rama `main`, carpeta `/ (root)` → **Save**.
4. La pantalla queda en `https://<usuario>.github.io/difutbol-board/`.

Mientras `WEB_APP_URL` esté vacía, la pantalla muestra **datos de demostración**.

## Ajustes (bloque `CONFIG` de index.html)

| Opción | Por defecto | Qué hace |
|---|---|---|
| `SEGUNDOS_POR_PAGINA` | 10 | Tiempo de cada página en pantalla |
| `COLUMNAS` / `FILAS` | 2 / 7 | Partidos por página (14) |
| `REFRESCO_DATOS_MIN` | 5 | Cada cuánto pide datos nuevos |
| `RECARGA_COMPLETA_HORAS` | 6 | Recarga total para limpiar memoria del TV |

## Pruebas

- Ver otro día: agregar `?fecha=2026-10-11` al final del enlace de la pantalla.
- Forzar lectura del calendario (sin caché): abrir la URL `/exec?refrescar=1`.
- Si se cambia el código de Apps Script: **Implementar → Gestionar implementaciones → Editar → Nueva versión** (la URL `/exec` no cambia).
