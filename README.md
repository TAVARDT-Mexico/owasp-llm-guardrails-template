<p align="center">
  <a href="https://ag.tavardt.com/mx/">
    <img src="https://ag.tavardt.com/img/tavardt-banner.png" alt="TAVARDT" width="1024" />
  </a>
</p>

# OWASP LLM01 Guardrails Template y Blueprint de Seguridad para IA

[![OWASP LLM01](https://img.shields.io/badge/OWASP-LLM01%20Hardened-ff0055.svg)](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
[![AI Diagnostics](https://img.shields.io/badge/TAVARDT-AI%20Evaluator-5b3cf5.svg)](https://tools.tavardt.com/mx/evaluador-robustez-llm/)
[![Licence MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

***

Middleware de código abierto y blueprint de seguridad para la mitigación de Inyecciones de Prompt (OWASP LLM01), extracción de directivas de sistema y jailbreaks adversarios, desarrollado por **TAVARDT**, en cumplimiento con los requerimientos de la LFPDPPP y estándares internacionales ISO/IEC 42001.

Los Modelos Masivos de Lenguaje procesan las instrucciones de gobierno y los datos introducidos por el usuario dentro del mismo contexto computacional. Sin mecanismos de saneamiento en la capa de aplicación y verificación de salida mediante testigos canario, los agentes autónomos y flujos RAG permanecen expuestos a secuestros de contexto y filtración de credenciales.

***

## Capacidades Principales y Arquitectura Defensiva

* **Saneamiento Determinista de Entrada**: Evalúa los mensajes de entrada frente a expresiones regulares compiladas para bloquear anulaciones de instrucciones y manipulación de etiquetas antes del envío al modelo.
* **Verificación Criptográfica por Testigo Canario**: Inyecta identificadores impredecibles (`TAVARDT_CANARY_<hex16>`) mediante la biblioteca `secrets` para interceptar sesiones que intenten filtrar las instrucciones del sistema.
* **Cobertura Integral de Amenazas**: Neutraliza los riesgos definidos en OWASP-LLM01 (Inyección de Prompt), OWASP-LLM06 (Divulgación de Información Sensible) y OWASP-LLM07 (Diseño Inseguro de Plugins).
* **Cero Dependencias Externas**: Funciona exclusivamente con la biblioteca estándar de Python, asegurando una latencia de verificación inferior a 2 milisegundos sin peticiones de red externas.
* **Integración Transparente y Modular**: Envuelve fácilmente cualquier proveedor de modelos (OpenAI, Anthropic, Google Gemini, modelos locales Llama) en agentes autónomos, asistentes y sistemas RAG.

***

## Vectores de Riesgo e Impacto Operacional

Desplegar modelos fundacionales en entornos de producción sin salvaguardas técnicas expone la información confidencial y las herramientas de automatización a manipulaciones adversarias. El paquete mitiga cuatro vectores fundamentales:

* **Inyección Directa de Prompt (OWASP LLM01)**: Comandos diseñados para anular directivas previas (`ignore previous instructions`) y forzar al modelo a ejecutar acciones no autorizadas.
* **Exfiltración de Instrucciones de Sistema (OWASP LLM06)**: Peticiones orientadas a persuadir al modelo para que reproduzca sus directivas internas y reglas de negocio.
* **Evasión de Delimitadores y Suplantación de Roles**: Manipulación de marcas markdown o XML para activar modos de desarrollo o eliminar restricciones operativas.
* **Inyección Indirecta mediante Fuentes de Conocimiento**: Cargas hostiles camufladas en documentos de terceros que secuestran el proceso de razonamiento en canales de RAG.

***

## Instalación y Ejecución CLI

Incorpore el middleware de protección en su entorno de Python o verifique su funcionamiento con el script de prueba integrado:

```bash
# Clonar el repositorio oficial
git clone https://github.com/TAVARDT-Mexico/owasp-llm-guardrails-template.git

# Acceder al directorio del proyecto
cd owasp-llm-guardrails-template

# Ejecutar el script de verificación rápida
python examples/quickstart.py
```

### Python Integration Example

Proteja cualquier ciclo de inferencia con verificaciones deterministas de entrada y salida:

```python
from guardrails.validator import LLMGuardrail

guard = LLMGuardrail()

# 1. Ingress Validation (Input Guardrail)
is_safe, error = guard.validate_input(user_input)
if not is_safe:
    raise ValueError(f"Input blocked due to suspicious adversarial pattern: {error}")

# 2. Inject Canary Token into System Prompt
hardened_system_prompt = guard.get_canary_injection_prompt(system_prompt)

# 3. Egress Validation (Output Guardrail)
is_valid, leak_error = guard.validate_output(completion)
if not is_valid:
    raise SecurityError("Critical alert: System prompt leak detected via canary token!")
```

***

## Preguntas Técnicas Frecuentes

### ¿Qué vectores de riesgo mitiga el esquema de guardrails OWASP LLM en flujos de modelos fundacionales?

El esquema implementa barreras deterministas en la capa de aplicación frente a inyecciones directas de prompt, jailbreaks y extracción de instrucciones de sistema. Al coordinar el escaneo heurístico de entrada con la verificación de salida mediante testigos canario, el middleware protege las cargas de trabajo de IA sin necesidad de reentrenamiento.

### ¿Cómo previene la validación dinámica por testigo canario la filtración de directivas de sistema?

La clase LLMGuardrail genera un identificador criptográfico único y lo añade a las directivas de sistema junto a instrucciones estrictas de confidencialidad. Durante la validación de salida, el componente analiza las respuestas generadas y bloquea instantáneamente cualquier entrega que contenga dicho testigo de seguridad.

### ¿Por qué es necesario el saneamiento determinista de entrada si el modelo ya dispone de alineación de seguridad?

Los mecanismos de alineación interna de los modelos pueden esquivarse mediante lenguaje persuasivo complejo, codificaciones avanzadas o simulaciones de roles. El filtrado por expresiones regulares neutraliza los patrones conocidos antes de que los tokens ingresen al contexto, suprimiendo riesgos y optimizando el consumo computacional.

### ¿Cómo se integra este sistema de guardrails en agentes autónomos y arquitecturas de recuperación RAG?

El sistema supervisa la inferencia saneando tanto los fragmentos de documentos recuperados como las consultas de los usuarios antes de generar el prompt final. Este aislamiento evita que comandos maliciosos insertados en bases vectoriales alteren el razonamiento del modelo o activen llamadas inapropiadas a herramientas conectadas.

### ¿Cuál es el coste computacional y el impacto en la latencia que introduce esta capa de guardrails?

El componente en Python opera estrictamente en memoria local mediante expresiones regulares precompiladas y comparaciones nativas de cadenas, completando el análisis en menos de 2 milisegundos. Dado que no requiere consultas a APIs externas ni modelos evaluadores adicionales, no añade latencia de red a las aplicaciones en producción.

***

## Enlaces Útiles

* **Portal Corporativo:** https://ag.tavardt.com/mx/
* **Suite de Diagnósticos:** https://tools.tavardt.com/mx/
* **Contacto Técnico:** https://ag.tavardt.com/mx/contacto/

***

### Licencia
Distribuido bajo la Licencia MIT. Consulte [LICENSE](LICENSE) para obtener detalles completos.

***

© 2026 TAVARDT. Todos los derechos reservados.
