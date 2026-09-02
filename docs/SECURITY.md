# Seguridad

## Reportar vulnerabilidades

Envía un email a `bondwell_@hotmail.com` (NO abras un issue público).

## Modelo de seguridad

- **Workspace restringido:** todas las rutas se validan contra `WORKSPACE_DIR`.
- **Denylist de comandos:** `rm -rf /`, `format`, `del /f /s /q`, `shutdown`, `reboot`.
- **Timeout de comandos:** 30 segundos.
- **HITL:** toda acción crítica requiere aprobación humana con timeout de 10 min.
- **Sin red saliente** salvo hacia el endpoint LLM configurado.
