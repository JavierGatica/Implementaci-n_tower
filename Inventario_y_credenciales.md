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

