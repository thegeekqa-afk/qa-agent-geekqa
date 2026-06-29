---
name: log-results
description: Registra resultados y prepara un reporte integral de pruebas HTML.
compatibility:
  gemini: ">=2.5.0"
metadata:
  version: 1.0.0
  author: Gemini
allowed-tools: ""
---
# Habilidad: Registrar Resultados y Preparar Reporte de Pruebas HTML

*Esta habilidad siempre se ejecuta – la única etapa obligatoria al final de cada ejecución.*

## Cuándo Usar

Después de que todas las pruebas automatizadas y auditorías seleccionadas hayan terminado, incluso si algunas fueron omitidas por el plan. Esta habilidad genera un reporte HTML integral y profesional resumiendo toda la ejecución.

## Entrada

Esta habilidad consolida **TODOS** los outputs de las habilidades ejecutadas:

- **Ejecución / Funcional**: `3-outputs/run/{story-id}/execution/execution-report.json` (casos de prueba, pasos, resultados)
- **Accesibilidad**: `3-outputs/run/{story-id}/accessibility/` (archivos JSON de axe-core, reportes por breakpoint)
- **Lighthouse**: `3-outputs/run/{story-id}/lighthouse/{story-id}-lighthouse.json` y `.html`
- **Bugs/Errores**: `3-outputs/run/{story-id}/bugs/{story-id}-bugs.json` y `{story-id}-bugs.md` (con evidencia en `bugs/evidence/`)
- **Planificación**: `3-outputs/run/{story-id}/planning/{story-id}-plan.json` y `{story-id}-plan.html`
- **Plantilla HTML**: `templates/test-report-template.html`

## Pasos

1.  **Calcular Tiempo de Ejecución**: Calcular el tiempo total en minutos que transcurrió desde que inició la ejecución desde el CLI del primer skill hasta que termina completamente el último skill ejecutado, se genera el reporte y se finaliza la ejecución.

2.  **Consolidar Datos de TODOS los Skills**:
    -   **Ejecución Funcional**: Lee `execution-report.json`, extrae: casos ejecutados, pasados/fallidos, pasos, evidencia.
    -   **Accesibilidad**: Lee archivos JSON de axe-core de cada breakpoint (mobile 375px, tablet 768px, desktop 1440px), consolida todas las violaciones y pasus.
    -   **Lighthouse**: Lee `{story-id}-lighthouse.json`, extrae puntajes, oportunidades, diagnósticos.

    -   **Bugs/Report-Bugs**: Lee `{story-id}-bugs.json` y `{story-id}-bugs.md`, extrae:
        - IDs de bugs (ej. BUG-US_001-01)
        - Severidad (Crítica, Alta, Media, Baja)
        - Descripción y pasos de reproducción
        - Rutas a evidencia en `bugs/evidence/`
        - JIRA keys (si existe)


4.  **Analizar Escenarios de Prueba**: 
    -   Lee `planning/{story-id}-plan.json` y extrae `testScenarios` array.
    -   Mapea cada resultado en `detailedResults` con su escenario usando ID.

5.  **Aplicar Reglas de Fallos Automáticos**:
    -   ❌ Si **algún Lighthouse < 80** (cualquier categoría): marcar como FAIL
    -   ❌ Si **existel bugs.json con items**: marcar todos esos bugs como FAIL
    -   ❌ Si **cualquier prueba funcional estado = FAILED**: contar como fallo
    -   ❌ Si **violaciones críticas A11y**: marcar como FAIL
    -   **Veredicto General**: PASS solo si todos los checks pasan sin Bugs críticos

6.  **Cargar y Poblar Plantilla HTML**:
    -   Lee `templates/test-report-template.html`.
    -   Reemplaza placeholders con:
        - Información de historia (URL, Title, Story ID)
        - Tabla `detailedResults` con todas las filas consolidadas
        - Sección de Bugs con enlace a cada evidencia
        - Sección de Accesibilidad con resumen por breakpoint
        - Sección de Lighthouse con gráficos de puntajes
        - Veredicto final PASS/FAIL con color

7.  **Generar Tabla de Resumen**:
    -   Tabla con 4 columnas sumarias:
        | Tipo | Ejecutado | Pasados | Fallidos |
        |------|-----------|---------|----------|
        | Funcional | 12 | 11 | 1 | ← datos de execution-report
        | Accesibilidad (3 breakpoints) | 3 auditorías | Todas limpias o # violaciones | # críticas |
        | Lighthouse | 6 categorías | # >= 80 | # < 80 |
        | Bugs Encontrados | - | - | 5 |
        | **TOTAL** | todos los anteriores | X% éxito | X% fallo |

