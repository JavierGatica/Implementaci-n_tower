# Inventario y credenciales.

Este módulo se enfoca en crear y configurar un inventario estático con Ansible Tower. Primero revisa los inventarios de Ansible, luego describe los inventarios de la Torre de Ansible, que se gestionan como objetos.

### Inventario Ansible
* Los Playbooks de Ansible se ejecutan contra el inventario de hosts y grupos de hosts
* Puede seleccionar partes o todo el inventario como sistemas de destino para administrar mediante la ejecución de tareas
* Ansible normalmente define el inventario como archivo estático, o dinámicamente desde fuentes externas a través de script

### Ansible Inventory Review

    london1.example.com

    [southeast] 
    raleigh1.example.com 
    atlanta1.example.com

    [northeast]
    boston1.example.com

    [east:children]   
    southeast northeast
    
1.- london1.example.com no es miembro de ningún grupo.
2.- raleigh1.example.com y atlanta1.example.com son miembros del grupo de hosts southeast
3.- boston1.example.com es un miembro del grupo anfitrión northeast.
4.- El grupo host east es un grupo de grupos, incluidos los grupos host southeast y northeast.

### También existen dos grupos implícitos.
* El grupo all incluye todos los hosts en el inventario. 
* El grupo ungrouped incluye todos los hosts que no están en un grupo que no sea all.

### Inventarios de torre de Ansible
* Los inventarios de Ansible Tower se gestionan como objetos
* Cada organización puede tener muchos inventarios
* Las plantillas de trabajo se pueden configurar para usar inventario específico perteneciente a una organización específica
* Los usuarios que pueden usar el objeto de inventario están determinados por los roles que tienen en el inventario
* El inventario se puede configurar de varias maneras diferentes

### Roles de inventario

* Se pueden asignar roles de RBAC a usuarios y equipos para ver y administrar equipos y organizaciones
* Los roles también se pueden asignar a usuarios y equipos
* Les permite ver y administrar otros objetos.
* Los usuarios y los equipos pueden obtener permiso para leer, usar o administrar un inventario mediante la asignación de roles apropiados para ese inventario
* Los roles también pueden ser heredados

# Definiciones de roles

### Admin
Otorga a los usuarios permisos completos sobre un inventario, incluida la eliminación y modificación del inventario. También otorga permisos asociados con los roles de inventario Uso, Adhoc y Actualización.

### Use
Otorga a los usuarios la capacidad de utilizar el inventario en el recurso de plantilla de trabajo, que controla el inventario utilizado para iniciar trabajos utilizando el libro de jugadas de la plantilla de trabajo.

### Adhoc
Otorga a los usuarios la capacidad de usar el inventario para ejecutar comandos ad hoc

### Update
Otorga a los usuarios la capacidad de actualizar el inventario dinámico desde su fuente de datos externa.

### Read
Otorga a los usuarios la capacidad de ver el contenido de un inventario.

### Configurar acceso
* Inicialmente, el inventario es accesible solo para usuarios con roles de administrador o auditor para la organización a la que pertenece el inventario
* Todos los demás accesos deben estar configurados
* Hecho mediante la asignación de roles apropiados a usuarios y equipos
* El inventario debe crearse y guardarse antes de que a los usuarios y equipos se les puedan asignar roles.
* Los roles se asignan a través de la sección Permisos de la pantalla del editor
* Accesible a través de pencil_ico(lápiz) al lado de su nombre

### Objetos de credenciales
* Los objetos de Ansible Tower se utilizan para autenticarse en sistemas remotos para diversos fines
* Puede proporcionar secretos como contraseñas, claves SSH u otra información necesaria para acceder o usar recursos remotos
* Ansible Tower mantiene un almacenamiento seguro de secretos en objetos de credenciales
* Contraseñas y claves de credenciales cifradas antes de guardarse en la base de datos
* No se puede recuperar en texto claro desde la interfaz de usuario de Ansible Tower
* A los usuarios y equipos se les pueden asignar privilegios para usar credenciales, pero los secretos no están expuestos a ellos.
* Cuando el usuario cambia de equipo o abandona la organización, no es necesario volver a utilizar las credenciales y los sistemas.
* Cuando Ansible Tower necesita credenciales, descifra los datos internamente y los pasa directamente a SSH u otro programa
* Una vez que los datos confidenciales de autenticación se ingresan en la credencial y se cifran, no se pueden recuperar en forma descifrada a través de Ansible Tower

