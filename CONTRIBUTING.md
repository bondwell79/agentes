# Contribuir

## Reportar bugs

Abre un [issue](https://github.com/<usuario>/gestor-agentes/issues) con:
- Pasos para reproducir.
- Salida esperada vs. obtenida.
- Versión de Python y SO.
- Contenido relevante de `gestor_agentes.db` (si aplica).

## Proponer features

Abre un issue con la etiqueta `enhancement` antes de enviar un PR.

## Estilo de código

- PEP 8.
- Type hints en funciones públicas.
- Docstrings en clases y funciones no triviales.
- Sin dependencias externas (excepto `llama-cpp-python` opcional).

## Tests

Antes de enviar un PR:

```bash
python test_resilience.py
python test_funcionamiento.py
```

Ambos deben pasar al 100%. Añade tests para cualquier bug que corrijas.
