# Second Brain con Claude — Plantilla base

Este sistema es una extensión de tu memoria de trabajo. Registra lo que pasa en tu ámbito laboral o productivo — decisiones, conversaciones, bloqueos, tareas pendientes — y le da a Claude el contexto necesario para ayudarte de verdad: con seguimiento real, sin repetir lo que ya sabés, sin perder el hilo entre sesiones.

**Su utilidad depende directamente de la frecuencia con la que lo usás.** Un segundo cerebro que solo se abre una vez por semana es poco más que un repositorio. Uno que se consulta y actualiza todos los días se vuelve un colaborador que conoce tu trabajo tan bien como vos.

La curva es corta: en la primera semana el sistema empieza a ser útil, en el primer mes ya tiene contexto real, en tres meses es difícil trabajar sin él.

---

## Cómo encajan los archivos entre sí

El sistema tiene capas. Cada archivo tiene un rol específico y se relaciona con los demás de una forma concreta.

```
┌─────────────────────────────────────────────────────────────────┐
│                        CADA SESIÓN                              │
│                                                                 │
│   Claude lee esto al arrancar:                                  │
│                                                                 │
│   ┌──────────────┐   ┌──────────────┐   ┌───────────────────┐  │
│   │  BRAIN.md    │   │  memory.md   │   │  tracking/        │  │
│   │              │   │              │   │                   │  │
│   │ Quién sos.   │   │ Qué decidiste│   │ Qué está pasando  │  │
│   │ Cómo trabajás│   │ en sesiones  │   │ ahora mismo:      │  │
│   │ Qué priorizás│   │ anteriores.  │   │ proyectos, hilos, │  │
│   │ Reglas fijas │   │ Qué aprendiste│  │ bloqueos, dailies │  │
│   │ que no cambian│  │ Se actualiza  │  │ Se actualiza cada │  │
│   │              │   │ al cerrar    │   │ sesión            │  │
│   └──────┬───────┘   └──────┬───────┘   └────────┬──────────┘  │
│          │                  │                    │              │
│          └──────────────────┴────────────────────┘              │
│                             │                                   │
│                             ▼                                   │
│                    ┌────────────────┐                           │
│                    │   CLAUDE.md    │                           │
│                    │                │                           │
│                    │ El motor.      │                           │
│                    │ Define qué     │                           │
│                    │ hace Claude en │                           │
│                    │ cada sesión,   │                           │
│                    │ en qué orden,  │                           │
│                    │ y qué agente   │                           │
│                    │ usar según la  │                           │
│                    │ tarea          │                           │
│                    └───────┬────────┘                           │
│                            │                                    │
│              ┌─────────────┼─────────────┐                     │
│              ▼             ▼             ▼                      │
│        ┌──────────┐  ┌──────────┐  ┌──────────┐               │
│        │ Agent A  │  │ Agent B  │  │ Agent C  │               │
│        │          │  │          │  │          │               │
│        │ Un rol   │  │ Un rol   │  │ Un rol   │               │
│        │ específico│ │ específico│ │ específico│              │
│        │ (tracker,│  │(propuestas│ │(comms,   │               │
│        │ research…│  │ research…)│ │ agenda…) │               │
│        └────┬─────┘  └────┬─────┘  └────┬─────┘               │
│             │             │             │                       │
│             ▼             ▼             ▼                       │
│        ┌─────────────────────────────────────┐                 │
│        │              Skills                 │                 │
│        │                                     │                 │
│        │  Instrucciones de ejecución para     │                 │
│        │  tareas concretas: formato, reglas,  │                 │
│        │  criterios de calidad, pasos exactos │                 │
│        └─────────────────────────────────────┘                 │
└─────────────────────────────────────────────────────────────────┘
```

### Qué hace cada pieza

