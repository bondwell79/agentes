# Changelog

Todos los cambios notables se documentan aquí. Formato basado en [Keep a Changelog](https://keepachangelog.com/).

**Mantenedor:** Rubén Pastor — `bondwell_@hotmail.com`

## [Unreleased]

### Added
- **Descomposición automática de tareas en subtareas.** Cada tarea del usuario se divide en 4 fases:
  1. 📋 **Requisitos técnicos** — análisis y documentación.
  2. 🛠 **Desarrollo de la solución** — implementación.
  3. ✅ **Ejecución y comprobación** — verificación.
  4. 🔧 **Rectificación** (condicional) — corrección si la verificación falla.
- Clase `TaskOrchestrator` que gestiona el flujo secuencial y el ciclo de rectificación.
- Enum `SubtaskType` con 4 valores: `REQUIREMENTS`, `DEVELOPMENT`, `EXECUTION_VERIFICATION`, `RECTIFICATION`.
- Nuevas columnas en la tabla `tasks`: `parent_task_id`, `subtask_type`, `attempt_number`.
- Migración ligera de esquema (compatible con BDs existentes).
- Configuración `max_rectification_retries` en `[Agent]` de `config.ini` (por defecto 3).
- Tablero de tareas con subtareas anidadas bajo su tarea padre.
- Nuevos tipos de evento: `SUBTASK_CREATED`, `SUBTASK_STARTED`, `SUBTASK_COMPLETED`, `SUBTASK_FAILED`, `ORCHESTRATION_DECISION`.
- Suite de tests `test_subtareas.py` con 27 escenarios del orquestador.

### Changed
- `_on_execute()` del dashboard ahora lanza el orquestador en lugar del agente directamente.
- `_refresh_task_lists()` muestra solo tareas padre en el tablero principal; las subtareas se renderizan anidadas.

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
