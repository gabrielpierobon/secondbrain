# tracking/ — Cómo definir tu estructura

*No copies la estructura de otro. La carpeta `tracking/` debe reflejar las áreas reales de tu trabajo, no las de alguien más.*

---

## Archivos que ya existen (no borrar)

| Archivo | Para qué sirve |
|---------|----------------|
| `dashboard.md` | Resumen maestro de todo lo activo. Claude lo lee en cada sesión. |
| `memory.md` | Decisiones y contexto táctico acumulado sesión a sesión. |
| `daily/` | Un archivo por día. Claude los crea y actualiza automáticamente. |

---

## Cómo definir tus subcarpetas

Antes de crear carpetas, respondé:

**¿Cuáles son las 3–5 áreas de trabajo que más demandan tu atención?**

Cada área que tenga múltiples hilos activos simultáneos merece su propia carpeta. Las que tienen un solo hilo pueden ir directo en `dashboard.md`.

Ejemplos de estructuras posibles (elegí la que se adapte a vos):

```
# Si trabajás con clientes externos
tracking/
├── clientes/
│   ├── [nombre-cliente-1].md
│   └── [nombre-cliente-2].md
└── proyectos-internos/

# Si trabajás por proyectos
tracking/
├── proyectos/
│   ├── [proyecto-1].md
│   └── [proyecto-2].md
└── personal/

# Si trabajás por áreas funcionales
tracking/
├── ventas/
├── operaciones/
└── equipo/
```

No hay una estructura correcta. La correcta es la que refleja cómo organizás tu trabajo mentalmente.

---

## Formato de cada archivo de área

Cada archivo dentro de tus subcarpetas puede seguir esta estructura:

```markdown
# [Nombre del área / cliente / proyecto]

## Hilo 1 — [Nombre descriptivo]
**Estado:** 🔵 Activo / ⏳ Standby / ✅ Cerrado / ⏸️ En pausa
**Responsable actual:** [nombre]
**Último update:** [fecha]
**Próxima acción:** [qué · quién · cuándo]

---

## Hilo 2 — [Nombre descriptivo]
...
```

Claude entiende este formato y lo actualiza automáticamente.

---

## Cuándo crear un nuevo archivo

- Cuando un área tiene 3 o más hilos activos simultáneos
- Cuando necesitás historial de algo a lo largo del tiempo
- Cuando `dashboard.md` se empieza a sentir largo

Si dudás, empezá todo en `dashboard.md` y separá cuando el volumen lo justifique.
