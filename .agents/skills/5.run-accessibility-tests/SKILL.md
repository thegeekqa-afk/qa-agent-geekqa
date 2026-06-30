---
name: run-accessibility-tests
description: Realiza una auditoría de accesibilidad WCAG 2.2 AA usando axe-core.
compatibility:
  gemini: ">=2.5.0"
metadata:
  version: 1.1.0
  author: Gemini
allowed-tools: "MCP Playwright", "run_shell_command"
---
# Habilidad: Ejecutar Pruebas de Accesibilidad

> **Condicional:** Esta habilidad solo se ejecuta si `run-accessibility-tests` está listado en el arreglo `selectedSkills` del archivo `{story-id}-plan.json`.
> **Nota:** Esta habilidad usa el paquete `@axe-core/playwright` instalado localmente para ejecución de alta velocidad.

## Cuándo Usar

Usa esta habilidad para auditar una página web en busca de problemas de accesibilidad contra estándares WCAG 2.2 AA. La auditoría debe estar limitada a la página específica o componente descrito en la historia de usuario.

## Entrada

- **Plan de Pruebas:** El archivo `{story-id}-plan.json`, que contiene la URL objetivo.

## Pasos

1.  **Configuración y Navegación**:
    -   Extrae la URL objetivo del plan de pruebas.
    -   Lanza una instancia de navegador Playwright y navega a la URL.
    -   Si la página requiere Google Sign-In, usa las variables de entorno para iniciar sesión.

2.  **Ejecutar Auditoría Axe**:
    -   Inyecta la librería `axe-core` en la página.
    -   Ejecuta la auditoría para reglas WCAG 2.2 AA en múltiples breakpoints (ej. 375px, 768px, 1440px).

3.  **Realizar Verificaciones Manuales**:
    -   Verifica brevemente problemas no fácilmente capturados por automatización, como orden de encabezados y problemas de trampa de teclado.

4.  **Generar Reporte**:
    -   Todos los archivos temporales, incluyendo cualquier archivo de configuración, deben crearse dentro del directorio `3-outputs/run/{story-id}/accessibility/`.
    -   Agrega todas las violaciones de las auditorías.
    -   Crea un reporte JSON (`-report.json`) y un reporte Markdown legible por humanos (`-report.md`) detallando los hallazgos.

## Salida

-   **Reporte JSON:** `3-outputs/run/{story-id}/accessibility/{story-id}-report.json`.
-   **Reporte Markdown:** `3-outputs/run/{story-id}/accessibility/{story-id}-report.md`.
-   **Capturas de Pantalla:** Capturas de pantalla de cualquier elemento fallido se guardan en el mismo directorio.

## Criterios de Finalización

- ✅ La auditoría de accesibilidad se ejecuta exitosamente para todos los breakpoints especificados.
- ✅ Un reporte JSON y un reporte Markdown son generados en el directorio de salida `accessibility`.
- ✅ Capturas de pantalla son capturadas para cualquier elemento con violaciones.

## Si Falla

| Problema | Acción |
|----------|--------|
| Página inalcanzable | Registrar el error y marcar la habilidad como OMITIDA. |
| Inyección de script Axe falla | Reintentar navegación. Si falla nuevamente, documentar el error y omitir la auditoría. |
| Excepciones durante auditoría | Capturar la excepción, documentarla en el reporte, y continuar al siguiente breakpoint. |
