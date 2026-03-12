# codex-agents

Repositorio de ejemplo para trabajar con **Codex en modo multiagente**, usando **Engram** como memoria persistente y un conjunto de **skills locales** para estandarizar exploración, especificación, implementación, validación y revisión.

## Qué resuelve este repositorio

Este proyecto organiza el trabajo de Codex con tres piezas principales:

1. **Agentes especializados** definidos en `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents`
2. **Skills locales** definidas en `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/skills`
3. **Memoria persistente con Engram** para recordar decisiones, convenciones, bugs y resúmenes de sesión

La idea es producir cambios:

- pequeños
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

## Cómo está configurado este repositorio

### Multi-agent habilitado

En `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/config.toml` está activado:

```toml
[features]
multi_agent = true
```

Además, el repositorio define agentes especializados para cada fase del trabajo.

### Agentes configurados localmente

Los agentes declarados en el repositorio son:

| Agente       | Propósito                                                 |
| ------------ | --------------------------------------------------------- |
| Orchestrator | Coordina el flujo completo y consolida el resultado final |
| Explorer     | Analiza el repositorio antes de implementar cambios       |
| Spec Writer  | Convierte la solicitud en una especificación clara        |
| Architect    | Define el enfoque técnico más seguro y simple             |
| Backend      | Implementa cambios backend                                |
| Frontend     | Implementa cambios de interfaz                            |
| Data SQL     | Maneja cambios de base de datos y queries                 |
| Test         | Valida criterios de aceptación y regresiones              |
| Reviewer     | Hace revisión técnica final                               |
| DevOps       | Evalúa impacto operativo, despliegue y configuración      |

### Flujo esperado de trabajo

Este repositorio está pensado para ejecutarse en esta secuencia:

1. **Explorer**
2. **Spec Writer**
3. **Architect**
4. **Backend / Frontend / Data SQL**
5. **Test**
6. **Reviewer**
7. **DevOps** (si aplica)

Ese flujo está alineado con `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/AGENTS.md`.

---

## Skills locales configuradas en este repositorio

Las skills locales disponibles en `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/skills` son:

| Skill                | Uso principal                                                 |
| -------------------- | ------------------------------------------------------------- |
| `repo-exploration`   | analizar el repositorio antes de cambiar algo                 |
| `feature-spec`       | convertir una petición en una especificación implementable    |
| `safe-refactor`      | modificar código existente sin cambiar comportamiento externo |
| `incident-debugging` | investigar y resolver incidentes o bugs en producción         |
| `sql-migration`      | planear cambios de esquema o migraciones de datos             |
| `release-checklist`  | revisar preparación para despliegue y riesgo operativo        |

### Cómo trabajan juntos agentes + skills

Una forma práctica de entenderlo es esta:

- **Explorer** suele apoyarse en `repo-exploration`
- **Spec Writer** suele apoyarse en `feature-spec`
- **Backend / Frontend** pueden usar `safe-refactor` cuando el objetivo es cambiar poco y con bajo riesgo
- **Data SQL** puede usar `sql-migration` cuando hay cambios de esquema o datos
- **Reviewer / DevOps** pueden complementar validación y despliegue con `release-checklist`
- **Incident debugging** entra cuando la tarea es investigar causa raíz y proponer una corrección segura

En otras palabras:

- los **agentes** definen el **rol**
- las **skills** definen el **método de trabajo**
- **Engram** conserva el **contexto útil** entre sesiones

---

## Cómo funciona Engram dentro del flujo multiagente

En este repositorio, Engram se usa para persistir conocimiento de alto valor, por ejemplo:

- decisiones de arquitectura
- convenciones del repositorio
- hallazgos importantes
- bugs investigados
- resúmenes de sesión

### Uso típico

1. El agente recibe una tarea
2. Busca memoria previa si el tema ya pudo haberse trabajado
3. Ejecuta el flujo multiagente
4. Implementa el cambio mínimo posible
5. Guarda en Engram lo importante para futuras sesiones
6. Cierra la sesión con un resumen utilizable

Eso reduce el costo de volver a entender el proyecto cada vez.

---

## Ventajas de utilizar esta combinación

### 1. Menos pérdida de contexto

Con Engram, el agente no depende sólo del contexto activo de la conversación; puede recuperar decisiones y hallazgos de sesiones anteriores.

### 2. Cambios más consistentes

Las skills locales hacen que el trabajo siga un patrón repetible: explorar, especificar, diseñar, implementar, probar y revisar.

### 3. Mejor calidad de cambios

El flujo multiagente obliga a pensar en:

- impacto técnico
- riesgos
- regresiones
- despliegue
- revisión final

### 4. Menor riesgo operativo

No todo termina en “editar código”. También se contempla:

- pruebas
- revisión técnica
- compatibilidad
- despliegue
- rollback o mitigación

### 5. Reutilización del conocimiento del proyecto

A medida que el equipo trabaja, Engram acumula memoria útil del repositorio sin llenar el contexto con ruido.

### 6. Onboarding más rápido

Nuevas sesiones o nuevos agentes pueden entender más rápido:

- cómo se trabaja aquí
- qué decisiones ya se tomaron
- qué patrones conviene respetar

---

## Archivos relevantes del repositorio

- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/AGENTS.md` — reglas, responsabilidades y flujo obligatorio
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/config.toml` — habilitación de multi-agent y registro de agentes
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/agents/` — instrucciones por agente
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/skills/` — skills locales del proyecto
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/engram-instructions.md` — protocolo de memoria para Codex
- `/Users/yairoman/Documents/Proyectos/Aimorro/codex-agents/.codex/engram-compact-prompt.md` — recuperación después de compaction

---
