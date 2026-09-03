# Gestor de Agentes LLM

> Agente LLM autónomo con control de permisos humano-en-el-bucle (HITL), construido exclusivamente con la biblioteca estándar de Python.

![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen)

## ✨ Características

- 🤖 **Agente ReAct** con bucle de razonamiento y herramientas.
- 🎼 **Descomposición automática en subtareas:** cada tarea se divide en Requisitos → Desarrollo → Ejecución/Verificación → Rectificación (si falla).
- 🛡️ **Human-in-the-Loop (HITL):** aprobación manual de acciones críticas.
- 🔌 **Modo dual LLM:** local (GGUF con `llama-cpp-python`) o HTTP (OpenAI/Ollama/llama.cpp server).
- 📊 **Dashboard tkinter** con 4 zonas: prompt, tablero de tareas, historial y panel de aprobación.
- 💾 **Persistencia SQLite** con borrado en cascada y limpieza al arrancar.
- 🧪 **Resiliencia probada:** 108+ tests de resiliencia, más suites de funcionamiento en paralelo y orquestación de subtareas.
- 📦 **Ejecutable autónomo** compilable con Nuitka (cero instalación en el destino).

## 📦 Requisitos

- Python 3.10 o superior
- Windows 10/11 (probado en Windows; tkinter es multiplataforma)
- Opcional: `llama-cpp-python` (solo para modo local)
- Opcional: un modelo GGUF (recomendado: `Qwen3-4B-Instruct-2507-Q4_K_M.gguf`)

## 🚀 Instalación rápida

```bash
git clone https://github.com/bondwell79/agentes.git
cd gestor-agentes
python gestor_agentes.py
```

El fichero `config.ini` ya viene incluido con valores por defecto; edítalo para ajustar la ruta del modelo GGUF u otros parámetros (ver [Configuración](docs/CONFIGURATION.md)).

## 🧪 Tests

```bash
python test_global.py             # ejecuta todas las suites y muestra el resumen
python test_resilience.py         # 108+ escenarios de resiliencia
python test_funcionamiento.py     # tareas en paralelo
python test_subtareas.py          # orquestador de subtareas
```

## 🏗️ Compilar ejecutable

```bash
nuitka.bat
```

El ejecutable queda en `gestor_agentes.dist/gestor_agentes.exe`.

## 📚 Documentación

- [Manual de usuario](docs/MANUAL.md)
- [Guía de despliegue](docs/DEPLOYMENT.md)
- [Arquitectura](docs/ARCHITECTURE.md)
- [Configuración](docs/CONFIGURATION.md)
- [Seguridad](docs/SECURITY.md)
- [Testing](docs/TESTING.md)

## 📄 Licencia

MIT — ver [LICENSE](LICENSE).

## 👤 Autor

**Rubén Pastor** — `bondwell_@hotmail.com`
