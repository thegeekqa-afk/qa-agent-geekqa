---
name: generate-automated-tests
description: Genera casos de prueba ejecutables a partir de un plan de pruebas.
compatibility:
  gemini: ">=2.5.0"
metadata:
  version: 1.1.0
  author: Gemini
allowed-tools: ""
---
# Habilidad: Generar Pruebas Automatizadas

## Cuándo Usar

Usa esta habilidad para convertir escenarios definidos en un plan de pruebas en casos de prueba ejecutables.

## Entrada

- **Metadatos del Plan JSON:** El archivo `{story-id}-plan.json`. La habilidad solo se ejecutará si `generate-automated-tests` está presente en el arreglo `selectedSkills`.

## Pasos

1.  **Verificar si se Omite**: Salir inmediatamente si `generate-automated-tests` no está en el arreglo `selectedSkills` del plan JSON.
2.  **Extraer Escenarios**: Analizar los escenarios del archivo `{story-id}-plan.json`.
3.  **Generar Definiciones de Prueba**: Para cada escenario, crear una definición de caso de prueba (nombre, pasos, resultado esperado) usando técnicas ISTQB (ej. caminos positivos/negativos, valores límite).
4.  **Crear Archivo de Prueba**: Generar un archivo de prueba Playwright (ej. `{story-id}.spec.ts`) a partir de las definiciones de prueba.
5.  **Publicar en Google Sheets (Opcional)**: Si un MCP de Google Sheets está configurado, agregar cada caso de prueba como una nueva fila con estado "pending".

## Salida

- **Primaria:** Un archivo de prueba Playwright ubicado en `3-outputs/run/{story-id}/tests/{story-id}.spec.ts`.
- **Secundaria:** Si está configurado, nuevas filas agregadas a una hoja de Google.

## Criterios de Finalización

- ✅ Un archivo `{story-id}.spec.ts` es creado en el directorio `tests`.
- ✅ El archivo de prueba contiene sintaxis Playwright válida.
- ✅ Todos los escenarios del plan están cubiertos por un caso de prueba.

## Si Falla

| Problema | Acción |
|----------|--------|
| Archivo de plan no encontrado | Registrar un error y detener, ya que esta habilidad no puede proceder sin un plan. |
| Sintaxis TypeScript inválida | Usar un linter para identificar y corregir errores en el archivo de prueba generado. |
| Datos de prueba faltantes | Usar datos placeholder y agregar un comentario `// TODO:` para indicar dónde se necesitan datos reales. |
