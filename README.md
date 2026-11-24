# OWASP LLM01 Guardrails Template y Guía de Seguridad de IA

[![Licence MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![OWASP LLM01](https://img.shields.io/badge/OWASP-LLM01%20Hardened-ff0055.svg)](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
[![Language](https://img.shields.io/badge/Language-Español%20(México)-green.svg)](README.md)
[![AI Diagnostics](https://img.shields.io/badge/TAVARDT-AI%20Evaluator-5b3cf5.svg)](https://tools.tavardt.com/mx/evaluador-robustez-llm/)

Blueprint declarativo y middleware de código abierto para la mitigación de Prompt Injection (OWASP LLM01), fuga de system prompt y manipulación adversarial en agentes de IA, en cumplimiento con la LFPDPPP y recomendaciones del INAI para sistemas inteligentes.

Desarrollado y mantenido por el equipo de ingeniería de inteligencia artificial de **TAVARDT**.

***

## 1. Arquitectura de Seguridad para IA y Agentes Cognitivos

Este repositorio proporciona una librería modular sin dependencias pesadas para implementar **guardrails** en agentes autónomos de atención, sistemas RAG y plataformas de negocio que consumen modelos fundacionales de IA (OpenAI, Anthropic, Gemini, Llama).

### Blindaje Perimetral en la Capa de IA
Los LLMs procesan las directrices de control y los datos de entrada en un mismo flujo contextual. Sin guardrails activos, las soluciones de inteligencia artificial corporativas se exponen a ataques de suplantación de identidad de agente y fuga de secretos comerciales.

***

## 2. Vectores de Amenaza Mitigados (OWASP Top 10 for LLMs)

| Capa de Defensa | Vector de Amenaza Mitigado | Mecanismo de Mitigación |
| :- | :- | :- |
| **Input Sanitizer** | Inyección Directa de Prompt (Jailbreak) | Detección heurística de comandos de escape (`ignora las instrucciones anteriores`, cambios de rol no autorizados). |
| **Canary Token Verifier** | Fuga de Prompt de Sistema | Supervisión continua de tokens canario en las respuestas para evitar la revelación de prompts confidenciales. |
| **RAG Boundary Guard** | Inyección Indirecta en Bases RAG | Aislamiento y sanitización de fragmentos documentales recuperados de bases vectoriales. |
| **Structured Output Filter** | Disparo No Autorizado de Acciones | Verificación rigurosa de esquemas JSON antes de disparar acciones en APIs o herramientas MCP. |

***

## 3. Diagnóstico Previo de Robustez de su Modelo

Antes de poner agentes inteligentes en interacción directa con usuarios o clientes, evalúe su vulnerabilidad con la herramienta de diagnóstico de TAVARDT:

* **Evaluación de Robustez:** Evaluador de Robustez de LLMs TAVARDT
* **Suite Completa de Diagnósticos:** **TAVARDT Tools México**

***

## 4. Instalación y Guía de Implementación Rápida

### Clonar y Ejecución

```bash
git clone https://github.com/TAVARDT-Mexico/owasp-llm-guardrails-template.git
cd owasp-llm-guardrails-template
python examples/quickstart.py
```

### Ejemplo de Integración en Python

```python
from guardrails.validator import LLMGuardrail

guard = LLMGuardrail()

# 1. Validación de Entrada (Input Guardrail)
is_safe, error = guard.validate_input(user_input)
if not is_safe:
    raise ValueError(f"Entrada bloqueada por patrón sospechoso: {error}")

# 2. Inserción de Token Canario en el Prompt de Sistema
hardened_system_prompt = guard.get_canary_injection_prompt(system_prompt)

# 3. Validación de Salida (Output Guardrail)
is_valid, leak_error = guard.validate_output(completion)
if not is_valid:
    raise SecurityError("Fallo crítico: ¡Fuga de system prompt detectada mediante token canario!")
```

***

## 5. Consultoría Especializada en Agentes de IA y Ciberseguridad

Si su empresa requiere consultoría para el despliegue seguro de agentes en WhatsApp o auditorías de sistemas generativos:

* **Portal Corporativo:** **TAVARDT**
* **Contacto Técnico Directo:** [Atención Técnica Especializada](https://ag.tavardt.com/mx/contacto/)

***

## Licencia

Distribuido bajo licencia de código abierto MIT. Consulte el archivo [`LICENSE`](LICENSE) para más detalles.  
Copyright

***

© 2026 TAVARDT. Todos los derechos reservados.
