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

### Ansible Tower tiene tres tipos de usuarios.

El tipo de usuario Administrador del sistema, también conocido como superusuario, tiene permiso ilimitado para realizar cualquier acción dentro de toda la instalación de Ansible Tower. El administrador del sistema es una función de singleton especial que tiene permiso de lectura y escritura en todos los objetos en todas las organizaciones.

El usuario administrador creado por el instalador tiene el rol de administrador único del sistema y, por lo tanto, solo debe ser utilizado por el administrador de Ansible Tower.

El tipo de usuario del Auditor del sistema tiene acceso de solo lectura a toda la instalación. Esto también se considera un rol especial de singleton.

Usuario normal es el tipo de usuario estándar sin roles especiales asignados inicialmente. Comienza con un acceso extremadamente limitado. A los usuarios normales se les asignan roles asociados con la organización a la que pertenecen.

# Roles
* Roles de organización
* Los nuevos usuarios heredan roles de organización específicos del tipo de usuario
* Asigne roles adicionales para otorgar permisos para ver, usar o cambiar objetos adicionales de Ansible Tower
* La organización misma es uno de estos objetos.
* A los usuarios se les puede asignar uno de los cuatro roles en la organización:
  * Admin
  * Auditor
  * Member
  * Read
  
### Administración
* Acceso sin restricciones para gestionar todos los aspectos de la organización:
* Leer y cambiar la organización
* Agregar y eliminar usuarios y equipos

### Auditor
* Acceso de solo lectura a todos los aspectos de la organización.

### Miembro
* Permiso de lectura dentro de la organización
* Puede ver la lista de usuarios de la organización y sus roles.
* No se puede acceder a ningún recurso de la organización, como equipos, credenciales, proyectos, inventarios, plantillas de trabajo, plantillas de trabajo y notificaciones.

### Leer
* Igual que miembro
  
### Herencia del rol de la organización
* El rol de administrador único del sistema hereda el rol de administrador en cada organización dentro de Ansible Tower
* El rol singleton de System Auditor hereda el rol de Auditor en cada organización
* Usuario normal asignado El rol de miembro en el usuario de la organización fue asignado durante la creación del usuario
* Se pueden agregar otros roles más adelante, incluidos los roles de miembros adicionales en otras organizaciones

### Administrar usuarios de manera eficiente con equipos
* Los equipos son grupos de usuarios que hacen que la gestión de roles sea más eficiente:
* En lugar de asignar los mismos roles a múltiples usuarios, puede asignar roles al equipo
* Los usuarios asignados al equipo heredan todos los roles asignados al equipo
* Los usuarios existen como objetos en el nivel de Ansible Tower
* El usuario puede tener roles en múltiples organizaciones
* El equipo solo puede pertenecer a una organización
* El administrador del sistema puede asignar roles de equipo a recursos que pertenecen a otras organizaciones

# Roles de equipo

### Administración
* Otorga al usuario el control total del equipo
* Puede administrar los usuarios del equipo y sus roles de equipo
* También puede administrar roles de equipo en recursos para los cuales se asigna un Admin rol de equipo

### Miembro
* Puede ver los usuarios del equipo y las funciones asociadas del equipo.
* Hereda roles en recursos otorgados al equipo

### Leer
* Puede ver los usuarios del equipo y las funciones asociadas del equipo.
* No hereda roles en los recursos otorgados al equipo

# Puntos para recordar
* Las organizaciones son colecciones lógicas de usuarios, equipos, proyectos e inventarios.
* Los usuarios pueden ser uno de tres tipos: administrador del sistema, auditor del sistema y usuario normal
* El administrador del sistema y el auditor del sistema son roles únicos que otorgan acceso de lectura-escritura y solo lectura, respectivamente, a todos los objetos de Ansible Tower
* A los usuarios se les puede asignar uno de los cuatro roles de la organización: administrador, auditor, miembro y lectura
* Un equipo es un grupo de usuarios que simplifica la asignación de roles a los usuarios en los recursos de Ansible Tower
* A los usuarios se les puede asignar uno de los tres roles de equipo: administrador, miembro y lectura
* El rol que un usuario tiene en un objeto determina lo que el usuario puede hacer a ese objeto
