# Design

- **Change ID**: enable-codex-cli-multi-agent
- **Status**: approved
- **Owner**: Orchestrator

## Proposed Design

Usar `.codex/config.toml` como manifiesto principal de roles multi-agent y crear archivos `.codex/agents/<role>.toml` como configuración ejecutable por Codex CLI. Cada archivo de rol incluirá `model` y `developer_instructions` derivadas del `.md` existente del mismo nombre.

## Impacted Components

- `.codex/config.toml`
- `.codex/agents/*.toml`
- `.codex/agents/*.md` como fuente editorial existente
- `README.md`
- `specs/changes/enable-codex-cli-multi-agent/*`

## Data / Request Flow

1. Codex CLI carga el config principal.
2. El config principal declara roles y resuelve `config_file` relativo hacia `.codex/agents/<role>.toml`.
3. Cada archivo de rol aporta `model` y `developer_instructions` del agente.
4. README explica cómo integrar este manifiesto en la configuración activa del usuario.

## Dependency Graph

- `.codex/agents/*.md` → fuente de instrucciones
- `.codex/agents/*.toml` → empaquetado para Codex CLI
- `.codex/config.toml` → manifiesto de roles
- `README.md` → guía de consumo

## Ownership Plan

- Config principal: `.codex/config.toml`
- Configs de roles: `.codex/agents/*.toml`
- Documentación: `README.md`
- Artefactos SDD: `specs/changes/enable-codex-cli-multi-agent/*`

## Merge Order

1. Crear artefactos SDD.
2. Crear configs `.toml` por rol.
3. Actualizar `.codex/config.toml` para apuntar a los nuevos archivos.
4. Actualizar README.
5. Validar parseo y coherencia.

## Serialization Points

- `.codex/config.toml` depende de que existan los `config_file` de cada rol.
- README debe documentar la forma final de la configuración, no un estado intermedio.

## Interface / Contract Changes

- El contrato operativo del repo cambia de un pseudo-schema propio a uno alineado con el schema oficial de Multi-agents.

## Tradeoffs

- Se duplica el contenido de instrucciones entre `.md` y `.toml` para mejorar compatibilidad inmediata con Codex CLI.
- Mantener `.md` como fuente legible facilita edición humana, pero requiere disciplina para evitar deriva.

## Migration / Rollout Concerns

- El usuario debe integrar manualmente el config del repo en la configuración activa de Codex CLI si su entorno no consume `.codex/config.toml` directamente.
- La validación será sintáctica/estructural, no una ejecución end-to-end autenticada.

## Risks

- Posible deriva entre `.md` y `.toml` si sólo se edita uno de los dos formatos.
- Diferencias de soporte entre versiones de Codex CLI/App.

## Recommendation

Aplicar el schema oficial con archivos `.toml` por rol y documentar explícitamente que el repo entrega la configuración reusable para Codex CLI, dejando fuera cambios sobre `~/.codex/config.toml` del usuario.
