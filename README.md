# Gestor de Agentes LLM

> Agente LLM autónomo con control de permisos humano-en-el-bucle (HITL), construido exclusivamente con la biblioteca estándar de Python.

![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Dependencies](https://img.shields.io/badge/dependencies-zero-brightgreen)

## ✨ Características

- 🤖 **Agente ReAct** con bucle de razonamiento y herramientas.
- 🛡️ **Human-in-the-Loop (HITL):** aprobación manual de acciones críticas.
- 🔌 **Modo dual LLM:** local (GGUF con `llama-cpp-python`) o HTTP (OpenAI/Ollama/llama.cpp server).
- 📊 **Dashboard tkinter** con 4 zonas: prompt, tablero de tareas, historial y panel de aprobación.
- 💾 **Persistencia SQLite** con borrado en cascada y limpieza al arrancar.
- 🧪 **Resiliencia probada:** 40+ tests que cubren respuestas malformadas del modelo.
- 📦 **Ejecutable autónomo** compilable con Nuitka (cero instalación en el destino).

## 📸 Captura de pantalla

*(añadir aquí una captura del dashboard)*

## 📦 Requisitos

- Python 3.10 o superior
- Windows 10/11 (probado en Windows; tkinter es multiplataforma)
- Opcional: `llama-cpp-python` (solo para modo local)
- Opcional: un modelo GGUF (recomendado: `Qwen3-4B-Instruct-2507-Q4_K_M.gguf`)

## 🚀 Instalación rápida

```bash
git clone https://github.com/ruben-pastor/gestor-agentes.git
cd gestor-agentes
copy config.ini.example config.ini
python gestor_agentes.py
```

## 🧪 Tests

```bash
python test_resilience.py        # 40+ escenarios de resiliencia
python test_funcionamiento.py    # tareas en paralelo
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
