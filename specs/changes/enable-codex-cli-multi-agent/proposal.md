# Proposal

- **Change ID**: enable-codex-cli-multi-agent
- **Status**: approved
- **Owner**: Orchestrator

## Goal

Convertir la configuración local del repositorio al esquema oficial de Multi-agents para Codex CLI, de forma que el proyecto publique una configuración reutilizable y consistente con los roles ya definidos.

## In Scope

- Reemplazar `.codex/config.toml` por un archivo principal compatible con el esquema oficial de Multi-agents.
- Crear archivos `.toml` por rol bajo `.codex/agents/` usando los agentes existentes del repositorio.
- Mantener los `.md` actuales como fuente legible de instrucciones y reflejar su uso en la documentación.
- Actualizar README con la forma correcta de usar la configuración en Codex CLI.

## Out of Scope

- Modificar `~/.codex/config.toml` del usuario.
- Cambiar el contenido funcional de las instrucciones de los agentes más allá del empaquetado para Codex CLI.
- Introducir automatización adicional o nuevos agentes fuera de los ya existentes.

## Risks

- El schema oficial puede diferir entre versiones de Codex CLI/App.
- El repositorio puede documentar una configuración local que no sea la activa en la instalación del usuario.
- Duplicar instrucciones entre `.md` y `.toml` puede generar deriva futura si no se documenta la relación.

## Unknowns

- Qué versión exacta de Codex CLI aplicará finalmente el usuario al activar la configuración global.
- Si el usuario preferirá copiar el config del repo a `~/.codex/config.toml` o integrarlo manualmente.

## Shared Surfaces / Parallelism Notes

- Superficie compartida principal: `.codex/config.toml`, `.codex/agents/*`, `README.md`.
- No hay independencia defendible entre archivos de config y documentación; el cambio se ejecuta de forma secuencial.

## Approval Notes

- Gated por afectar múltiples archivos de configuración/documentación y por cambiar el contrato operativo del repo para Codex CLI.
