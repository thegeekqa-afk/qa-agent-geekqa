# Mejores Prácticas para la Generación de Casos de Prueba

## 1. Analizar Criterios de Aceptación y Contexto Empresarial

- Analizar criterios de aceptación y contexto empresarial si se agregan.

## 2. Seguir Mejores Prácticas de Pruebas

- No incluir casos relacionados con validación pixel-perfect o UI estática.
- Cada caso de prueba debe ser parte de un solo grupo de pruebas.
- Debes escribirlos paso a paso, estructurados y detallados.
- Los casos de prueba deben ser creados considerando los requisitos del cliente.
- Los casos de prueba deben ser precisos, claros y directos.
- Usar lenguaje asertivo al escribir los casos de prueba.
- NO PRESUMIR: Siempre autorizar los casos de prueba según la especificación de requisitos.
- Si algún caso de prueba requiere ejecutar los mismos pasos que otro caso de prueba, llamar a ese caso de prueba por su ID en la columna de prerrequisitos.
- Asegurar que todos los requisitos del cliente sean cumplidos al autorizar el caso de prueba.
- Proporcionar prerrequisitos claros e instrucciones de configuración.
- Cuando sea necesario, si los casos de prueba son parte de un flujo, agruparlos en un caso de prueba con múltiples resultados esperados por paso.

## 3. Incluir Técnicas de Pruebas

Incluir Técnicas de Pruebas según la complejidad de la historia de usuario.
- Análisis de Valores Límite
- Particionamiento en Clases de Equivalencia
- Técnicas de Transición de Estados
- Tablas de Decisión
- Técnica de Adivinación de Errores

## 4. Incluir un Grupo de Pruebas

Incluir un Grupo de Pruebas según la complejidad y alcance de la historia de usuario:

### Pruebas Funcionales
- Lógica de Negocio
- Validación de Datos
- Manejo de Errores
- Puntos de Integración

### Pruebas de Compatibilidad
- Versión del Navegador
- Versión del Dispositivo
- Sistemas Operativos
- Resoluciones de Pantalla

### Pruebas de Rendimiento
- Requisitos de Tiempo de Carga
- Límites de Tiempo de Respuesta
- Monitoreo de Uso de Recursos
- Soporte de Usuarios Concurrentes

### Pruebas de Seguridad
- Validación de Entrada
- Autenticación
- Autorización
- Protección de Datos
- Seguridad de API

## 5. Incluir Casos de Prueba Positivos y Negativos

## 6. Identificar Casos de Prueba Necesarios por Historia de Usuario

## 7. Revisar que los Casos de Prueba Cumplan con Criterios de Aceptación y Contexto Empresarial

## 8. Formato de Salida

Salida de los casos de prueba con la siguiente información para cada caso:
- **Grupo de Pruebas:** (Elegir de la lista proporcionada)
- **Título:** (Una descripción concisa de la prueba. No agregar ID de caso de prueba o prefijo numerado como 'TC_001_'.)
- **Pre-condiciones:** (Cualquier paso de configuración requerido antes de la prueba)
- **Pasos del Caso de Prueba:** (Una lista numerada de acciones a realizar, presentada claramente y secuencialmente, usando lenguaje natural. Ver el ejemplo abajo para inspiración en estructura y nivel de detalle.)
- **Resultados Esperados de la Prueba:** (El resultado esperado después de realizar los pasos)
- **Datos de prueba:** (Cualquier dato específico necesario para la prueba; especificar si no aplica)
- **Indicador de Notebook QA de Chrome:** (Número de página si es relevante, de lo contrario N/A)

## 9. Estilo y Tono

- Enfocarse en claridad y concisión, y evitar redundancia en las respuestas.

### Ejemplo de estilo deseado para Pasos del Caso de Prueba (para inspiración, no replicar exactamente):
1. Este componente es un video que está centrado y con animación basada en scroll...
2. Esta animación basada en scroll también activa el video para que comience...
3. El video crece, pero el elemento arriba de él no desaparece detrás de él...
(y así sucesivamente)
