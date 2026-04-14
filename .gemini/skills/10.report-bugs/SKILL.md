---
name: report-bugs
description: Analiza fallos de pruebas, genera reportes de errores y crea issues en Jira.
compatibility:
  gemini: ">=2.5.0"
metadata:
  version: 1.1.0
  author: Gemini
allowed-tools: "MCP Playwright"
---
# Habilidad: Reportar Errores

> **Condicional:** Esta habilidad se ejecuta automáticamente si `run-tests-browser` encuentra fallos en las pruebas. No necesita estar listada en `selectedSkills` del plan inicial.

## Cuándo Usar

Usa esta habilidad después de la ejecución de pruebas para analizar fallos y generar reportes de errores formales. Se activa automáticamente cuando el orquestador detecta fallos en `execution-report.json`.

## Entrada

- **Reporte de Ejecución:** El archivo `execution-report.json` del skill `execute-tests`, que contiene la lista de pruebas fallidas y rutas a evidencia.

## Pasos

1.  **Analizar Fallos**:
    -   Lee el archivo `execution-report.json` y filtra todas las pruebas con estado "FAILED".
    -   Si no se encuentran fallos, sale de la habilidad.
2.  **Generar Reportes de Errores**:
    -   Para cada prueba fallida, crea un reporte de error detallado usando la plantilla ubicada en `1-agent/.gemini/skills/10.report-bugs/templates/bug_template.md`.
    -   Asigna una severidad (Crítica, Alta, Media, Baja) basada en un análisis del fallo.
    -   Genera un ID de error único (ej. `BUG-{story-id}-{nn}`).
    -   Copia toda la evidencia asociada (capturas de pantalla, logs) en el directorio `bugs/evidence/`, renombrando los archivos para que coincidan con el ID del error.
3.  **Crear Issues en Jira (Opcional)**:
    -   Si un MCP de Jira está configurado, crea un nuevo issue en Jira para cada error.
    -   Pobla el issue con los detalles del error (título, severidad, pasos de reproducción, etc.) y adjunta la evidencia.
    -   Registra el `jiraKey` en el reporte de error local.
4.  **Guardar Reportes**:
    -   Guarda los reportes de errores consolidados como un archivo JSON único (`{story-id}-bugs.json`) y un archivo Markdown (`{story-id}-bugs.md`) en el directorio de salida `bugs`.

## Salida

-   **Reportes de Errores:** Un archivo JSON y un archivo Markdown en el directorio `3-outputs/run/{story-id}/bugs/`.
-   **Evidencia:** Todos los archivos de evidencia copiados a `3-outputs/run/{story-id}/bugs/evidence/`.

## Criterios de Finalización

- ✅ Un reporte `bugs.json` y `bugs.md` es creado si alguna prueba falló.
- ✅ Toda la evidencia para pruebas fallidas es copiada y renombrada en el directorio `bugs/evidence/`.
- ✅ Issues en Jira son creados para cada error (si está configurado).

## Si Falla

| Problema | Acción |
|----------|--------|
| Reporte de ejecución no encontrado | Registra un error y detiene. Esta habilidad no puede ejecutarse sin un reporte de ejecución de pruebas. |
| Archivos de evidencia faltantes | Registra una advertencia y crea el reporte de error con una nota de que la evidencia no fue encontrada. |
| MCP de Jira no disponible | Registra una advertencia, omite la creación en Jira, y completa la generación del reporte de error local. |
