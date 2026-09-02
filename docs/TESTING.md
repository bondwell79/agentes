# Testing

## Suites disponibles

### `test_resilience.py`

40+ escenarios que verifican que el agente no se rompe ante respuestas malformadas del LLM. Usa mocks para `LLMConnector` y `PermissionManager`. **No requiere LLM real ni UI.**

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

## Añadir nuevos tests

1. Crea una clase mock que herede de `LLMConnector`.
2. Define las respuestas que quieres simular.
3. Llama a `Agent.run()` y verifica el estado final.
4. Usa los helpers `assert_eq`, `assert_true`, `assert_raises` y `assert_not_crashes` definidos al inicio de cada fichero de test (no requieren import externo).