| Archivo | Rol | Cuándo se lee | Cuándo se escribe |
|---------|-----|---------------|-------------------|
| `BRAIN.md` | Tu identidad y reglas de decisión permanentes | Al inicio de cada sesión | Solo cuando cambian tus prioridades o forma de trabajar |
| `CLAUDE.md` | El motor: rutina de sesión + tabla de despacho a agentes | Al inicio de cada sesión | Rara vez — cuando cambia la rutina o agregás agentes |
| `tracking/memory.md` | Decisiones tácticas y aprendizajes acumulados entre sesiones | Al inicio de cada sesión | Al cerrar sesión y en la revisión semanal |
| `tracking/dashboard.md` | Estado maestro de todo lo activo ahora mismo | Al inicio de cada sesión | Cada sesión — es el más dinámico |
| `tracking/daily/YYYY-MM-DD.md` | Log del día: qué pasó, qué se decidió, qué quedó pendiente | Solo el del día en curso | Durante y al final de cada sesión |
| `.claude/agents/*.md` | Rol especializado para un tipo de tarea | Cuando CLAUDE.md lo despacha | Cuando refinás cómo opera ese agente |
| `.claude/skills/*.md` | Instrucciones de ejecución: formatos, pasos, criterios | Cuando el agente correspondiente lo necesita | Cuando querés mejorar la calidad de una tarea específica |

**El flujo en una sesión normal:**
1. Claude lee `BRAIN.md` → `memory.md` → `tracking/` (contexto completo)
2. `CLAUDE.md` define qué hacer: resumen del día, updates pendientes, comunicaciones
3. Si aparece una tarea específica (ej. redactar propuesta), `CLAUDE.md` despacha al agente correcto
4. El agente carga su skill y ejecuta con ese contexto
5. Al cerrar: `tracking/` actualizado, `memory.md` con lo importante del día

---

## ¿Con qué herramienta usarlo?

Este repositorio está diseñado para usarse con un **coding AI** que pueda leer y editar archivos directamente en tu máquina:

