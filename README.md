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

En `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/config.toml` está activado:

```toml
[features]
multi_agent = true
```

### Agentes configurados localmente

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
