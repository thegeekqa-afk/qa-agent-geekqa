# Plantilla de Reporte de Prueba Exploratoria

## Descripción
Esta plantilla HTML profesional genera reportes de pruebas exploratorias completamente en **español** con estilos modernos y responsivos.

## Variables de Plantilla

La plantilla usa variables con sintaxis `{{NOMBRE}}` que deben reemplazarse con valores reales:

### Metadatos Principales
- `{{URL}}` - URL probada
- `{{PAGE_TITLE}}` - Título de la página HTML
- `{{TIMESTAMP}}` - Fecha y hora en formato legible (ej: 2026-04-09 20:45:00)

### Resumen Ejecutivo
- `{{OVERALL_STATUS}}` - Estado general (ej: "Página accesible y funcional")
- `{{SUMMARY_DESCRIPTION}}` - Descripción del resumen
- `{{AUTH_STATUS}}` - Estado de autenticación (ej: "Acceso directo sin autenticación requerida")
- `{{AUTH_ICON_CLASS}}` - Clase CSS para el icono de autenticación (success/warning/error/info)

### Secciones Condicionales
Las secciones entre `{{#if VARIABLE}}` y `{{/if}}` se muestran solo si la variable contiene datos.

- `CONSOLE_ERRORS` - Array de errores de consola
- `HEADINGS` - Array de encabezados encontrados
- `IMAGES` - Array de imágenes detectadas
- `VIDEOS` - Array de videos detectados
- `INTERACTIVE_ELEMENTS` - Array de elementos interactivos
- `INTERACTIONS` - Array de interacciones realizadas
- `SCREENSHOTS` - Array de capturas de pantalla

### Estructura de Arrays

#### CONSOLE_ERRORS
```json
[
  {
    "SEVERITY": "error|warning|info",
    "MESSAGE": "Descripción del error",
    "SOURCE": "origin/file.js:123"
  }
]
```

#### HEADINGS
```json
[
  {
    "TAG": "H1|H2|H3|etc",
    "TEXT": "Contenido del encabezado"
  }
]
```

#### IMAGES
```json
[
  {
    "SRC": "URL de la imagen",
    "ALT": "Texto alternativo",
    "BROKEN": false
  }
]
```

#### INTERACTIVE_ELEMENTS
```json
[
  {
    "TYPE": "LINK|BUTTON|INPUT|etc",
    "TEXT": "Texto visible",
    "HREF_OR_ATTRIBUTE": "href, onclick, etc"
  }
]
```

#### INTERACTIONS
```json
[
  {
    "TIMESTAMP": "20:45:15",
    "ACTION": "Clic en botón 'Siguiente'",
    "RESULT": "Carrusel avanzó a la siguiente categoría"
  }
]
```

#### SCREENSHOTS
```json
[
  {
    "PATH": "evidence/screenshot-initial.png",
    "CAPTION": "Página inicial completa"
  }
]
```

### Otros
- `{{BROKEN_COUNT}}` - Cantidad de imágenes rotas
- `{{FUNCTIONAL_FINDINGS}}` - Hallazgos funcionales (texto libre)
- `{{RISK_AREAS}}` - Áreas de riesgo identificadas (texto libre)
- `{{RECOMMENDATIONS}}` - Recomendaciones para pruebas adicionales (texto libre)

## Características de Diseño

### Responsive
- Se adapta automáticamente a dispositivos móviles
- Grid layout para galería de imágenes
- Tablas scrollables en pantallas pequeñas

### Accesibilidad
- Uso de semántica HTML correcta
- Contraste de colores WCAG AA
- Texto alternativo para imágenes
- Labels claras en tablas

### Profesionalismo
- Gradiente moderno en el encabezado (púrpura/violeta)
- Iconos de estado (✓, ✗, ●)
- Código incrustado (CSS) para portabilidad
- Colores consistentes (éxito verde, error rojo, advertencia naranja)

## Cómo Usar

1. **Reemplazar Variables:** Sustituye todas las variables `{{VARIABLE}}` con valores reales
2. **Ver Condicionales:** Solo incluye secciones si tienen datos (no dejes `{{#if}}` sin completar)
3. **Generar Rutas:** Asegúrate de que las rutas de screenshots sean relativas al HTML: `evidence/screenshot-name.png`
4. **Abrir en Navegador:** El HTML es completamente autocontenido, funciona offline

## Ejemplo de Uso

Para convertir el reporte de markdown a HTML, sigue estos pasos:

1. Extrae la información del reporte markdown
2. Mapea cada sección a las variables correspondientes
3. Reemplaza las variables en la plantilla
4. Guarda como `.html` en `3-outputs/run/exploratory/`

## Estilos Disponibles

El archivo incluye clases CSS para diferentes estados:

- `.success` - Color verde (#27ae60)
- `.error` - Color rojo (#f44336)
- `.warning` - Color naranja (#ff9800)
- `.info` - Color azul (#2196f3)

Personaliza los colores en la sección `<style>` si es necesario.