- **[Claude Code](https://claude.ai/code)** — la opción nativa de Anthropic, ideal para este tipo de sistemas basados en ficheros
- **[Cursor](https://cursor.com)** — editor de código con agente IA integrado, muy efectivo para gestionar el repo en el día a día

También funciona dentro de **Claude Desktop** si tenés habilitado el acceso a herramientas de ficheros y usás el proyecto como workspace de contexto.

La diferencia práctica: con un coding AI el agente edita los archivos directamente en tu repositorio. Con Claude Desktop sin herramientas, el flujo es más manual (copiás y pegás el contenido).

### Conectar herramientas externas con MCP

Para que el sistema pueda interactuar con el mundo exterior — leer emails, buscar en la web, procesar documentos, enviar notificaciones — se recomienda conectar un **MCP server** (Model Context Protocol).

MCP es el protocolo estándar de Anthropic para que Claude acceda a herramientas externas de forma controlada. Algunos ejemplos de lo que podés conectar:

| Herramienta | Para qué sirve en este sistema |
|-------------|-------------------------------|
| Gmail / Outlook | Leer emails con updates, generar borradores de respuesta |
| Búsqueda web | Investigación, verificación de datos, contexto de mercado |
| Google Drive / OneDrive | Leer documentos, propuestas, presentaciones |
| OCR | Extraer texto de imágenes o PDFs escaneados |
| Slack / Teams | Leer mensajes relevantes, preparar respuestas |
| Notificaciones push (ntfy, Telegram, etc.) | Recibir resúmenes en el móvil |

Para explorar los MCP servers disponibles: [mcp.so](https://mcp.so) o el [repositorio oficial de Anthropic](https://github.com/anthropics/anthropic-mcp-servers).

El sistema funciona sin MCP — pero con herramientas conectadas pasa de ser un gestor de notas a un agente que actúa sobre tu entorno real.

---

## Base técnica: markdown puro

Este repositorio está construido íntegramente sobre ficheros markdown. Toda la identidad, el tracking, los agentes y los skills son archivos de texto plano que Claude lee directamente en contexto. No hay infraestructura adicional requerida para empezar.

**Limitación conocida y evolución futura**

A medida que el segundo cerebro crece — más dailies, más hilos, más decisiones acumuladas — leer todos los archivos en cada sesión empieza a ser ineficiente. El contexto de Claude tiene un límite, y cargar cientos de archivos no escala bien.

La evolución natural de este sistema es **indexar y vectorizar el conocimiento**: construir una base de búsqueda semántica sobre los archivos de tracking para que el agente pueda recuperar información relevante sin necesitar leerlo todo. Esto permitiría que el segundo cerebro crezca indefinidamente sin degradar la calidad de las sesiones.

Esa capacidad — RAG sobre el propio tracking — es una **feature futura** de este template. La arquitectura de ficheros markdown que usás hoy es compatible con esa evolución: los mismos archivos que leés manualmente son los que se indexarían.

Por ahora, el sistema funciona muy bien con markdown puro para la mayoría de los casos de uso. Cuando el volumen de información lo justifique, la capa de vectorización se agrega encima sin cambiar la estructura base.

---

## Cómo funciona

Claude lee tus archivos de contexto al inicio de cada sesión, sigue una rutina definida, actualiza el tracking automáticamente y (opcionalmente) te notifica al móvil. Cuanto más lo usás, más contexto acumula.

El sistema tiene tres capas:

1. **Identidad** — quién sos, cómo trabajás, qué priorizás (`BRAIN.md`)
2. **Motor** — qué hace Claude en cada sesión y cómo despacha tareas (`CLAUDE.md`)
3. **Memoria** — el estado del mundo: qué está pasando, qué decidiste, qué aprendiste (`tracking/`)

---

## Antes de tocar un solo archivo: el assessment inicial

El error más común es clonar esto y empezar a llenar archivos sin tener claro qué querés que el sistema haga por vos.

**Hacete estas preguntas primero:**

- ¿Qué tipo de trabajo hacés? ¿Cuáles son las 3-5 áreas que más demandan tu atención?
- ¿Qué es lo que más se te escapa o se te acumula hoy? (emails, propuestas, seguimientos, tareas del equipo, etc.)
- ¿Cuándo arranca y termina tu día laboral?
- ¿Qué canales usás para recibir información? (email, Slack, WhatsApp, etc.)
- ¿Qué querés que Claude haga solo, sin que lo pidas?

Con esas respuestas, podés poblar `BRAIN.md` con contexto real desde el día 1.

---

## Estructura del repositorio

```
/
├── BRAIN.md                  → Identidad, rol y reglas de decisión
├── CLAUDE.md                 → Rutina de sesión + tabla de despacho a agentes
│
├── tracking/
│   ├── README.md             → Cómo definir TU estructura de tracking
│   ├── dashboard.md          → Resumen maestro de lo activo
│   ├── memory.md             → Contexto táctico acumulado sesión a sesión
│   └── daily/
│       └── README.md         → Formato y convención de los archivos diarios
│
└── .claude/
    ├── agents/
    │   └── tracker.md        → Ejemplo de agente (gestión de tracking)
    └── skills/
        └── tracking-management/
            └── SKILL.md      → Ejemplo de skill
```

**La carpeta `tracking/` no tiene estructura predefinida** — salvo `dashboard.md`, `memory.md` y `daily/`. Leé `tracking/README.md` antes de crear subcarpetas.

---

## Cómo arrancar

### 1. Clonar y abrir en Cursor

```bash
git clone <este-repo> mi-segundo-cerebro
cd mi-segundo-cerebro
```

Abrir la carpeta con Cursor (u otro IDE con Claude).

### 2. Completar BRAIN.md

Es el primer archivo que Claude lee en cada sesión. Define el filtro de todo lo demás. Sin esto, el sistema opera genéricamente.

Seguir las instrucciones dentro del archivo.

### 3. Completar CLAUDE.md

Reemplazar todos los `[PLACEHOLDER]` con tus datos reales. Los más importantes:
- Tu nombre
- El canal de notificaciones push que elegiste (ntfy, Telegram, Slack, etc.) y el comando para enviar mensajes
- Los canales por donde te llega información
- El idioma y tono que querés

### 4. Definir tu estructura de tracking

Leer `tracking/README.md`. Crear una subcarpeta por cada área de trabajo real. No usar las carpetas de otra persona como referencia — las tuyas deben reflejar tu contexto.

### 5. Primera sesión

Abrir una conversación en Cursor y escribir:

```
Leé BRAIN.md y CLAUDE.md. Después ejecutá la rutina de inicio de sesión.
```

Claude va a leer el contexto, identificar qué falta, y pedirte lo que necesita para arrancar.

---

## Cómo expandir el sistema

Cuando tengas un tipo de tarea recurrente que Claude hace de forma genérica y querés que lo haga mejor:

1. Crear un archivo en `.claude/agents/` describiendo el rol específico
2. Crear un skill en `.claude/skills/` con el formato y las reglas de ejecución
3. Agregar una entrada en la tabla de despacho de `CLAUDE.md`

Ejemplo: si manejás muchas propuestas → crear `agents/proposals.md` + `skills/proposal-drafting/SKILL.md`.

---

## Tareas programadas (opcional — para que el sistema sea proactivo)

Por defecto, Claude solo actúa cuando vos se lo pedís. Las tareas programadas son lo que le dan **proactividad**: el sistema hace cosas solo, sin que tengas que iniciarlo.

Son completamente opcionales. Si no las configurás, el sistema sigue siendo útil — simplemente requiere que lo activés vos cada vez.

### Tareas sugeridas

| Tarea | Frecuencia sugerida | Qué hace |
|-------|---------------------|----------|
| Morning brief | Cada día hábil al inicio | Lee todo el contexto, genera resumen, notifica |
| Midday pulse | 1-2 veces durante el día | Revisa bloqueos, actualiza tracking |
| Weekly review | Una vez por semana | Consolida aprendizajes, actualiza BRAIN.md y memory.md |

Podés agregar o quitar tareas según tu rutina. Lo importante es que tengan una instrucción clara que Claude pueda ejecutar sin contexto adicional.

### Dos formas de implementarlas

**Opción A — Claude Desktop Scheduled Tasks** (más simple, sin código)
Configurar las tareas directamente en la interfaz de Claude Desktop. No requiere infraestructura. Limitación: solo corre mientras la computadora esté encendida.

**Opción B — Cron job** (más robusto)
Un script que se ejecuta en el horario definido y lanza Claude vía API (Anthropic SDK o Claude Code CLI). Válido si querés que corra en un servidor remoto o independientemente del escritorio.

En ambos casos, la instrucción que recibe Claude es la misma — lo que cambia es el mecanismo que la dispara.

### Pedile a tu coding agent que lo configure

No hace falta configurarlo a mano. Con Cursor (u otro coding agent), podés pedirle directamente:

```
Configurá las tareas programadas del sistema usando Claude Desktop Scheduled Tasks.
Las tareas son: morning brief diario a las 8:30, midday pulse a las 12h y 16h,
weekly review los viernes a las 15h. Las instrucciones de cada tarea están en CLAUDE.md.
```

O si preferís cron:

```
Creá un cron job que ejecute las tareas programadas del sistema.
Usá el Anthropic SDK para lanzar Claude con las instrucciones de CLAUDE.md.
Las tareas son: [lista]. El horario es: [horario].
```

El agente genera el código, lo configura y te explica cómo validar que funciona.

---

## Usá tu coding agent para configurar el repo

No hace falta completar estos archivos a mano. La forma más eficiente de arrancar es abrir el repo en Cursor (u otro IDE con Claude) y pedirle al agente que te guíe o que complete los archivos por vos, a partir de una conversación.

Por ejemplo:

```
Leé todos los archivos de este repo. Voy a contarte cómo trabajo y quiero que
completes BRAIN.md, definas mi estructura de tracking y configures CLAUDE.md
con mis datos. Empezá preguntándome lo que necesitás saber.
```

El agente va a hacerte las preguntas del assessment inicial, procesar tus respuestas y escribir los archivos directamente. Es mucho más rápido que leer cada placeholder y completarlo solo.

Lo mismo aplica para agregar agentes, skills y tareas programadas: describís qué querés y el agente lo implementa.

---

## Principios

- **Claude prepara, vos ejecutás.** Nada sale sin tu confirmación.
- **Todo es texto plano.** Podés leer, editar y versionar todo con git.
- **El sistema es tuyo.** No copies la estructura de otro — construí la que refleje tu trabajo real.
- **Empieza con poco.** Un agente, un skill. Agregás capas cuando el uso lo pida.
