# Changelog

Todos los cambios notables se documentan aquí. Formato basado en [Keep a Changelog](https://keepachangelog.com/).

**Mantenedor:** Rubén Pastor — `bondwell_@hotmail.com`

## [Unreleased]

## [1.0.0] - 2026-09-02

### Added
- Suite de tests de resiliencia con 40+ escenarios.
- Suite de tests de funcionamiento (tareas en paralelo).
- Mecanismo de detección de bucles y compactación de contexto.
- Barra de uso de contexto en el dashboard.
- Herramientas `get_current_time` y `delete_file`.
- Soporte de bloques `<tool_call>` en texto plano.
- Modo dual LLM (local con GGUF / HTTP con OpenAI/Ollama).

### Changed
- Migración de PyInstaller a Nuitka para compilación.
- Layout de UI reorganizado en 4 zonas con prioridad estricta.
- Tema oscuro por defecto con colores configurables.

### Fixed
- Crash cuando `choices[0]` no contenía `message`.
- Argumentos con tipos extremos (`None`, `int`, `bool`) ya no rompen el parser.
