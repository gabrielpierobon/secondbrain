---
name: tracking-management
description: Leer, actualizar y consolidar el sistema de tracking. Usar cuando hay que actualizar estados, crear dailies o procesar updates.
---

# Skill: Tracking Management

## Archivos y su función

| Archivo | Qué contiene | Cuándo se actualiza |
|---------|--------------|---------------------|
| `tracking/dashboard.md` | Resumen maestro de todos los hilos activos | Cada sesión |
| `tracking/memory.md` | Decisiones tácticas y contexto acumulado | Cierre de sesión + weekly review |
| `tracking/daily/YYYY-MM-DD.md` | Log del día | Cada sesión |
| `tracking/[tus carpetas]/*.md` | Hilos por área de trabajo | Cada update relevante |

## Estructura de hilo estándar

```markdown
### Hilo — [Nombre descriptivo]
**Estado:** 🔵 Activo / ⏳ Standby / ✅ Cerrado / ⏸️ En pausa
**Responsable actual:** [nombre]
**Último update:** [fecha]
**Próxima acción:** [qué · quién · cuándo]
```

## Reglas de mapeo de updates verbales

| Update del usuario | Acción en tracking |
|--------------------|--------------------|
| "ya está hecho" / "lo cerré" | ✅ con fecha de hoy |
| "esperando a X" | ⏳ con nombre explícito de quién tiene la pelota |
| "lo retomo la semana que viene" | Diferir con fecha aproximada |
| "suspendido" / "en pausa" | ⏸️ con motivo si lo hay |
| "nuevo proyecto / cliente / tema" | Crear hilo nuevo en el archivo correspondiente |

## Detección automática de problemas

| Patrón | Señal | Acción |
|--------|-------|--------|
| Standby vencido | Estado ⏳ + último update > 3 días hábiles sin movimiento | Alertar con nombre del responsable y días transcurridos |
| Hilo sin próxima acción | Ítem activo sin acción o responsable definido | Marcar como incompleto |
| Discrepancia | Estado distinto entre dashboard y archivo de área | El archivo de área gana — actualizar dashboard |

## Regla para decisiones importantes

Cuando en sesión se toma una decisión significativa (estratégica, de pricing, de relación, de go/no-go), registrarla en el daily con el prefijo `[DECISIÓN]` para que el weekly review la encuentre fácilmente.

## Regla de cierre de sesión

Antes de cerrar, verificar si hubo:
- Decisiones no registradas → agregar a `tracking/memory.md`
- Cambios de estado no volcados → actualizar archivos correspondientes
- Daily del día sin cerrar → completarlo con estado final
