---
name: run-tests-browser
description: Ejecuta pruebas de navegación en navegador con todas las interacciones usando MCP Playwright.
compatibility:
  gemini: ">=2.5.0"
metadata:
  version: 1.0.0
  author: Gemini
allowed-tools: "MCP Playwright", "run_shell_command"
---
# Habilidad: Ejecutar Pruebas en Navegador

> **Condicional:** Esta habilidad solo se ejecuta si `run-tests-browser` está listado en el arreglo `selectedSkills` del archivo `{story-id}-plan.json`.

## Cuándo Usar

Esta habilidad se activa cuando se encuentra una URL en la historia de usuario y el plan de pruebas requiere interacción directa del navegador. Úsala para tareas que involucren:
- Explorar una página o flujo de usuario.
- Hacer clic en botones, enlaces y otros elementos.
- Llenar formularios.
- Tomar capturas de pantalla para evidencia.
- Navegación e interacción general.

## Herramientas Preferidas
Para cualquier interacción real del navegador, prefiere herramientas MCP Playwright sobre herramientas de obtención web.

Usa MCP Playwright cuando necesites:
- abrir páginas en un navegador
- hacer clic en botones, enlaces, menús, pestañas y controles
- llenar entradas y formularios
- esperar cambios de estado de UI
- capturar capturas de pantalla
- inspeccionar comportamiento renderizado
- validar páginas dinámicas que dependen de JavaScript

No confíes en herramientas web de solo texto para pruebas de UI interactivas.

## Herramientas MCP Playwright Preferidas

Prefiere estas herramientas cuando estén disponibles:
- playwright.open_page
- playwright.click
- playwright.fill
- playwright.wait_for
- playwright.screenshot
- playwright.evaluate
- playwright.select_option
- playwright.hover

Si los nombres exactos de herramientas difieren en este entorno, usa las acciones equivalentes más cercanas de MCP Playwright del navegador.

## Entrada

- **Plan de Pruebas:** El archivo `{story-id}-plan.json`, que contiene la URL objetivo y otro contexto.
- **Escenarios de Prueba:** El archivo `{story-id}-plan.json`, que contiene los casos de prueba detallados.
- **Historia de Usuario:** El archivo de historia de usuario que contiene la URL y descripción de la característica a probar.

## Pasos

1.  **Configuración y Navegación (Multi-Navegador)**:
    -   Extrae la URL objetivo de la historia de usuario o plan de pruebas.
    -   **Verificación Multi-Navegador**: Si se especifica en el plan de pruebas ejecutar entre navegadores, o si múltiples servidores MCP Playwright están configurados (ej. `playwright-chromium`, `playwright-firefox`, `playwright-webkit`), itera a través de CADA motor disponible.
    -   Lanza una instancia de navegador Playwright para el motor actual.
    -   Navega a la URL y espera a que la página alcance un estado de red inactivo (eventos `load`, `domcontentloaded`, y `networkidle`).

2.  **Manejar Sign-In (si es necesario)**:
    -   Si la página es un formulario de Google Sign-In, usa las variables de entorno para iniciar sesión.

3.  **Análisis Inicial de Página**:
    -   **Analizar Animaciones:** Espera a que cualquier animación inicial de carga de página se complete. Usa `page.waitForTimeout()` o selectores más específicos si ciertos elementos animados son conocidos.
    -   **Verificar Errores de Consola:** Escucha y registra cualquier error crítico de consola (ej. `console.error`) que ocurra durante la carga de página o interacción inicial. Falla la prueba si se encuentran errores críticos.

4.  **Ejecutar Escenarios de Prueba**:
    -   Lee y analiza el archivo `{story-id}-plan.json`.
    -   Para cada escenario de prueba:
        -   Registra el inicio del caso de prueba (ej. "Ejecutando TC_C001: Verificar video 'Handshake'...").
        -   Usa acciones MCP Playwright (`click`, `fill`, `waitFor`, etc.) para realizar los pasos descritos en el escenario.
        -   Después de cada interacción significativa, espera a que cualquier animación o transición resultante se complete. Esto puede ser una combinación de `page.waitForLoadState('networkidle')`, `page.waitForSelector`, y `page.waitForTimeout`.
        -   Captura evidencia (capturas de pantalla, logs de consola) para pasos clave y en fallo.

5.  **Generar Reporte**:
    -   Todos los archivos temporales y evidencia deben crearse dentro del directorio `3-outputs/run/{story-id}/execution/`.
    -   Crea un archivo único `execution-report.json` resumiendo los resultados (acciones realizadas, hallazgos, estado de cada caso de prueba) y enlazando a evidencia capturada.
    - si hay fallos documentarlos en el reporte de ejecución para que el orquestador pueda decidir ejecutar `report-bugs`.

## Salida

-   **Reporte de Ejecución:** `3-outputs/run/{story-id}/execution/execution-report.json`.
-   **Archivos de Evidencia:** Capturas de pantalla y logs guardados en el directorio `3-outputs/run/{story-id}/execution/`.

## Criterios de Finalización

- ✅ El agente navegó exitosamente la URL y realizó las interacciones descritas.
- ✅ Un archivo `execution-report.json` es generado en el directorio de salida `execution`.
- ✅ Evidencia es capturada para todos los pasos relevantes.
