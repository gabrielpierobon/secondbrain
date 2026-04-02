# daily/ — Archivos de log diario

*Claude crea y actualiza estos archivos automáticamente. No hace falta crearlos a mano.*

---

## Convención de nombres

```
YYYY-MM-DD.md
```

Ejemplos: `2026-04-02.md`, `2026-04-03.md`

---

## Qué contiene cada daily

Claude genera el contenido automáticamente durante la sesión. Típicamente incluye:

- Resumen ejecutivo del día
- Updates procesados (email, ntfy, verbales)
- Decisiones tomadas — marcadas con `[DECISIÓN]` para que el weekly review las encuentre fácil
- Comunicaciones enviadas o descartadas
- Estado final al cierre de sesión

---

## Para qué sirve el historial

Los dailies son la materia prima del weekly review. Claude los lee para:
- Identificar patrones que merezcan ir a `BRAIN.md`
- Promover decisiones importantes a `memory.md`
- Generar el resumen semanal

No borrar dailies antiguos — son la memoria del sistema.
