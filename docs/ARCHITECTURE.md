# Arquitectura del Sistema

## 1. Visión general

Agente LLM con bucle ReAct, HITL, persistencia SQLite, UI tkinter y **orquestador de subtareas**. Cero dependencias externas en tiempo de ejecución (excepto `llama-cpp-python` opcional).

## 2. Diagrama de flujo

### 2.0. Flujo de orquestación (nivel superior)

```mermaid
flowchart TD
    A([Prompt del usuario]) --> B[Tarea padre]
    B --> C[📋 Requisitos]
    C --> D[🛠 Desarrollo]
    D --> E[✅ Ejecución/Verificación]
    E -- VERIFICACIÓN EXITOSA --> F[Tarea padre COMPLETED]
    E -- VERIFICACIÓN FALLIDA --> G{¿Quedan reintentos?}
    G -- Sí --> H[🔧 Rectificación]
    H --> E
    G -- No --> I[Tarea padre FAILED]
```

### 2.1. Flujo del agente (nivel subtarea)

```mermaid
flowchart TD
    A([Nueva Subtarea]) --> B[Bucle de Razonamiento]
    B --> C{¿Tool Call?}
    C -- No --> G[Respuesta Final]
    C -- Sí --> D{¿Riesgo?}
    D -- Lectura --> E[Ejecución Auto]
    D -- Crítico --> F[Espera HITL]
    F -- Aprobado --> E
    F -- Denegado --> B
    E --> H[Historial]
    H --> B
```

### 2.2. Mecanismo de forzado de herramientas (3 strikes)

1. Primera iteración sin `tool_calls`: `tool_choice="auto"`.
2. Segunda: `tool_choice="required"`.
3. Tercera: recordatorio explícito en el historial.
4. Cuarta: se acepta el contenido como respuesta final.

## 3. Componentes principales

### A. Orquestador de Subtareas (TaskOrchestrator)

Descompone cada tarea del usuario en 4 subtareas secuenciales:

| # | Subtarea | Prompt fijo (resumen) |
|---|---|---|
| 1 | 📋 REQUIREMENTS | Analiza y documenta requisitos técnicos. NO ejecuta acciones. |
| 2 | 🛠 DEVELOPMENT | Implementa la solución basándose en los requisitos. |
| 3 | ✅ EXECUTION_VERIFICATION | Ejecuta y verifica. Devuelve `VERIFICACIÓN EXITOSA:` o `VERIFICACIÓN FALLIDA:`. |
| 4 | 🔧 RECTIFICATION | (Condicional) Corrige la solución si la verificación falla. |

El ciclo 3↔4 se repite hasta que la verificación sea exitosa o se alcance `max_rectification_retries` (configurable en `config.ini`, por defecto 3).

### B. Motor del Agente (LLM & Tools)

Soporta dos modos:

| Modo | Descripción | Dependencia |
|---|---|---|
| `local` | GGUF con `llama-cpp-python` | `llama-cpp-python` |
| `http` | Endpoint compatible OpenAI | Ninguna |

**Parser dual:** soporta `message.tool_calls` (OpenAI nativo) y bloques `<tool_call>{...}</tool_call>` en texto plano.

### C. Gestor de Permisos (HITL)

- Acciones seguras → ejecución automática.
- Acciones críticas → pausa con `AWAITING_APPROVAL` hasta decisión humana.
- Timeout: 10 minutos (denegación automática).

### D. Estados de tarea

```mermaid
stateDiagram-v2
    direction LR
    [*] --> PENDING
    PENDING --> IN_PROGRESS
    IN_PROGRESS --> AWAITING_APPROVAL
    AWAITING_APPROVAL --> IN_PROGRESS
    IN_PROGRESS --> COMPLETED
    IN_PROGRESS --> FAILED
    IN_PROGRESS --> CANCELLED
```

### E. Persistencia

- SQLite nativo con FK `ON DELETE CASCADE`.
- Índices en `task_id`, `status` y `parent_task_id`.
- Thread-safety con `threading.Lock` y timeout de 10s.
- Limpieza de tareas `PENDING`/`IN_PROGRESS`/`AWAITING_APPROVAL` al arrancar.
- Migración ligera: columnas nuevas se añaden con `ALTER TABLE` si no existen.

## 4. Mecanismos de resiliencia

- **Regla de 3 strikes** para forzar uso de herramientas.
- **Detección de bucles** con `loop_threshold` y compactación de contexto.
- **Parser dual** (OpenAI nativo + bloques `<tool_call>`).
- **Normalización de argumentos** malformados.
- **Captura global de excepciones** en el bucle del agente.

## 5. Modelo de datos

```sql
CREATE TABLE tasks (
    id              INTEGER PRIMARY KEY,
    title           TEXT,
    prompt          TEXT,
    status          TEXT,
    created_at      TEXT,
    updated_at      TEXT,
    final_answer    TEXT,
    parent_task_id  INTEGER REFERENCES tasks(id) ON DELETE CASCADE,
    subtask_type    TEXT,           -- REQUIREMENTS | DEVELOPMENT |
                                    -- EXECUTION_VERIFICATION | RECTIFICATION
    attempt_number  INTEGER NOT NULL DEFAULT 0
);

CREATE TABLE history (
    id INTEGER PRIMARY KEY,
    task_id INTEGER REFERENCES tasks(id) ON DELETE CASCADE,
    event_type TEXT,
    content TEXT,
    created_at TEXT
);
```

Las tareas padre tienen `parent_task_id = NULL` y `subtask_type = NULL`.
Las subtareas tienen `parent_task_id` apuntando a su padre y `subtask_type`
indicando la fase del flujo. `attempt_number` se incrementa en cada
reintento de rectificación.

## 6. Herramientas

### Seguras

- `read_file(path)`
- `list_directory(path?)`
- `search_files(pattern, path?)`
- `get_current_time()`

### Críticas (HITL)

- `write_file(path, content)`
- `execute_command(command)`
- `delete_file(path)`

### Restricciones

- Workspace restringido (path traversal bloqueado).
- Denylist: `rm -rf /`, `format`, `del /f /s /q`, `shutdown`, `reboot`.
- Timeout de comandos: 30s.
- Truncado: 50 KB archivos, 20 KB comandos, 200 coincidencias búsquedas.
