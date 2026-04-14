---
name: qa-test-planner
description: Crea un plan de pruebas integral basado en el contexto empresarial y el análisis de historias de usuario.
compatibility:
  gemini: ">=2.5.0"
metadata:
  version: 2.0.0
  author: Gemini
allowed-tools: "list_directory read_file"
---

# Habilidad: Planificador de Pruebas QA
Crea un plan de pruebas integral basado en el contexto empresarial y el análisis de historias de usuario.

## Cuándo Usar

Activado por su nombre (o nombre parcial) o como el primer paso en un comando personalizado/de barra. Esta habilidad es responsable de crear un plan de pruebas integral.

## Entrada

- **Ruta de Carpeta de Activos:** Una carpeta que contiene la historia de usuario, criterios de aceptación y cualquier activo relacionado (`.txt`, `.md`, `.pdf`, `.png`, `.jpg`).

## Pasos

1.  **Analizar Contexto Empresarial.**
    -   Usa `list_directory` y `read_file` para procesar todos los archivos en la carpeta `2-stories/project-context` para entender el contexto empresarial general.

2.  **Analizar Memoria del Producto.**
    -   Usa `list_directory` y `read_file` para procesar todos los archivos en la carpeta `3-outputs/memory` para entender los flujos críticos del producto, áreas inestables y errores comunes.
    -   Esta información se usará para informar el plan de pruebas.

3.  **Analizar Activos de la Historia y Extraer URLs.**
    -   Usa `list_directory` y `read_file` para procesar todos los activos de texto y visuales en la carpeta de entrada.
    -   Consolida el contenido para extraer el ID de la Historia, Título, Descripción, Criterios de Aceptación, e identificar elementos clave de UI y flujos de usuario.
    -   Identifica todas las URLs mencionadas en los activos de la historia, valídalas e inclúyelas en el plan de pruebas.

4.  **Analizar y Asociar Activos.**
    -   Usando las capacidades multimodales de Gemini, analiza los activos (imágenes, PDFs, etc.) y asócialos con la historia de usuario por su nombre o contenido.
    -   Para cada activo, genera un objeto con su `name`, `path`, y `relation` a la historia de usuario.
    -   Esta información se usará para poblar la sección 'Activos Analizados' del plan de pruebas.

5.  **Definir Estrategia de Pruebas y Seleccionar Habilidades.**
    -   Basado en el contexto empresarial, la historia de usuario y el análisis de contenido de URL, realiza un análisis de riesgo (ISTQB) para determinar los tipos de pruebas necesarios (ej. funcional, accesibilidad, visual, analíticas).
    -   Para cada tipo de prueba requerido, agrega el nombre de la habilidad correspondiente a una lista `selectedSkills`. Proporciona una breve justificación para cada selección.
    -   **Determinar Estrategia de Navegador**: Decide si probar en múltiples motores de navegador (Chromium, Firefox, Webkit) basado en el tipo de historia, riesgo (ej. UI compleja, dependencias de estilo), y contexto del proyecto. Por defecto a `["chromium"]` si no se encuentran elementos de UI de alto riesgo.
    -   La justificación debe ser detallada y alineada con el conocimiento del agente de la historia de usuario y contexto del proyecto.
    -   Las habilidades `log-results` deben siempre incluirse.
    -   **Definir Skills Condicionales**: Configura `conditionalSkills` para skills que se ejecutan solo bajo ciertas condiciones:
        - `report-bugs`: Se ejecuta automáticamente si `run-tests-browser` encuentra fallos
        - Otros skills condicionales pueden agregarse según necesidad

6.  **Generar Escenarios de Prueba.**
    -   Basado en la historia de usuario, criterios de aceptación y activos analizados, genera una lista de escenarios de prueba.
    -   Cada escenario debe incluir un `title` y una `description` detallada de los pasos para validar la característica.
    -   Los escenarios deben cubrir requisitos funcionales, comportamiento UI/UX, y casos extremos potenciales.
    -   Sigue las mejores prácticas descritas en `references/test-case-best-practices.md`.

7.  **Generar Archivos del Plan de Pruebas.**
    -   Produce un reporte HTML detallado (`-plan.html`) usando la plantilla en la carpeta `templates`, incluyendo los escenarios de prueba generados.
    -   Genera un archivo de metadatos JSON (`-plan.json`) que contiene el arreglo `selectedSkills`, arreglo `testScenarios`, y cualquier otra información clave para habilidades posteriores.

## Habilidades Disponibles para el Plan de Pruebas
Cuando selecciones habilidades para el plan de pruebas, elige de la siguiente lista:
- `run-tests-browser`
- `run-accessibility-tests`
- `run-lighthouse-tests`
- `run-visual-tests`
- `run-analytics-tests`
- `generate-automated-tests`

## Salida

- **Plan de Pruebas HTML:** `3-outputs/run/{story-id}/planning/{story-id}-plan.html`
- **Metadatos JSON:** `3-outputs/run/{story-id}/planning/{story-id}-plan.json` (Este archivo impulsa la ejecución condicional de habilidades posteriores. Debe incluir arreglos `selectedSkills`, `conditionalSkills`, `testScenarios`, y cualquier otra información clave para habilidades posteriores).

## Criterios de Finalización

- ✅ Un plan de pruebas HTML es generado desde la plantilla.
- ✅ Un archivo `plan.json` con arreglos `selectedSkills` y `testScenarios` es creado.
- ✅ Ambos archivos son guardados en la carpeta de planificación correcta.

## Si Falla

| Problema | Acción |
|----------|--------|
| Activos de historia ilegibles | Solicita al usuario la ruta correcta y reintenta. |
| Ninguna habilidad parece aplicable | Por defecto a `["run-tests-browser", "log-results"]` y documenta la razón. |
| Error de escritura de salida | Verifica que el directorio de salida existe y es escribible. |
| Contexto empresarial no encontrado | Continúa con el análisis de la historia de usuario y documenta el contexto faltante en el plan. |

## Notas Importantes

### Para el Orquestador

El archivo `{story-id}-plan.json` DEBE tener esta estructura mínima para que el orquestador funcione:

```json
{
  "storyId": "US_001",
  "url": "https://example.com",
  "selectedSkills": [
    "run-tests-browser",
    "run-accessibility-tests",
    "log-results"
  ],
  "conditionalSkills": {
    "report-bugs": {
      "condition": "run-tests-browser has failures",
      "dependsOn": "run-tests-browser",
      "description": "Genera reportes de bugs si se encuentran fallos en las pruebas funcionales"
    }
  },
  "selectedBrowsers": ["chromium"],
  "testScenarios": [
    {
      "id": "SC_001",
      "title": "Descripción del escenario",
      "description": "Pasos detallados..."
    }
  ],
  "riskAnalysis": {
    "overallRisk": "HIGH|MEDIUM|LOW",
    "justification": "..."
  }
}
```

**El orquestador ejecutará:**
1. **Skills Iniciales**: Los listados en `selectedSkills`
2. **Skills Condicionales**: Solo si se cumplen las condiciones especificadas en `conditionalSkills`
3. **Siempre**: `log-results` al final para consolidar resultados

