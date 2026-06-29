---
name: run-lighthouse-tests
description: Ejecuta una auditoría Lighthouse para rendimiento, accesibilidad y SEO.
compatibility:
  gemini: ">=2.5.0"
metadata:
  version: 1.1.0
  author: Gemini
allowed-tools: "MCP Playwright", "lighthouse performance analysis", "run_shell_command"
---
# Habilidad: Ejecutar Pruebas Lighthouse

> **Condicional:** Esta habilidad solo se ejecuta si `run-lighthouse-tests` está listado en el arreglo `selectedSkills` del archivo `{story-id}-plan.json`.

## Cuándo Usar

Usa esta habilidad para auditar una página web para métricas de rendimiento, accesibilidad, mejores prácticas y SEO.

## Entrada

- **Plan de Pruebas:** El archivo `{story-id}-plan.json`, que contiene la URL objetivo.

## Pasos

1.  **Configuración y Navegación**:
    -   Extrae la URL objetivo del plan de pruebas.
    -   Lanza una instancia de navegador Playwright y navega a la URL.
    -   Si la página requiere Google Sign-In, usa las variables de entorno para iniciar sesión.

2.  **Ejecutar Auditoría Lighthouse**:
    -   Ejecuta la auditoría Lighthouse contra el estado actual de la página usando la API programática (ej. `playwright-lighthouse`).
    -   Configúrala para ejecutar las categorías de rendimiento, accesibilidad, mejores prácticas y SEO.

3.  **Generar Reporte**:
    -   Todos los archivos temporales, incluyendo cualquier archivo de configuración, deben crearse dentro del directorio `3-outputs/run/{story-id}/lighthouse/`.
    -   Guarda la salida JSON cruda de Lighthouse a un archivo `{story-id}-report.json`.
    -   Opcionalmente, crea un reporte HTML más legible por humanos también.

## Salida

-   **Reporte JSON:** `3-outputs/run/{story-id}/lighthouse/{story-id}-report.json`.
-   **Reporte HTML (Opcional):** `3-outputs/run/{story-id}/lighthouse/{story-id}-report.html`.

## Criterios de Finalización

- ✅ La auditoría Lighthouse se completa exitosamente.
- ✅ Un reporte JSON es generado en el directorio de salida `lighthouse`.

## Si Falla

| Problema | Acción |
|----------|--------|
| Lighthouse no instalado | Registrar un error y detener. Asegúrate de que Lighthouse sea una dependencia del proyecto. |
| Página inalcanzable | Registrar el error y marcar la habilidad como OMITIDA. |
| Auditoría se agota | Aumentar la configuración de timeout y reintentar. |
