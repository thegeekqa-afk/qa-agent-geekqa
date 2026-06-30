# QA Agent GeekQA

Este repositorio contiene un prototipo de agente de QA orientado a validar aplicaciones web mediante Playwright, accesibilidad y reportes automatizados. El flujo está guiado por las instrucciones de [GEMINI.md](GEMINI.md) y por la configuración de [.gemini/mcp_config.json](.gemini/mcp_config.json).

Este recurso forma parte del curso de IA para QA y automatización disponible en Udemy: https://www.udemy.com/course/ai-powered-qa-automatizacion-y-agentes-inteligentes-en-qe/

## Qué incluye este proyecto

- Planificación de pruebas basada en historias de usuario.
- Ejecución de pruebas funcionales, accesibilidad y Lighthouse.
- Generación de artefactos en carpetas estructuradas bajo [3-outputs](3-outputs).
- Integración con Antigravity CLI y servidores MCP de Playwright.

## Requisitos previos

- Git instalado
- Node.js 18+ recomendado
- npm
- Antigravity CLI instalado
- Acceso a la terminal o PowerShell
- 1 + mas cuentas de correo de Gmail para autenticación en Antigravity CLI

## Instalación

1. Clona el repositorio:
   ```bash
   git clone <url-del-repositorio>
   ```
2. Entra en la carpeta del proyecto:
   ```bash
   cd qa-agent-geekqa
   ```


## Estructura del proyecto

- [2-inputs](2-inputs): archivos de entrada, historias de usuario y materiales de contexto.
- [3-outputs](3-outputs): resultados generados por el flujo de QA.
- [.agent](.gemini): configuración de Antigravity y MCP.
- [GEMINI.md](GEMINI.md): instrucciones principales para el agente.

## Uso recomendado

1. Prepara la entrada en [2-inputs](2-inputs).
2. Asegura que [GEMINI.md](GEMINI.md) y [.gemini/mcp_config.json](.gemini/mcp_config.json) estén presentes.
3. Desde la raíz del proyecto, ejecuta Antigravity CLI:
   ```bash
   agy --dangerously-skip-permissions
   ```
4. realice setup inicial de Antgravity, permisos etc y autenticatión con tu cuenta de correo de Gmail
5. El agente leerá las instrucciones y generará resultados en [3-outputs/run](3-outputs/run).

## Salida esperada

Los artefactos de QA se almacenan normalmente en carpetas por story o caso, por ejemplo:

- [3-outputs/run](3-outputs/run)/{story-id}/planning
- [3-outputs/run](3-outputs/run)/{story-id}/execution
- [3-outputs/run](3-outputs/run)/{story-id}/accessibility
- [3-outputs/run](3-outputs/run)/{story-id}/bugs
- [3-outputs/run](3-outputs/run)/{story-id}/test-report.html

## Notas

- La carpeta [.gemini](.gemini) se mantiene en el repositorio para que Antigravity pueda leer la configuración del proyecto.
- Si tu entorno requiere permisos adicionales, usa el comando mostrado arriba con la bandera correspondiente.

## Copyright

Recurso creado por GeekQA con fines educativos.
