# Historia de Usuario: Gestión de Incorporación de Nuevos Empleados

- ID: US_002
- Título: Gestión de incorporación de nuevos empleados
- URL: https://testing1.geekqa.net/

Como Administrador de Recursos Humanos en GeekRetail,
quiero registrar la información personal y laboral de los nuevos empleados en el sistema,
para poder mantener una base de datos centralizada que me permita gestionar al personal y sus condiciones contractuales.

## Criterios de Aceptación (CA)

### Escenario 1: Registro exitoso y visualización en la tabla
Dado que el administrador completa todos los campos obligatorios con datos válidos:

- Nombre: más de 3 caracteres
- Edad: entre 18 y 65 años (Nota: hay una inconsistencia en el código entre la condición if y el mensaje de error)
- Salario: mayor que 0

Cuando hace clic en el botón "Registrar Empleado",

Entonces el sistema debe mostrar el mensaje:

> "¡Empleado registrado con éxito!"

Y los datos del empleado deben aparecer inmediatamente como una nueva fila en la tabla de "Empleados registrados".

### Escenario 2: Validación de reglas de negocio (Campos críticos)
Dado que el administrador intenta registrar a un empleado,

Cuando ingresa datos que no cumplen las siguientes reglas:

- Nombre: menos de 3 caracteres
- Edad: fuera del rango definido
- Email: formato inválido (falta @ o dominio)
- Teléfono: valor no numérico

Entonces el sistema debe mostrar los mensajes de error correspondientes en rojo debajo de cada campo, y el registro no debe agregarse a la tabla.

### Escenario 3: Selección de departamento y estado del contrato
Dado que el administrador está completando el formulario,

Cuando selecciona un departamento del menú desplegable (Ventas, Administración, Logística),

Y marca o desmarca la casilla "Estado del contrato",

Entonces la tabla de resultados debe mostrar el nombre del departamento y el estado como "Activo" (si está marcado) o "Inactivo" (si está desmarcado).