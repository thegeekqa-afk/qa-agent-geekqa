---
name: run-exploratory-tests
description: Ejecuta pruebas exploratorias mejoradas en una página web y genera reportes HTML en español.
compatibility:
 gemini: ">=2.5.0"
metadata:
 version: 2.0.0
 author: Gemini
 updated: 2026-04-09
allowed-tools: "Playwright MCP" 
---
# Habilidad: Ejecutar Pruebas Exploratorias Mejoradas

## Cuándo Usar
Usa esta habilidad para realizar una prueba exploratoria en profundidad en una página web. Esta habilidad es ideal para evaluar rápidamente las características principales, contenido y elementos interactivos de una página de inicio, especialmente cuando podría estar protegida por Google Sign-In.

## Prerrequisitos
La siguiente variable de entorno debe establecerse antes de ejecutar la habilidad:
- `TARGET_URL`: La URL completa de la página a probar.

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

## Pasos

1.  **Inicializar Entorno y Navegar.**
    -   Lee la `TARGET_URL` de las variables de entorno o CLI.
    -   Lanza una nueva página de navegador usando Playwright.
    -   Navega a la `TARGET_URL` y espera a que la página cargue.

2.  **Manejar Google Sign-In (Condicional).**
    -   Verifica si la URL de la página actual es `accounts.google.com` o si la página contiene elementos de formulario de Google Sign-In (ej. `input[type="email"]#identifierId`).
    -   Si se detecta una página de sign-in:
        -   Registra "Página de Google Sign-In detectada. No hay credenciales automáticas configuradas."
        -   Revisa si el flujo puede continuar sin autenticación o aborta la prueba.

3.  **Realizar Exploración Integral.**
    -   **Capturar Estado Inicial:** Toma una captura de pantalla de página completa llamada `screenshot-initial.png`.
    -   **Analizar Consola:** Registra cualquier error de consola.
    -   **Identificar Contenido Clave:**
        -   **Imágenes:** Encuentra todas las etiquetas `<img>` y registra su `src` y texto `alt`. Nota cualquier imagen rota.
        -   **Videos:** Encuentra todas las etiquetas `<video>` y registra su `src`.
        -   **Encabezados:** Lista todos los encabezados `<h1>`, `<h2>`, `<h3>` para entender la estructura de la página.
        -   **Elementos Interactivos:** Encuentra todos los `<a>`, `<button>`, y elementos con `role="button"` y registra su nombre/texto accesible.
    -   **Interactuar con Animaciones/Contenido Dinámico:**
        -   Intenta identificar e interactuar con componentes dinámicos comunes.
        -   **Carruseles/Sliders:** Busca botones que contengan texto/iconos "next" o "previous". Si se encuentra, haz clic en el botón "next", espera cualquier transición, y toma una captura de pantalla llamada `screenshot-carousel-interaction.png`.
        -   **Hovers:** Pasa el mouse sobre los primeros tres elementos interactivos identificados por 1 segundo cada uno para verificar cualquier animación o cambio de estado desencadenado por hover.

4.  **Reportar Resultados en HTML.**
    -   Genera un reporte HTML estructurado (`exploratory-report-{timestamp}.html`) que contenga todos los hallazgos con estilos profesionales:
        -   **Encabezado:** Título "Reporte de Prueba Exploratoria", URL Probada, Fecha/Hora, Título de la Página.
        -   **Resumen Ejecutivo:** Estado general de la exploración, si se detectó autenticación, errores críticos.
        -   **Sección de Errores de Consola:** Lista formateada con severidad (error, warning, info).
        -   **Contenido Identificado:** Secciones para encabezados, imágenes (con thumbnails si es posible), videos, y elementos interactivos.
        -   **Interacciones Realizadas:** Notas detalladas sobre intentos de interacción con timestamps.
        -   **Evidencia de Capturas:** Galería de screenshots con captions explicativas.
        -   **Conclusiones:** Hallazgos clave y áreas para pruebas adicionales.
    -   Incluye CSS incrustado para garantizar que el reporte sea autocontenido y profesional.

5.  **Limpieza.**
    -   Cierra el contexto del navegador.


## Salida

-   **Reporte HTML:** `3-outputs/run/exploratory/exploratory-report-{timestamp}.html`
-   **Evidencia:** Capturas de pantalla almacenadas en `3-outputs/run/exploratory/evidence/`.


## Criterios de Finalización

-   ✅ Navegó a la URL y manejó la autenticación si fue aplicable.
-   ✅ Realizó análisis de contenido integral e interacción.
-   ✅ Capturó todos los hallazgos y evidencia en un reporte detallado.
-   ✅ Generó capturas de pantalla para pasos clave.
-   ✅ Reporte HTML está formateado profesionalmente en español.

## Si Falla

| Problema | Acción |
|----------|--------|
| Variable `TARGET_URL` no definida | Solicita al usuario que proporcione la URL y reintenta. |
| Página requiere autenticación | Documenta el bloqueo en el reporte y proporciona un resumen parcial de los elementos visibles. |
| Error al generar HTML | Verifica que el directorio `3-outputs/run/exploratory/` existe y es escribible. |
| Playwright no puede abrir navegador | Reinicia el navegador o intenta con un puerto diferente. |
| Timeouts en carga de página | Aumenta el timeout a 60 segundos o documenta la página como inaccesible. |

## Notas de Implementación

### Estructura HTML del Reporte

Consulta la plantilla profesional en `templates/exploratory-report-template.html`. Esta plantilla incluye:

- Diseño responsive con CSS moderno
- Estilos incrustados para portabilidad
- Secciones condicionales para datos variables
- Iconos y colores para estados (éxito, error, advertencia)
- Galería de imágenes optimizada
- Tablas formateadas para legibilidad

Para más detalles sobre variables, estructura de datos y ejemplos, consulta `templates/README.md`.

### Estructura Recomendada del HTML Generado

El reporte debe contener estas secciones en orden:

1. **Encabezado** - Título, URL, Timestamp, Título de página
2. **Resumen Ejecutivo** - Estado general, autenticación, hallazgos críticos
3. **Errores de Consola** - Solo si hay errores
4. **Contenido Identificado** - Encabezados, imágenes, videos, elementos interactivos
5. **Interacciones Realizadas** - Detalles de clics, hovers, cambios observados
6. **Evidencia Visual** - Galería de capturas de pantalla
7. **Conclusiones** - Hallazgos, áreas de riesgo, recomendaciones
8. **Pie de Página** - Información de generación
