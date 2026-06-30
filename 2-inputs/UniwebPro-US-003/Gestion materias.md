# Historia de Usuario: Gestión de Materias

* **URL**: https://uniwebpro.geekqa.net/
* **Rol**: Administrador Académico
* **Módulo**: Materias

## Descripción

Como Administrador Académico quiero poder crear, visualizar y eliminar materias en el sistema para mantener actualizado el catálogo de asignaturas disponibles para las diferentes carreras.

## Criterios de Aceptación (AC)

### AC1: Crear una Nueva Materia

* Dado que el administrador hace clic en el botón "+ Nueva Materia".
* Entonces se debe mostrar un modal con un formulario completo.
* Y el formulario debe solicitar: Nombre, Código, Créditos, ID profesor, Semestre, Carrera (menú desplegable), y Descripción.
* Cuando el administrador completa los datos requeridos y presiona "Registrar".
* Entonces la nueva materia debe guardarse y aparecer reflejada inmediatamente en la lista general.

### AC2: Eliminar una Materia

* Dado que el administrador necesita remover una materia del catálogo.
* Cuando hace clic en el icono de eliminación (papelera roja) en la fila de dicha materia.
* Entonces el sistema debe eliminar la asignatura de manera exitosa y removerla de la lista activa de forma visual.



