# Configuración

## Prioridad

1. Variables de entorno
2. `config.ini`
3. Valores por defecto

## Variables de entorno

| Variable | Default | Descripción |
|---|---|---|
| `LLM_MODE` | `local` | `local` o `http` |
| `LLM_BASE_URL` | `http://localhost:8080/v1` | Endpoint HTTP |
| `LLM_API_KEY` | _(vacío)_ | Clave de API |
| `LLM_MODEL` | `Qwen3-4B-Instruct-2507-Q4_K_M.gguf` | Modelo |
| `LLM_MODEL_PATH` | `modelos/Qwen3-4B-Instruct-2507-Q4_K_M.gguf` | Ruta GGUF |
| `LLM_TIMEOUT` | `120` | Timeout HTTP (s) |
| `LLM_N_CTX` | `8192` | Contexto (tokens) |
| `LLM_N_THREADS` | `8` | Hilos CPU |
| `LLM_N_GPU_LAYERS` | `0` | Capas GPU |
| `GESTOR_AGENTES_WORKSPACE` | `./workspace` | Workspace |
| `GESTOR_AGENTES_DB` | `gestor_agentes.db` | BD SQLite |
| `GESTOR_AGENTES_MAX_ITER` | `10` | Iteraciones máx. |
| `GESTOR_AGENTES_LOOP_THRESHOLD` | `5` | Umbral de bucle |
| `GESTOR_AGENTES_MAX_RECTIFICATION_RETRIES` | `3` | Reintentos máx. de rectificación |
| `GESTOR_AGENTES_FULLSCREEN` | `false` | Pantalla completa |
| `GESTOR_AGENTES_CONFIG` | `config.ini` | Ruta alternativa |

## Secciones de `config.ini`

Ver el fichero [`config.ini`](../config.ini) — todas las claves están documentadas inline.
