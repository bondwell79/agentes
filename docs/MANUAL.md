# Manual de Usuario — Gestor de Agentes LLM

## 1. Introducción

Gestor de Agentes es una aplicación de escritorio que ejecuta un agente LLM con control de permisos humano-en-el-bucle. Permite dar instrucciones en lenguaje natural y observar paso a paso cómo el agente las resuelve, aprobando o denegando las acciones sensibles.

## 2. Instalación

### 2.1. Desde el ejecutable (recomendado para usuarios)

1. Descarga `gestor_agentes.zip` desde la sección Releases.
2. Descomprime en cualquier carpeta.
3. Asegúrate de que el modelo GGUF está en `modelos/` (o ajusta `config.ini`).
4. Ejecuta `gestor_agentes.exe`.

### 2.2. Desde el código fuente

```bash
pip install llama-cpp-python   # solo si vas a usar modo local
python gestor_agentes.py
```

## 3. Configuración inicial

Edita `config.ini` antes del primer arranque:

- **Modo LLM:** `local` (usa el modelo GGUF) o `http` (usa un servidor externo).
- **Modelo:** ruta al `.gguf` o identificador del endpoint.
- **Workspace:** carpeta donde el agente puede leer/escribir.
- **UI:** colores y tipografía (opcional).

Ver [CONFIGURATION.md](CONFIGURATION.md) para la referencia completa.

## 4. Uso de la aplicación

### 4.1. Crear una tarea

1. Escribe la instrucción en la **caja de prompt** (zona superior).
2. Pulsa `▶ Ejecutar`.
3. La tarea aparece en la columna izquierda del tablero.

### 4.2. Monitorizar una tarea

- Selecciona una tarea en el tablero para ver su historial en tiempo real.
- Los eventos se muestran con colores diferenciados:
  - 💭 Pensamiento
  - 🔧 Tool Call
  - 📥 Tool Result
  - ⚠ Solicitud de aprobación
  - ✅ Aprobado / ❌ Denegado
  - 🏁 Respuesta final

### 4.3. Aprobar o denegar acciones críticas

Cuando el agente intenta ejecutar una acción sensible (`write_file`, `execute_command`, `delete_file`):

1. Aparece el **panel de aprobación** en la zona inferior.
2. Revisa los argumentos formateados (con vista previa del contenido si es `write_file`).
3. Pulsa `✅ Permitir` o `❌ Cancelar`.
4. Si no respondes en 10 minutos, la acción se deniega automáticamente.

### 4.4. Ejecutar varias tareas en paralelo

Puedes lanzar varias tareas seguidas sin esperar a que terminen. Cada una corre en su propio hilo y mantiene su historial independiente.

## 5. Herramientas disponibles

### Seguras (ejecución automática)

| Herramienta | Uso |
|---|---|
| `read_file` | Leer un archivo |
| `list_directory` | Listar contenido de una carpeta |
| `search_files` | Buscar por patrón glob |
| `get_current_time` | Obtener fecha/hora UTC |

### Críticas (requieren aprobación)

| Herramienta | Uso |
|---|---|
| `write_file` | Escribir/crear archivo |
| `execute_command` | Ejecutar comando del sistema |
| `delete_file` | Eliminar archivo o carpeta |

## 6. Solución de problemas

- **El agente no usa herramientas:** el sistema aplica automáticamente la [regla de 3 strikes](ARCHITECTURE.md#21-mecanismo-de-forzado-de-herramientas-3-strikes); si el modelo es muy pequeño, considera usar uno mayor.
- **Error "ruta fuera del workspace":** todas las rutas deben ser relativas a la carpeta configurada en `[Workspace] path`.
- **Comando bloqueado:** la denylist incluye `rm -rf /`, `format`, `shutdown`, etc.
