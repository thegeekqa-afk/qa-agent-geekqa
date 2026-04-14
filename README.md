# QA Agent GeekQA

Este proyecto es un agente de QA prototipo que planea, diseña  y ejecuta pruebas para apps web en el navegador. El agente usa skills de QA, pruebas exploratorias, pruebas de accesibilidad y generación de reportes, y se apoya en configuraciones y flujos definidos en GEMINI.md `.gemini`. 

Esto es un recurso  que se explica en el curso de Ai-powered-QA que puedes encontrar en Udemy aquí https://www.udemy.com/course/ai-powered-qa-automatizacion-y-agentes-inteligentes-en-qe/ 

## Prerrequisitos

- Git instalado
- Node.js instalado (recomendado 16+)
- Acceso a la terminal o PowerShell

## Instalación

1. Clona el repositorio:
   ```bash
   git clone <url-del-repositorio>
   ```
2. Entra en la carpeta del proyecto:
   ```bash
   cd qa-agent-udemy
   ```
3. Instala dependencias si el proyecto usa Node.js (verificar `package.json`):
   ```bash
   npm install
   ```

## Setup

- Revisa la carpeta `2-inputs/` para los datos de entrada y `3-outputs/` para resultados generados.
- Si hay configuración específica en `.gemini`, deja esa carpeta tal cual para que se suba a GitHub.

## Uso

- Abre el proyecto en tu editor favorito.
- Ejecuta los scripts o comandos que correspondan según el stack del proyecto.

## Notas

- El archivo `.gitignore` se ha ajustado para que la carpeta `.gemini` no se ignore y pueda subirse a GitHub.