9.  **Incluir Enlaces a Reportes Originales**:
    -   En cada sección, incluir enlaces a los reportes HTML originales:
        - "Ver Reporte Lighthouse completo" → `lighthouse/{story-id}-lighthouse.html`
        - "Ver Reporte Bugs en detalle" → `bugs/{story-id}-bugs.md`
        - "Ver Plan de Pruebas" → `planning/{story-id}-plan.html`

10. **Incrustación e Interactividad**:
    -   Todo CSS incrustado (autocontenido).
    -   Tablas con capacidad de filtrado/orden por tipo o status.
    -   Iconos que indiquen colores: ✅ PASS, ❌ FAIL, ⚠️ WARNING, ℹ️ INFO

11. **Escribir Archivo HTML**:
    -   Guarda a `3-outputs/run/{story-id}/test-report.html`

12. **Actualizar Memoria del Producto**:
    -   Basado en bugs encontrados y áreas inestables, actualiza:
        - `3-outputs/memory/product-memory.md` (nuevas áreas inestables, errores comunes)
        - `3-outputs/memory/api-map.json` (nuevos endpoints si aplica)
        - `3-outputs/memory/ui-map.json` (nuevos elementos UI, interacciones)

13. **Registrar Veredicto en Consola**:
    -   Imprime en consola con formato:
        ```
        ═════════════════════════════════════════
        REPORTE QA - {STORY_ID}
        ═════════════════════════════════════════
        ✅ PASS | ❌ FAIL
        
        Resumen:
        • Casos Ejecutados: X/Y (Z%)
        • Bugs Encontrados: N
        • Tiempo Total (inicio CLI a fin): {MM} minutos
        • Reporte: 3-outputs/run/{story-id}/test-report.html
        ═════════════════════════════════════════
        ```

## Salida

- **Artefacto Primario**: `3-outputs/run/{story-id}/test-report.html` (reporte consolidado con TODOS los resultados)
- **Actualización de Memoria**: `3-outputs/memory/product-memory.md`, `api-map.json`, `ui-map.json`
- **Mensaje de Consola**: Veredicto final PASS/FAIL con resumen

## Si Falla

| Problema | Acción |
|----------|--------|
| No hay directorios de ejecución | Crea un reporte vacío pero válido indicando que no se ejecutó ningún skill |
| Archivos JSON malformados | Parsea lo que puedas; registra advertencias para campos inválidos |
| Bugs.json no existe | Continúa; simplemente omite la sección de bugs del reporte |
| Plantilla HTML no encontrada | Crea un reporte HTML básico sin usar plantilla, pero asegúrate que sea válido |
| Memoria no actualizable | Registra advertencia pero continúa; la memoria es secundaria |

## Notas Importantes

### Consolidación de Bugs en Reporte

El reporte final debe destacar claramente cualquier bug encontrado:

1. **Tabla Principal** incluye todos los bugs con:
   - ID (ej. BUG-US_001-01)
   - Tipo: "Bug"
   - Severidad (Crítica/Alta/Media/Baja)
   - Status: FAIL (todos los bugs son fallos)
   - Evidencia: enlaces a screenshots en `bugs/evidence/`

2. **Sección Dedicada de Bugs**:
   - Lista de todos los bugs con descripción completa
   - Pasos de reproducción
   - Severidad visual (colores)
   - JIRA key (si está disponible)
   - Enlaces directos a evidencia

3. **Cálculo de Veredicto**:
   ```
   Si existen bugs CRÍTICOS o ALTOS:
     → Veredicto = ❌ FAIL
   Si no hay bugs y todos los tests PASS:
     → Veredicto = ✅ PASS
   Si hay bugs MEDIA/BAJA pero tests pasan:
     → Veredicto = ⚠️ PASS WITH WARNINGS
   ```

## Criterios de Finalización

- ✅ `test-report.html` es generado y es un archivo visualizable autocontenido.
- ✅ Tabla `detailedResults` consolidada incluye items de TODOS los skills (Funcional, Accesibilidad, Lighthouse, Visual, Bugs).
- ✅ Sección de Bugs muestra todos los items del archivo `{story-id}-bugs.json` con severidad y enlaces a evidencia.
- ✅ Tabla de Resumen Sumaria tiene 4 columnas: Tipo, Ejecutado, Pasados, Fallidos.
- ✅ Veredicto final refleja correctamente: PASS si no hay bugs críticos Y todos los tests pasan; FAIL en caso contrario.
- ✅ Enlaces a reportes originales (Lighthouse HTML, Bugs MD, Plan HTML) están presentes y funcionan.
- ✅ CSS incrustado; el archivo es 100% autocontenido sin dependencias externas.
- ✅ Mensaje de consola muestra veredicto PASS/FAIL y ruta al reporte.
- ✅ Memoria del Producto actualizada con nuevos bugs/areas inestables.
