# Historia de Usuario: Gestión de Incorporación de Nuevos Empleados

- ID: US_002
- Título: Gestión de incorporación de nuevos empleados
- URL: https://testing1.geekqa.net/

Como Administrador de Recursos Humanos en GeekRetail,
quiero registrar la información personal y laboral de los nuevos empleados en el sistema,
para poder mantener una base de datos centralizada que me permita gestionar al personal y sus condiciones contractuales.

## Criterios de Aceptación (CA)

### AC 1: Registro exitoso y visualización en la tabla
Dado que el administrador completa todos los campos obligatorios con datos válidos:

- Nombre: más de 3 caracteres
- Edad: entre 18 y 65 años (Nota: hay una inconsistencia en el código entre la condición if y el mensaje de error)
- Salario: mayor que 0

Cuando hace clic en el botón "Registrar Empleado", Entonces el sistema debe mostrar el mensaje:"¡Empleado registrado con éxito!"
Y los datos del empleado deben aparecer inmediatamente como una nueva fila en la tabla de "Empleados registrados".
