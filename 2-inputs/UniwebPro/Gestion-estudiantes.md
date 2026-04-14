# Historia de Usuario: Gestión de Estudiantes

**Título:** Gestión Integral de Datos de Estudiantes en UniwebPro

- **URL**: https://uniwebpro.geekqa.net/
- **Rol**: Administrador Académico
## Descripción

**Como** Administrador del sistema UniwebPro  
**Quiero** poder registrar, buscar, visualizar, editar y eliminar la información de los estudiantes  
**Para** mantener un control actualizado, centralizado y preciso de las admisiones, datos personales y académicos de la comunidad universitaria.

---

## Criterios de Aceptación (AAC - Acceptance Criteria)

### AAC 1: Visualización del Listado de Estudiantes
- **Dado** que hay estudiantes registrados en el sistema,
- **Cuando** el usuario ingresa al módulo de "Estudiantes",
- **Entonces** debe visualizar una tabla que contenga las siguientes columnas:
  - **Estudiante:** Foto/Avatar, Nombres, Apellidos y Correo Electrónico.
  - **Documento:** Tipo de documento y Número de documento.
  - **Carrera:** Nombre de la carrera asociada (mostrada en formato etiqueta/badge).
  - **Contacto:** Número de teléfono.
  - **Acciones:** Botones para Editar y Eliminar.

### AAC 2: Búsqueda y Filtrado
- **Dado** que el usuario se encuentra en la vista principal de Estudiantes,
- **Cuando** el usuario ingresa texto en el campo de búsqueda ("Buscar por nombre o documento..."),
- **Entonces** la tabla debe actualizarse automáticamente en tiempo real mostrando únicamente los registros que coincidan parcial o totalmente con el nombre, apellido o número de documento ingresado.

### AAC 3: Registro de un Nuevo Estudiante
- **Dado** que el usuario requiere agregar un nuevo registro,
- **Cuando** hace clic en el botón "Registrar Estudiante",
- **Entonces** se debe abrir un formulario modal solicitando:
  - Nombres (Requerido)
  - Apellidos (Requerido)
  - Tipo de Documento (Seleccionable: DNI, Pasaporte, etc.)
  - Número de Documento (Requerido, solo numeros positivos)
  - Correo Electrónico (Requerido)
  - Teléfono
  - Fecha de Nacimiento (no se permiten fechas de nacimiento en el futuro)
  - Carrera (Requerido - Menú desplegable con las opciones existentes)
  - URL de Fotografía (Requerido)
  - Dirección Residencial
- **Y cuando** se guarda exitosamente, el modal debe cerrarse y el nuevo estudiante aparecerá en la tabla, mostrando una notificación de éxito.

### AAC 4: Edición de un Estudiante Existente
- **Dado** que el usuario necesita modificar la información de un estudiante,
- **Cuando** hace clic en el ícono de "Editar" en la fila correspondiente,
- **Entonces** se debe abrir el formulario modal con todos los campos pre-cargados con la información actual del estudiante.
- **Y cuando** el usuario modifica los datos y guarda, la información en la tabla se actualizará correspondientemente.

### AAC 5: Eliminación de un Estudiante
- **Dado** que el usuario desea remover un registro,
- **Cuando** hace clic en el ícono de "Eliminar" (papelera roja),
- **Entonces** el sistema debe solicitar una confirmación (alert).
- **Y cuando** el usuario confirma la acción, el sistema debe eliminar el registro, refrescar la tabla y mostrar una notificación de éxito. En caso de fallar, debe mostrar un error.

