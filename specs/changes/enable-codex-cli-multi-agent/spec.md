# Spec

- **Change ID**: enable-codex-cli-multi-agent
- **Status**: approved
- **Owner**: Orchestrator

## Problem Statement

La configuración actual del repositorio no está empaquetada según el esquema oficial de Multi-agents de Codex CLI, lo que provoca fallos o ambigüedad al intentar activar agentes por rol desde `config.toml`.

## Scope

- Definir un `.codex/config.toml` principal con `multi_agent`, límites generales y roles declarados con `description` y `config_file`.
- Definir archivos `.toml` por rol compatibles con Codex CLI para: orchestrator, explorer, spec_writer, architect, backend, frontend, data_sql, test, reviewer y devops.
- Documentar cómo usar esta configuración desde Codex CLI.

## Out of Scope

- Validar con una sesión real autenticada de multi-agent del usuario.
- Rediseñar el contenido funcional de cada rol.

## Requirements

- REQ-1: `.codex/config.toml` debe usar el esquema oficial de Multi-agents con `[features]`, `[agents]` y `[agents.<role>]` usando `description` y `config_file`.
- REQ-2: Cada rol declarado en `.codex/config.toml` debe tener un archivo `.toml` existente bajo `.codex/agents/`.
- REQ-3: Cada archivo de rol debe declarar al menos un `model` válido y `developer_instructions` para el rol correspondiente.
- REQ-4: La documentación del repo debe explicar que la configuración del repo es una base reusable para integrar en Codex CLI.

## Scenarios / Acceptance Criteria

- SCN-1: Al inspeccionar `.codex/config.toml`, existen entradas para todos los roles del proyecto con `config_file` relativo correcto.
- SCN-2: Al inspeccionar `.codex/agents/*.toml`, cada rol tiene un archivo compatible y consistente con el `.md` correspondiente.
- SCN-3: README describe el esquema multi-agent del repo y cómo activarlo desde Codex CLI.

## Edge Cases

- Nombres de rol con guion bajo como `spec_writer` y `data_sql` deben conservarse de forma consistente entre config principal y archivos.
- El repo puede mantenerse funcional aunque la activación final se haga en `~/.codex/config.toml` del usuario.

## Error Handling Expectations

- Si no es posible validar ejecución real del CLI, debe documentarse explícitamente como limitación de validación.
- El TOML generado debe parsear correctamente sin errores de sintaxis.

## Security / Permission Considerations

- No incluir secretos ni credenciales en los nuevos archivos `.toml`.
- No modificar configuración global del usuario sin solicitud explícita.

## Scope Boundaries

- Sólo se tocan `.codex/config.toml`, `.codex/agents/*.toml`, `README.md` y artefactos SDD del cambio.

## Lane Inputs / Outputs

- Lane única secuencial de configuración/documentación.

## Cross-Lane Dependencies

- No aplica; ejecución serial.

## Open Questions / Assumptions

- Asunción: `developer_instructions` inline es compatible con el schema oficial por rol.
- Asunción: los `.md` existentes siguen siendo la fuente editorial del contenido de cada agente.
