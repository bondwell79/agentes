# Testing

## Suites disponibles

### `test_resilience.py`

108+ escenarios que verifican que el agente no se rompe ante respuestas malformadas del LLM. Usa mocks para `LLMConnector` y `PermissionManager`. **No requiere LLM real ni UI.**

```bash
python test_resilience.py
```

Cubre:
- Parsing de `tool_calls` con argumentos inválidos.
- Extracción de bloques `<tool_call>` desde texto.
- Ejecución de herramientas con argumentos extremos.
- Bucle del agente con respuestas que causan excepciones.
- Agotamiento de iteraciones.

### `test_funcionamiento.py`

Verifica que el sistema acepta múltiples tareas en paralelo y mantiene historiales independientes.

```bash
python test_funcionamiento.py
```

### `test_subtareas.py`

27 escenarios que verifican el orquestador de subtareas (`TaskOrchestrator`).

```bash
python test_subtareas.py
```

Cubre:
- Flujo feliz: 3 subtareas, verificación exitosa al primer intento.
- Un ciclo de rectificación: verificación falla → rectifica → verifica OK.
- Agotamiento de reintentos: verificación siempre falla.
- Fallo en subtarea de requisitos → tarea padre FAILED.
- Relación padre-subtarea en la BD.
- Esquema de BD con nuevas columnas.

## Añadir nuevos tests

1. Crea una clase mock que herede de `LLMConnector`.
2. Define las respuestas que quieres simular.
3. Llama a `Agent.run()` o `TaskOrchestrator.run()` y verifica el estado final.
4. Usa los helpers `assert_eq`, `assert_true`, `assert_raises` y `assert_not_crashes` definidos al inicio de cada fichero de test (no requieren import externo).
5. Usa `make_test_db(tmp_dir)` para crear una BD temporal aislada.
