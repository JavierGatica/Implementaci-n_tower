# Proyectos para aprovisionamiento con Ansible Tower

### Infraestructura como Código
* Concepto clave de DevOps
* Reemplaza la infraestructura de administración mediante la ejecución manual de comandos
* Componentes construidos y mantenidos a través de procedimientos automatizados y programáticos.
* Los proyectos y libros de jugadas de Ansible admiten la implementación de infraestructura como código
* Los proyectos de Ansible son códigos utilizados para administrar la infraestructura.
* Sistema de control de versiones (por ejemplo, Git) requerido para rastrear y controlar cambios
* Use la herramienta de control de versiones para implementar el ciclo de vida de las etapas del código de infraestructura: desarrollo, control de calidad, producción
* Le permite probar cambios de código en entornos de desarrollo y control de calidad
* Evita interrupciones al implementar implementaciones en entornos de producción
* Ansible Tower: ubicación central para ejecutar playbooks de Ansible
* Tower extrae proyectos que contienen libros de jugadas del repositorio de Git
* Los proyectos de torres particulares pueden extraer materiales de ramas específicas del repositorio

La infraestructura como código es un concepto clave en DevOps. En lugar de administrar la infraestructura mediante la ejecución manual de comandos, los componentes esenciales se crean y mantienen a través de procedimientos automatizados y programáticos. Los proyectos de Ansible y sus libros de jugadas son herramientas clave que lo ayudan a implementar la infraestructura como código.

Ansible Tower proporciona una ubicación central desde la cual ejecutar los playbooks de Ansible. Tower puede extraer proyectos que contengan esos libros de jugadas de un repositorio de Git. Incluso se puede configurar para que proyectos particulares de Tower extraigan materiales de ramas específicas de un repositorio Git.

### Git
* Sistema de control de versiones distribuido (DVCS)
* Permite a los desarrolladores gestionar en colaboración los cambios en el archivo del proyecto
* Cada revisión de archivo comprometida con el sistema
* Beneficios de DVCS:
* Revisar / restaurar versiones antiguas de archivos
* Compare versiones de archivos para identificar cambios
* Ver el registro para ver quién realizó los cambios y cuándo
* Modifique en colaboración archivos, resuelva conflictos y combine cambios entre múltiples usuarios
* En un sistema distribuido, comience clonando un proyecto compartido existente desde un repositorio remoto
* Copia completa del repositorio remoto se convierte en repositorio local
* Incluye archivos de sistema de control de versión de historial completo

Las cuatro áreas donde Git administra archivos se muestran aquí: 
1.- repositorio remoto 
2.- repositorio local
3.- área de preparación
4.- árbol de trabajo.


### Descripción general del ciclo de vida del archivo
* Árbol de trabajo: pago de una instantánea única del proyecto
* Ediciones al archivo hechas aquí
* Luego, conjunto de cambios relacionados por etapas y comprometidos con el repositorio local
* No hay cambios en el repositorio remoto compartido
* Para compartir trabajo, empuje los cambios al repositorio remoto
* Alternativa: el propietario del repositorio remoto puede extraer los cambios del repositorio local
* El repositorio local debe ser accesible desde la red
* Para actualizar el repositorio local con cambios en el repositorio remoto:
* Obtener cambios desde el repositorio remoto
* Fusionarse con el trabajo local

Realiza ediciones en su árbol de trabajo, que es una comprobación de una única instantánea del proyecto. Un conjunto de cambios relacionados se organizan y se comprometen con el repositorio local. En este punto, no se han realizado cambios en el repositorio remoto compartido.

### Etapas de archivo del árbol de trabajo

### Modified
* Se ha editado la copia del archivo en el árbol de trabajo.
* La copia es diferente de la última versión en el repositorio

### Staged
* Archivo modificado agregado a la lista de archivos modificados para confirmar como conjunto
* Conjunto aún no comprometido

### Committed
* Archivo modificado comprometido con el repositorio local

### ndicadores de aviso de shell
* Si el directorio actual está en el árbol de trabajo de Git, el nombre actual de la rama de Git para el árbol de trabajo se muestra entre paréntesis
* El mensaje indica que los archivos no rastreados, modificados o en etapas no se han confirmado en el árbol de trabajo:
* (branch *): El archivo es rastreado, modificado
* (branch +): El archivo es rastreado, modificado, organizado con git add
* (branch %): Los archivos no se rastrean
* Combinaciones posibles, por ejemplo, (branch *+)

## Agregar / actualizar contenido con subcomandos

### git status
* Muestra información sobre qué archivos en el árbol de trabajo son:
* Modificado pero sin etapas
* Sin seguimiento o nuevo
* Organizado para la próxima confirmación

### git add
* Prepara y prepara los archivos para ser comprometidos
* El archivo debe estar organizado en el área de ensayo para guardarse en el repositorio en la próxima confirmación
* Si realiza dos cambios a la vez:
* Organice los archivos en dos confirmaciones para rastrear fácilmente los cambios.
* Cada conjunto de cambios por etapas y comprometidos por separado