### Tipos de credenciales
* Las credenciales de la máquina se utilizan para autenticar los inicios de sesión del libro de jugadas y la escalada de privilegios para los hosts de inventario
* Las credenciales de red se usan cuando los módulos de red Ansible se usan para administrar equipos de red
* Los proyectos utilizan las credenciales de control de origen (SCM) para clonar y actualizar materiales de proyecto Ansible desde un sistema de control de versiones remoto como Git, Subversion o Mercurial
* Las credenciales de inventario se utilizan para actualizar la información de inventario dinámico de una de las fuentes de inventario dinámico incorporadas de Ansible Tower
* Existen diferentes tipos de credenciales para cada fuente de inventario: Amazon Web Services, VMware vCenter, Red Hat Satellite 6, Red Hat Cloud Forms, Google Compute Engine, Microsoft Azure Resource Manager, OpenStack ® , etc.


### Creación de credenciales de máquina: credenciales privadas y credenciales de organización
* Las principales distinciones entre credenciales privadas y las asignadas a la organización son:
* Cualquier usuario puede crear una credencial privada, pero solo los administradores del sistema y los usuarios con rol de administrador de la organización pueden crear la credencial de la organización.
* Si la credencial pertenece a la organización, a los usuarios y equipos se les pueden otorgar roles, y se puede compartir
* Las credenciales privadas no asignadas a la organización solo pueden ser utilizadas por el propietario y los roles únicos de Ansible Tower
* A otros usuarios y equipos no se les puede otorgar roles en él.
* Ansible Tower Admin puede asignar organización a credenciales privadas
* Se convierte en credencial de organización


### Roles de credenciales
* Las credenciales privadas son accesibles para sus creadores, administradores y auditores.
* A otros usuarios no se les pueden asignar roles en credenciales privadas
* Para asignar roles a las credenciales, la credencial debe asignarse a la organización
* Luego, los usuarios y los equipos de esa organización pueden compartir esa credencial a través de asignaciones de roles

### Admin   
Otorga a los usuarios permisos completos sobre credenciales, incluida la eliminación y modificación de credenciales, así como la capacidad de usar credenciales en la plantilla de trabajo

### Use
Otorga a los usuarios la capacidad de usar credenciales en una plantilla de trabajo

#### Read
Otorga a los usuarios la capacidad de ver detalles de credenciales; no permite al usuario descifrar secretos que pertenecen a credenciales a través de la interfaz web

### Administrar el acceso de credenciales
* Cuando la credencial de la organización se crea por primera vez, solo el propietario y los usuarios con el rol de administrador o auditor pueden acceder a ella en la organización en la que se creó la credencial
* El acceso adicional debe configurarse específicamente
* No se pueden asignar roles adicionales a la credencial mientras se está creando
* La credencial debe guardarse primero y luego editarse para agregar acceso
* Los roles se asignan a través de la sección PERMISOS de la pantalla del editor de credenciales

### Puntos para recordar
* Los recursos de inventario se utilizan para administrar inventarios Ansible de hosts y grupos de hosts y sus variables de inventario.
* Se pueden configurar múltiples inventarios y se pueden usar roles para administrar quién puede usar y administrar inventarios particulares
* Los inventarios estáticos se pueden configurar manualmente a través de la interfaz web
* Las credenciales se utilizan para almacenar información de autenticación para máquinas, dispositivos de red, control de origen y actualizaciones dinámicas de inventario.
* Las credenciales de la máquina se utilizan para permitir que Ansible Tower autentique el acceso y la escalada de privilegios en los hosts de inventario para la ejecución del libro de jugadas
* Las credenciales asignadas a una organización se pueden compartir otorgando roles a usuarios y equipos
* La credencial no asignada a una organización es privada para el usuario que la creó y las funciones de Singleton de Ansible Tower, y no se puede compartir sin asignarla a una organización

