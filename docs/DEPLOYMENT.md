# Guía de Despliegue

## 1. Compilación del ejecutable

### 1.1. Con Nuitka (método actual)

```bash
nuitka.bat
```

El script ejecuta:

```bash
python -m nuitka --standalone --windows-console-mode=disable \
  --include-package=llama_cpp --include-package-data=llama_cpp \
  --enable-plugin=tk-inter gestor_agentes.py
```

**Resultado:** `gestor_agentes.dist/gestor_agentes.exe` (ejecutable autónomo).

### 1.2. Artefactos generados

- `gestor_agentes.build/` — código C intermedio (puede ignorarse).
- `gestor_agentes.dist/` — distribución final lista para empaquetar.

## 2. Empaquetado para distribución

```bash
cd gestor_agentes.dist
powershell Compress-Archive -Path * -DestinationPath ../gestor_agentes_v1.0.zip
```

## 3. Requisitos del sistema destino

| Componente | Mínimo | Recomendado |
|---|---|---|
| SO | Windows 10 64-bit | Windows 11 |
| RAM | 8 GB | 16 GB (modo local con modelo 4B) |
| CPU | 4 núcleos | 8 núcleos |
| GPU | No requerida | NVIDIA con VRAM ≥ 6 GB |
| Disco | 500 MB | 5 GB (incluye modelo GGUF) |

## 4. Despliegue del modelo GGUF

1. Descarga un modelo compatible (recomendado: Qwen3-4B-Instruct Q4_K_M).
2. Colócalo en `modelos/` junto al ejecutable.
3. Ajusta `config.ini` → `[LLM] model_path`.

## 5. Despliegue en servidor (modo HTTP)

Si prefieres usar Ollama o llama.cpp server:

```bash
# Ollama
ollama serve
# En config.ini:
#   mode = http
#   base_url = http://localhost:11434/v1
#   model = qwen3:4b
```

## 6. Configuración para producción

Recomendaciones:

- `fullscreen = false` para entornos con múltiples monitores.
- `max_iterations = 30` para evitar bucles largos.
- `loop_threshold = 3` para detectar ciclos antes.
- Montar `workspace/` en una carpeta con cuotas si se expone a varios usuarios.

## 7. Logs y diagnóstico

- La base de datos SQLite (`gestor_agentes.db`) contiene todo el historial auditable.
- Para inspeccionarla: `sqlite3 gestor_agentes.db "SELECT * FROM history LIMIT 50;"`
- Los eventos se categorizan por tipo (`thought`, `tool_call`, `tool_result`, `approval_request`, etc.).