### git rm
* Elimina el archivo del directorio de trabajo
* Eliminación de etapas del repositorio en la próxima confirmación

### git reset
* Elimina el archivo agregado para la próxima confirmación del área de preparación
* No afecta el contenido del archivo en el árbol de trabajo

### git commit
* Confirma archivos almacenados en el repositorio local de Git
* Requiere un mensaje de registro que explique el motivo por el que se está guardando el conjunto de archivos almacenados
* Confirmar abortado si no hay mensaje
* Haga que el mensaje sea lo suficientemente detallado como para ser útil
* No confirma automáticamente los archivos modificados en el árbol de trabajo
* Para organizar y confirmar archivos modificados en un solo paso, use git commit -a
* No incluye ningún archivo creado sin seguimiento
* Use git add agregar archivos y rastrearlos para futuras confirmaciones

## Referencias rapidas de Git

### git clone URL
Clona el proyecto Git existente desde el repositorio remoto en la URL en el directorio actual

### git status
 Muestra el estado de los archivos modificados y almacenados en el árbol de trabajo.

### git add file
Archivo de etapas para la próxima confirmación

### git rm file
Eliminación de etapas del archivo para la próxima confirmación

### git reset
Desinstala archivos para el próximo commit
Opuesto de git add

### git commit
Confirma los archivos almacenados en el repositorio local con un mensaje descriptivo

### git push
Empuja los cambios en el repositorio local al repositorio remoto

### git pull
Obtiene actualizaciones del repositorio remoto al repositorio local
Fusiona actualizaciones en el árbol de trabajo

## Estructura de proyecto Ansible en Git
* Variables y credenciales
* No use host_vars y group_vars directorios en el proyecto
* En cambio, almacene variables con Inventoryobjeto en Ansible Tower
* En los libros de jugadas, no lo use vars_promptpara establecer variables
* No funciona

* En cambio, use la funcionalidad de encuesta de Ansible Tower
* En Ansible Tower, los usuarios obtuvieron acceso a proyectos en sistemas de control de versiones a través de credenciales
* Proporcionar acceso a las fuentes de Git SCM a nivel de repositorio
* Los usuarios de Ansible Tower tienen acceso todo o nada a los repositorios de Git
* Las credenciales para acceder al repositorio de Git pueden acceder a todos los contenidos del repositorio
* Asegúrese de mantener solo los libros de jugadas y los roles para compartir con la audiencia prevista en el mismo repositorio de Git

### Estructura de proyecto Ansible en Git
* Almacene libros de jugadas, roles y otros detalles de proyectos de Ansible Tower en el sistema de control de versiones como Git
* Simplifica la colaboración en el desarrollo del libro de jugadas y el trabajo compartido
* El sistema proporciona un registro de los cambios para su uso como seguimiento de auditoría
* Dele a cada proyecto su propio repositorio Git
* Es mejor no asumir que los proyectos pueden importar roles o contenido de otros proyectos
* Para importar roles utilizados por múltiples proyectos, haga que cada proyecto use submódulos Git
* Limita el tamaño de descarga cuando se actualiza el libro de jugadas
* Ayuda a los contribuyentes a evitar códigos confusos para diferentes proyectos

### Plantillas de trabajo

### admin
* Permite al usuario eliminar la plantilla de trabajo o editar propiedades, incluidos los permisos asociados
* Otorga permisos asociados executey readroles
* No permite al usuario copiar la plantilla de trabajo
* El usuario debe tener un userol en el proyecto para copiar la plantilla de trabajo asociada

### execute
* Permite a los usuarios ejecutar y programar trabajos utilizando la plantilla de trabajo
* Otorga permisos asociados con el readrol
* El usuario con executerol en la plantilla de trabajo no necesita useroles en ninguno de los recursos asociados de Ansible Tower

### read
* Otorga a los usuarios acceso de solo lectura para ver las propiedades de la plantilla de trabajo
* Otorga acceso para ver otra información relacionada con la plantilla de trabajo
* Trabajos ejecutados usando plantilla de trabajo
* Permisos y notificaciones asociados

## Puntos para recordar
* Git es un sistema de control de versiones distribuido
* Los usuarios clonan el proyecto Git desde el repositorio remoto al repositorio local, realizan cambios en el repositorio local y, cuando esté listo, envían los cambios al repositorio remoto para compartir con otros usuarios
* El recurso del proyecto se usa para representar el proyecto Ansible
* El proyecto se puede completar de diferentes maneras:
* El método común es recuperar el proyecto de la fuente SCM
* Este método requiere una credencial SCM para autenticarse en fuentes SCM
* Las plantillas de trabajo definen parámetros para la ejecución de trabajos Ansible
* La plantilla de trabajo debe especificar el proyecto del cual obtener el libro de jugadas para su ejecución
* La plantilla de trabajo también usa el inventario para definir la lista de hosts administrados en los que ejecutar el libro de jugadas
* La credencial de la máquina se usa para autenticar contra hosts administrados

