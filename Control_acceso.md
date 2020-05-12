# Control de acceso basado en roles

Los usuarios de Ansible Tower necesitan diferentes niveles de permisos para diferentes tareas:
* Ejecutar plantillas de trabajo existentes contra inventario preconfigurado de máquinas
* Modificar inventarios particulares, plantillas de trabajo y libros de jugadas
* Cambiar cualquier cosa en la instalación de Ansible Tower

El usuario administrador incorporado tiene acceso de superusuario a toda la configuración
* Puede crear cuentas de usuario para administrar el acceso a inventarios, credenciales, proyectos y plantillas de trabajo

Los permisos de usuario se administran con control de acceso basado en roles (RBAC)
* Los roles de los usuarios definen lo que pueden ver, cambiar o eliminar.
* Los roles se pueden administrar colectivamente asignándolos al equipo
* Los usuarios asignados al equipo heredan los roles del equipo

### Organizaciones lógicas
* Colección lógica de equipos, proyectos e inventarios.
* Todos los usuarios deben estar asignados a una organización.
* Beneficio: comparta roles y libros de jugadas a través de límites departamentales o funcionales dentro de la empresa
* Ejemplo: el grupo de operaciones ya puede tener roles Ansible que el grupo de desarrolladores podría usar, como roles para aprovisionar servidores
* Para implementaciones muy grandes, puede ser útil clasificar a los usuarios, equipos, proyectos e inventarios bajo una organización paraguas:
* Evitar que ciertos departamentos se implementen en ciertos inventarios de hosts o que ejecuten ciertos libros de jugadas
* Configure una colección de usuarios y equipos para trabajar solo con los recursos que se espera que usen
* Ansible Tower se instala con la organización predeterminada

# Tipos de usuario

### Administrador de sistema
* Acceso a todas las acciones en toda la instalación de Ansible Tower
* Especial Singleton papel con permiso de lectura y escritura en todos los objetos en todas las organizaciones
* El administrador creado por el instalador tiene la función de administrador del sistema

### Auditor del sistema
* Acceso de solo lectura a toda la instalación de Ansible Tower
* Considerado papel especial de singleton

### Usuario normal
* Acceso extremadamente limitado
* No hay roles especiales.
* Roles asignados asociados con la organización a la que están asignados

Ansible Tower tiene tres tipos de usuarios.

El tipo de usuario Administrador del sistema, también conocido como superusuario, tiene permiso ilimitado para realizar cualquier acción dentro de toda la instalación de Ansible Tower. El administrador del sistema es una función de singleton especial que tiene permiso de lectura y escritura en todos los objetos en todas las organizaciones.

El usuario administrador creado por el instalador tiene el rol de administrador único del sistema y, por lo tanto, solo debe ser utilizado por el administrador de Ansible Tower.

El tipo de usuario del Auditor del sistema tiene acceso de solo lectura a toda la instalación. Esto también se considera un rol especial de singleton.

Usuario normal es el tipo de usuario estándar sin roles especiales asignados inicialmente. Comienza con un acceso extremadamente limitado. A los usuarios normales se les asignan roles asociados con la organización a la que pertenecen.
