# Memoria de Producto - UniwebPro

## Historial de Validaciones y Calidad

### US-003: Gestión de Materias (2026-06-29)
* **Estado:** FAIL (con advertencias y hallazgos críticos de accesibilidad)
* **Áreas Inestables:**
  - **Accesibilidad del Formulario:** Las etiquetas `<label>` de los campos de entrada en el modal "Nueva Materia" carecen de atributos `for` e `id` asociados, lo que representa una barrera crítica de accesibilidad (WCAG 2.1 AA).
  - **Contraste de Color:** El rol de usuario ("Administrador") en la parte inferior de la barra de navegación lateral presenta una baja relación de contraste (WCAG 2.1 AA).
* **Errores de Automatización y Pruebas comunes:**
  - **Manejo de Diálogos:** La acción de eliminación en la tabla general despliega un cuadro de confirmación nativo del navegador (`confirm`), el cual debe ser escuchado e interactuado de manera explícita en Playwright mediante `page.once('dialog', ...)` para evitar la cancelación por defecto.
  - **Unicidad de Datos:** Registrar materias con códigos duplicados (como `MAT-502`) no completa la transacción y mantiene el modal abierto en la UI, bloqueando la interacción con la tabla.
