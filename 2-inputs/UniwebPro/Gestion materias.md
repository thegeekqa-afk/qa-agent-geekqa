# Historia de Usuario: Gestión de Materias

- **URL**: https://uniwebpro.geekqa.net/
- **Rol**: Administrador Académico
- **Módulo**: Materias

## Descripción
Como Administrador Académico quiero poder crear, visualizar, editar y eliminar materias en el sistema para mantener actualizado el catálogo de asignaturas disponibles para las diferentes carreras.

## Criterios de Aceptación (AC)

### AC1: Visualizar el Catálogo de Materias
- Dado que el administrador accede al módulo de "Materias" en la navegación principal.
- Entonces debe visualizar una tabla que resuma la información clave de todas las asignaturas existentes.
- Y la tabla debe incluir las columnas: Nombre (con el semestre), Código, Créditos, Carrera (con etiquetas visuales de colores) y Acciones.

### AC2: Crear una Nueva Materia
- Dado que el administrador hace clic en el botón "+ Nueva Materia".
- Entonces se debe mostrar un modal con un formulario completo.
- Y el formulario debe solicitar: Nombre, Código, Créditos, Semestre, Carrera (menú desplegable) y Descripción.
- Cuando el administrador completa los datos requeridos y presiona "Registrar".
- Entonces la nueva materia debe guardarse y aparecer reflejada inmediatamente en la lista general.

### AC3: Editar una Materia Existente
- Dado que el administrador visualiza la lista de materias.
- Cuando hace clic en el icono de edición (lápiz azul) de una materia específica.
- Entonces se debe habilitar la edición de los campos (Nombre, Código, Créditos, Semestre, Carrera, Descripción).
- Y al guardar los cambios, la información actualizada debe reflejarse en la tabla de la interfaz.

### AC4: Eliminar una Materia
- Dado que el administrador necesita remover una materia del catálogo.
- Cuando hace clic en el icono de eliminación (papelera roja) en la fila de dicha materia.
- Entonces el sistema debe eliminar la asignatura de manera exitosa y removerla de la lista activa de forma visual.

### AC5: Búsqueda y Filtrado
- Dado que el administrador busca una materia en particular o quiere agilizar la exploración.
- Cuando ingresa texto en la barra de búsqueda superior.
- Entonces la vista de materias debe filtrar y listar únicamente los registros que coincidan con los términos buscados.
