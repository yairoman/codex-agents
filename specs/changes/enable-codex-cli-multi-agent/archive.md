# Archive

- **Change ID**: enable-codex-cli-multi-agent
- **Status**: approved
- **Owner**: Orchestrator

## Outcome

El repositorio quedó convertido al esquema oficial de Multi-agents para Codex CLI: `.codex/config.toml` ahora declara roles con `description` + `config_file`, y cada rol tiene un archivo `.codex/agents/<rol>.toml` ejecutable derivado de su `.md`.

## Follow-up Work

- Si se cambia el contenido de un agente en `.codex/agents/<rol>.md`, sincronizar el `.toml` correspondiente.
- Integrar este manifiesto en `~/.codex/config.toml` o en un profile global del entorno donde se quiera activar realmente.

## Archived Learnings

- El schema correcto del manifiesto principal usa `[agents.<rol>]` con `description` y `config_file`.
- Para Codex CLI, los archivos `.toml` por rol son el empaquetado ejecutable; los `.md` pueden mantenerse como fuente editorial legible.
- La validación local más segura sin tocar config global es comprobar parseo TOML y existencia de `config_file`, dejando la corrida end-to-end como paso manual del usuario.
