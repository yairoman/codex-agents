# codex-agents

Repositorio de ejemplo para trabajar con **Codex en modo multiagente**, usando **Engram** como memoria persistente, **SDD híbrido inspirado en gentle-ai** y un conjunto de **skills locales** para estandarizar exploración, especificación, diseño, implementación, validación y revisión.

## Qué resuelve este repositorio

Este proyecto organiza el trabajo de Codex con cuatro piezas principales:

1. **Agentes especializados** definidos en `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents`
2. **Skills locales** definidas en `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/skills`
3. **Memoria persistente con Engram** para recordar decisiones, convenciones, bugs y resúmenes de sesión
4. **Artefactos versionados por cambio** en `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes`

La idea es producir cambios:

- pequeños cuando sea posible
- trazables cuando el riesgo lo justifique
- auditables
- consistentes con el repositorio
- fáciles de revisar
- con menos pérdida de contexto entre sesiones

---

## Instalación de Engram

Repositorio oficial: [Gentleman-Programming/engram](https://github.com/Gentleman-Programming/engram)

### Opción recomendada en macOS / Linux

```bash
brew install gentleman-programming/tap/engram
engram version
```

### Instalación desde el repositorio fuente

```bash
git clone https://github.com/Gentleman-Programming/engram.git
cd engram
go install ./cmd/engram
engram version
```

### Binarios precompilados

También puedes descargar el binario desde [GitHub Releases](https://github.com/Gentleman-Programming/engram/releases).

---

## Integración recomendada con Codex

Según la documentación oficial de Engram, la forma recomendada para Codex es:

```bash
engram setup codex
```

Ese comando realiza automáticamente estas tareas:

- registra `engram` como servidor MCP en `~/.codex/config.toml`
- crea `~/.codex/engram-instructions.md` con el protocolo de memoria
- crea `~/.codex/engram-compact-prompt.md` para recuperación después de compaction

### Configuración manual alternativa

Si prefieres hacerlo manualmente, tu `~/.codex/config.toml` debe incluir algo como:

```toml
model_instructions_file = "~/.codex/engram-instructions.md"
experimental_compact_prompt_file = "~/.codex/engram-compact-prompt.md"

[mcp_servers.engram]
command = "engram"
args = ["mcp"]
```

### Qué aporta Engram en Codex

Engram agrega memoria persistente para que el agente pueda:

- buscar contexto previo antes de trabajar
- guardar decisiones importantes y hallazgos útiles
- recordar convenciones del repositorio
- sobrevivir mejor a reinicios de contexto o compaction
- cerrar sesiones con un resumen reutilizable en sesiones futuras

---

## SDD híbrido inspirado en gentle-ai

Este repositorio adopta un enfoque **híbrido** de **Spec-Driven Development (SDD)** inspirado en [gentle-ai](https://github.com/Gentleman-Programming/gentle-ai):

- **no** obliga SDD completo para tareas triviales
- **sí** exige artefactos versionados cuando el cambio es mediano, riesgoso o multi-scope
- mantiene los agentes actuales, pero los hace operar sobre artefactos y `task-id`

### Cuándo se activa el gate SDD

El Orchestrator debe activar SDD cuando una tarea:

- toca más de un subsistema
- requiere paralelismo
- cambia contratos, interfaces o base de datos
- tiene riesgo de regresión o rollout
- requiere handoff entre agentes

### Cuándo se permite bypass

El Orchestrator puede evitar SDD completo cuando la tarea es:

- doc-only
- cambio local/trivial
- fix pequeño y de muy bajo riesgo

En esos casos, el Orchestrator debe **decir explícitamente** por qué hizo bypass.

---

## Artefactos versionados por cambio

Los cambios que pasan el gate SDD deben crear una carpeta así:

```text
specs/changes/<change-id>/
  proposal.md
  spec.md
  design.md
  tasks.md
  verify.md
  archive.md
```

### Qué representa cada artefacto

- `proposal.md` — intención, alcance, no-alcance, riesgos, unknowns
- `spec.md` — requisitos, escenarios y criterios de aceptación
- `design.md` — decisiones técnicas, ownership, dependencias y serialización
- `tasks.md` — lista implementable con `TASK-*`, owner y lane
- `verify.md` — validación contra requisitos y tareas
- `archive.md` — cierre final, follow-ups y aprendizajes archivables

También existe una guía en:

- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/README.md`

Y plantillas base en:

- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/_template/`

---

## Cómo está configurado este repositorio

### Multi-agent habilitado

En `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/config.toml` el repositorio publica el manifiesto oficial de Multi-agents para Codex CLI:

```toml
[features]
multi_agent = true

[agents]
max_depth = 1
max_threads = 3
job_max_runtime_seconds = 600

[agents.orchestrator]
description = "Orquestador principal del workflow multiagente; coordina spec, diseño, implementación, validación y memoria final."
config_file = "agents/orchestrator.toml"
```

Cada rol se define con `config_file` relativo hacia `.codex/agents/<rol>.toml`, y cada uno de esos archivos contiene el `model` y `developer_instructions` del agente.

Los archivos `.md` en `.codex/agents/` se mantienen como fuente editorial legible; los `.toml` del mismo directorio son el empaquetado ejecutable para Codex CLI.

> Nota: si tu instalación de Codex CLI no consume automáticamente el `.codex/config.toml` del repo, usa este archivo como base reusable e intégralo en `~/.codex/config.toml` o en un profile global.

### Agentes configurados para Codex CLI

| Agente | Propósito |
|---|---|
| Orchestrator | Decide gated vs bypass, asigna `change-id`, ownership, lanes, consolidación y memoria final |
| Explorer | Analiza el repositorio y alimenta `proposal.md` |
| Spec Writer | Produce especificaciones verificables y `spec.md` |
| Architect | Define `design.md`, ownership, merge order y serialización |
| Backend | Implementa tareas backend referenciando `TASK-*` |
| Frontend | Implementa tareas frontend referenciando `TASK-*` |
| Data SQL | Implementa tareas de DB/queries/migraciones referenciando `TASK-*` |
| Test | Valida contra `spec.md` y `tasks.md`, y actualiza `verify.md` |
| Reviewer | Revisa contra diff + artefactos SDD consolidados |
| DevOps | Evalúa impacto operativo en el resultado consolidado |

Los archivos ejecutables por rol están en:

- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/orchestrator.toml`
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/explorer.toml`
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/spec_writer.toml`
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/architect.toml`
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/backend.toml`
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/frontend.toml`
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/data_sql.toml`
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/test.toml`
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/reviewer.toml`
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/devops.toml`

### Flujo esperado de trabajo

Para cambios con SDD activo, el flujo es:

1. **Explorer**
2. **Spec Writer**
3. **Architect**
4. **Orchestrator** decide `change-id`, scope y `tasks.md`
5. **Backend / Frontend / Data SQL** ejecutan `TASK-*`
6. **Orchestrator** consolida
7. **Test / DevOps** validan sobre el resultado consolidado
8. **Reviewer** revisa el resultado consolidado
9. **Orchestrator** cierra `archive.md` y persiste memoria final

---

## Skills locales configuradas en este repositorio

Las skills locales disponibles en `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/skills` son:

| Skill | Uso principal |
|---|---|
| `repo-exploration` | exploración inicial y contexto reutilizable para `proposal.md` |
| `feature-spec` | especificación verificable y límites de scope |
| `parallel-orchestration` | paralelismo seguro por ownership y consolidación |
| `sdd-propose` | creación de `proposal.md` |
| `sdd-design` | diseño técnico para `design.md` |
| `sdd-tasks` | descomposición en `TASK-*` y lanes |
| `sdd-verify` | validación contra requisitos y tareas |
| `sdd-archive` | cierre del cambio y aprendizajes archivables |
| `safe-refactor` | modificar código sin cambiar comportamiento externo |
| `incident-debugging` | investigar y resolver bugs o incidentes |
| `sql-migration` | planear cambios de esquema o datos |
| `release-checklist` | revisar preparación de despliegue y riesgo operativo |

### Cómo trabajan juntos agentes + skills

- **Explorer** suele apoyarse en `repo-exploration`
- **Spec Writer** suele apoyarse en `feature-spec`
- **Orchestrator** puede usar `sdd-propose`, `sdd-tasks`, `sdd-archive` y `parallel-orchestration`
- **Architect** puede apoyarse en `sdd-design`
- **Test** puede apoyarse en `sdd-verify`
- **Backend / Frontend** pueden seguir usando `safe-refactor` dentro de tareas ya definidas
- **Data SQL** puede usar `sql-migration` cuando el cambio lo requiera
- **DevOps** puede complementar con `release-checklist`

En otras palabras:

- los **agentes** definen el **rol**
- las **skills** definen el **método de trabajo**
- los **artefactos SDD** definen el **estado del cambio**
- **Engram** conserva el **conocimiento duradero**

---

## Escenarios recomendados de uso

Esta configuración no busca reemplazar el prompting simple en todos los casos. Funciona mejor como un modelo **híbrido**: prompting directo para lo pequeño y multi-agent + SDD para lo que necesita más control.

### Cuándo conviene usar Multi-agent + SDD

Úsalo cuando el trabajo tenga una o varias de estas características:

- toca varios archivos, capas o subsistemas
- requiere coordinación entre backend, frontend, datos o devops
- necesita trazabilidad y artefactos versionados
- tiene riesgo de regresión, rollout o integración
- requiere continuidad entre sesiones o mantenimiento futuro
- necesita validación y revisión más formales

### Cuándo conviene usar prompting directo

Es mejor ir directo con prompting simple cuando la tarea sea:

- pequeña y local
- de bajo riesgo
- claramente definida desde el inicio
- un ajuste rápido en un solo archivo o alcance
- una pregunta, explicación o refactor menor

### Regla práctica

- **Cambios pequeños** → prompting directo
- **Cambios medianos o riesgosos** → Multi-agent + SDD
- **Cambios multi-scope o con handoff** → gated SDD casi siempre

### Tabla comparativa rápida

| Escenario | Prompting simple | Multi-agent sin SDD completo | Multi-agent + SDD |
|---|---|---|---|
| Cambio pequeño en un archivo | ✅ Mejor opción | ⚠️ Generalmente innecesario | ❌ Excesivo |
| Fix local de bajo riesgo | ✅ Mejor opción | ⚠️ Útil sólo si quieres una segunda validación | ❌ Normalmente no vale la pena |
| Refactor mediano en una capa | ⚠️ Posible, pero depende mucho del prompt | ✅ Buena opción | ⚠️ Sólo si hay riesgo adicional |
| Feature multi-capa | ❌ Riesgo de ambigüedad | ⚠️ Puede servir si el alcance ya está claro | ✅ Mejor opción |
| Cambio con backend + frontend + datos | ❌ Poco recomendable | ⚠️ Sólo si no cambia contratos | ✅ Recomendado |
| Cambio con riesgo operativo o rollout | ❌ No ideal | ⚠️ Parcial | ✅ Recomendado |
| Trabajo con continuidad entre sesiones | ⚠️ Posible, pero frágil | ✅ Aceptable | ✅ Mejor opción |
| Mantenimiento complejo o con handoff | ❌ No ideal | ⚠️ Parcial | ✅ Mejor opción |

---

## Ejemplos concretos: usa este modo para estos casos

### Usa **prompting simple** cuando

- quieras cambiar un texto o comentario en un solo archivo
- necesites ajustar un nombre, constante o mensaje sin impacto estructural
- quieras pedir una explicación rápida del repositorio o de un archivo
- hagas un fix pequeño, bien localizado y de muy bajo riesgo
- necesites una propuesta rápida antes de decidir si vale la pena activar SDD

### Usa **multi-agent sin SDD completo** cuando

- quieras exploración y apoyo de roles, pero sin abrir todo el proceso formal
- el cambio sea mediano, pero el alcance ya esté bastante claro
- quieras una implementación más guiada y luego validación/review, sin necesidad de artefactos completos
- necesites repartir trabajo simple entre roles con poco riesgo contractual

### Usa **multi-agent + SDD** cuando

- vayas a tocar backend, frontend y base de datos en el mismo cambio
- necesites definir alcance, no-alcance, riesgos y criterios de aceptación antes de implementar
- el cambio requiera `TASK-*`, ownership por lane y consolidación posterior
- exista riesgo de regresión, integración o despliegue
- se trate de una mejora grande, un mantenimiento delicado o una refactorización con varias fases
- el trabajo vaya a continuar en varias sesiones y quieras dejar memoria útil y artefactos revisables

---

## Cómo forzar prompting directo sin agents + SDD

Aunque el proyecto esté configurado para multi-agent, puedes pedir **bypass explícito** cuando el cambio sea pequeño, local y de bajo riesgo.

### Qué decir en el prompt

Incluye estas tres ideas en tu solicitud:

1. **modo deseado** — “trabaja en modo directo”
2. **prohibición explícita** — “sin multi-agent, sin SDD, sin delegación”
3. **justificación** — “es un cambio pequeño, local y de bajo riesgo”

### Plantilla corta

```text
Trabaja en modo directo.
Sin multi-agent.
Sin SDD.
Sin artefactos.
Sin delegación.
Es un cambio local y de bajo riesgo.
```

### Plantilla recomendada

```text
Quiero resolver esto con prompting directo, sin multi-agent y sin SDD.
No uses Explorer, Spec Writer, Architect ni otros subagentes.
No crees proposal.md, spec.md, design.md, tasks.md, verify.md ni archive.md.
No abras un change-id ni una carpeta en specs/changes/.
Resuelve todo en esta misma conversación.
Es un cambio pequeño, local y de bajo riesgo.
```

### Ejemplos de uso

#### Ejemplo 1: cambio local

```text
Ajusta el texto del README en /Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/README.md.
Hazlo con prompting directo, sin multi-agent y sin SDD.
No delegates ni crees artefactos.
Es un cambio local de bajo riesgo.
```

#### Ejemplo 2: fix pequeño

```text
Corrige este bug en un solo archivo.
Bypass SDD: no uses subagentes ni abras specs/changes/.
Quiero una solución directa en este hilo porque el alcance es pequeño y local.
```

### Cuándo conviene usar este bypass

Úsalo cuando la tarea sea:

- doc-only
- un ajuste pequeño en un archivo
- un fix bien localizado
- una mejora de muy bajo riesgo
- una consulta o explicación puntual

---

## Beneficios principales

### 1. Más orden en cambios complejos

El workflow fuerza una secuencia explícita de exploración, especificación, diseño, implementación, validación y revisión. Eso reduce improvisación y cambios contradictorios.

### 2. Mejor trazabilidad

Los artefactos `proposal.md`, `spec.md`, `design.md`, `tasks.md`, `verify.md` y `archive.md` dejan evidencia clara de:

- qué se quería hacer
- por qué
- qué se aprobó
- cómo se validó
- qué follow-ups quedaron

### 3. Paralelismo más seguro

El paralelismo no ocurre desde el inicio. Primero se fija el diseño y luego se abren lanes sólo cuando el ownership y las fronteras son explícitas. Esto baja conflictos y reduce riesgo de merge caótico.

### 4. Mejor continuidad entre sesiones

Engram permite recuperar decisiones, convenciones y hallazgos importantes sin depender por completo del contexto temporal de una sola conversación.

### 5. Menor dependencia de un prompt perfecto

Con prompting puro, mucho depende de qué tan bien formules la solicitud inicial. Con este sistema, la exploración, el diseño y la consolidación ayudan a compensar solicitudes incompletas o ambiguas.

---

## Costos y desventajas

### 1. Más overhead

Hay más estructura, más coordinación y más pasos que en una conversación directa. Para tareas pequeñas esto puede sentirse innecesario.

### 2. Más consumo de tokens

Incluso optimizado, este enfoque consume más tokens que el prompting simple porque hay:

- handoffs entre agentes
- artefactos SDD
- consolidación
- validación y revisión

### 3. Más tiempo de ejecución

Un flujo gated completo suele tardar más que pedir un cambio directo, especialmente si hay exploración y validación formal.

### 4. Riesgo de sobreproceso

Si usas SDD o múltiples agentes para tareas triviales, el proceso puede volverse más pesado que el valor que aporta.

---

## Consumo de tokens: qué esperar

De forma general, el costo relativo suele verse así:

1. **Prompting simple sin agentes** → menor costo
2. **Un solo agente / cambio directo** → costo bajo a medio
3. **Multi-agent sin SDD completo** → costo medio
4. **Workflow gated completo con SDD** → mayor costo

Este repositorio ya fue optimizado para bajar costo con estas medidas:

- prompts de agentes más cortos
- `AGENTS.md` más compacto
- `fork_context = false` por defecto en handoffs SDD
- memoria on-demand para workers
- planeación secuencial y paralelismo sólo cuando es seguro

Aun así, el objetivo principal no es ahorrar tokens al máximo, sino **reducir errores, retrabajo y pérdida de contexto**.

---

## Recomendación final

### Sí vale la pena usar esta configuración cuando:

- el cambio es mediano o grande
- existe riesgo técnico u operativo
- hay varias capas involucradas
- necesitas artefactos, revisión y memoria duradera
- el trabajo continuará en sesiones futuras

### No vale la pena usarla como único modo de trabajo cuando:

- la tarea es pequeña o trivial
- quieres velocidad por encima de formalidad
- el alcance está completamente claro y es local
- el costo de coordinación supera el riesgo del cambio

### Recomendación operativa

Adopta un enfoque híbrido:

- usa **prompting simple** para trabajo pequeño, puntual o exploratorio
- usa **multi-agent + SDD** para cambios complejos, riesgosos o multi-scope

Ese balance suele dar el mejor resultado entre velocidad, costo, trazabilidad y calidad.

---

## Cómo funciona Engram dentro del flujo SDD

Engram se usa para persistir conocimiento de alto valor, pero ahora queda **alineado al `change-id`** cuando el cambio usa SDD.

### Política actual

- todos los agentes pueden **leer** memoria si aporta contexto
- sólo el **Orchestrator** puede **persistir memoria final**
- los demás agentes devuelven **memory candidates**
- los borradores de `proposal/spec/design/tasks` **no** deben guardarse como memoria final
- la memoria debe salir del resultado consolidado y, cuando exista, de `archive.md`

### Qué se guarda

- decisiones de arquitectura confirmadas
- convenciones del repositorio confirmadas
- root causes reales
- riesgos técnicos u operativos relevantes
- resumen final del cambio archivado

---

## Ventajas de utilizar esta combinación

### 1. Menos ambigüedad

Los cambios importantes dejan de depender sólo del contexto conversacional y pasan a tener artefactos explícitos.

### 2. Mejor paralelismo

No se paraleliza “porque sí”; primero se documenta ownership, dependencias y serialización.

### 3. Mejor review

Reviewer y Test ya no trabajan sólo contra el diff: pueden revisar contra `spec.md`, `design.md` y `verify.md`.

### 4. Mejor trazabilidad

Cada cambio importante puede reconstruirse por `change-id`.

### 5. Mejor memoria

Engram deja de almacenar ruido intermedio y se concentra en conclusiones aprobadas.

### 6. Mejor handoff

Cuando un agente retoma trabajo de otro, el estado ya está documentado en artefactos y no sólo en memoria informal.

---

## Archivos relevantes del repositorio

- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/AGENTS.md` — reglas, gating SDD, workflow y memoria
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/` — prompts por agente
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/skills/` — skills locales y skills SDD
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/engram-instructions.md` — política de memoria alineada a `change-id`
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/engram-compact-prompt.md` — compactación basada en resultados archivados
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/` — artefactos versionados por cambio
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/specs/changes/adopt-sdd-hybrid/` — ejemplo real del cambio que adoptó SDD en este repo
