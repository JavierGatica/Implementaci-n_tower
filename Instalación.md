# Temas.

* Introducción a la Ansible Tower
* Arquitectura de Ansible Towe
* Características de Ansible Tower
* Instalación de ansible tower
* Licencias y soporte
* Instaladores de Ansible Tower
* Ansible Tower Web Interface
* Configuración de plantilla de trabajo
* Lanzar un trabajo
* Puntos para recordar

Este módulo describe cómo se puede usar Red Hat Ansible Tower para ejecutar y administrar Ansible de manera eficiente a escala empresarial. Explica las posibles arquitecturas de Ansible Tower y cubre las muchas funciones para controlar, asegurar y administrar Ansible en un entorno empresarial. Se explican los requisitos de instalación y dos tipos de instaladores, así como las opciones de licencia y soporte.

### Introducción a la Torre Ansible

A medida que la experiencia de una empresa con Ansible madura, a menudo encuentra oportunidades adicionales para aprovechar Ansible para simplificar y mejorar las operaciones de TI. Los mismos Playbooks de Ansible utilizados por los equipos de operaciones para implementar sistemas de producción también se pueden usar para implementar sistemas idénticos al principio del ciclo de vida de desarrollo de software. Cuando se automatiza con Ansible, las tareas complejas de soporte de producción que normalmente manejan ingenieros calificados se pueden delegar a los técnicos de nivel de entrada.

* Ansible Playbooks se puede usar para implementar sistemas antes en el ciclo de vida del desarrollo de software
* Con la automatización Ansible, los técnicos de nivel de entrada pueden manejar tareas complejas de soporte de producción
* Compartir infraestructura Ansible para escalar la automatización de TI en toda la empresa presenta desafíos:
* Los libros de jugadas se pueden aprovechar en todos los equipos, pero Ansible no tiene instalaciones para administrar el acceso compartido
* La ejecución de libros de jugadas puede requerir credenciales de administrador altamente privilegiadas y protegidas
* Los equipos de TI que tradicionalmente usan herramientas GUI pueden encontrar intimidante la CLI de Ansible

### Marco de referencia
* Ansible Tower proporciona un marco para ejecutar y administrar Ansible de manera eficiente a escala empresarial
* Ofrece interfaz web, control de acceso basado en roles (RBAC) y registro y auditoría centralizados
* RESTful API facilita la integración con los flujos de trabajo y conjuntos de herramientas existentes de la empresa

### Arquitectura.
Ansible Tower es una aplicación web de Django diseñada para ejecutarse en un servidor Linux como una solución local y autohospedada que se superpone a la infraestructura Ansible existente de una empresa.

Ansible Tower se deriva del proyecto anterior AnsibleWorks AWX. Los rastros de este linaje son evidentes en las muchas referencias de AWX en los archivos y estructuras de directorios actuales de Ansible Tower.

### Interfaz web y API de Ansible Tower
* Los usuarios interactúan con la infraestructura de Ansible a través de la interfaz web de Ansible Tower o la API RESTful
* La interfaz web es un contenedor de interfaz gráfica que ejecuta llamadas contra API
* Las acciones disponibles a través de la interfaz web también están disponibles a través de API
* RESTful API esencial para integrar Ansible con herramientas y procesos de software existentes
* Ansible Tower almacena datos en la base de datos de back-end PostgreSQL y utiliza el sistema de mensajería RabbitMQ

Ansible Tower almacena sus datos en una base de datos de back-end PostgreSQL y hace uso del sistema de mensajería RabbitMQ. Las versiones de Ansible Tower anteriores a la versión 3.0 también se basaban en una base de datos MongoDB. Esta dependencia se eliminó en la versión 3.0, y los datos ahora se almacenan únicamente en una base de datos PostgreSQL.

### Posibles arquitecturas
Dependiendo de las necesidades de una empresa, Ansible Tower se puede implementar utilizando una de las siguientes arquitecturas:

* Máquina individual con base de datos integrada
* Máquina individual con base de datos remota
* Clúster multi-máquina de alta disponibilidad.

# Máquina individual con base de datos integrada
* Arquitectura estándar
* Todos los componentes de Ansible Tower, el front-end web, el back-end API RESTful y la base de datos PostgreSQL residen en una sola máquina

### Máquina individual con base de datos remota
* Interfaz web y API RESTful en una sola máquina
* Base de datos PostgreSQL instalada remotamente en otro servidor en la misma red
* La base de datos remota se puede alojar en el servidor con una instancia existente de PostgreSQL fuera de la administración de Ansible Tower
* Otra opción: hacer que el instalador cree una instancia de PostgreSQL administrada por Ansible Tower en un servidor remoto poblado con la base de datos de Ansible Tower

### Alta disponibilidad, grupo de máquinas múltiples
1.- Versiones anteriores:

* Arquitectura redundante activa-pasiva
* Solo nodo activo, uno o más nodos inactivos

2.- Versión 3.2:

Arquitectura activa-activa
* Clúster de alta disponibilidad con múltiples nodos de tower activos
* Cada nodo en el clúster aloja el front-end web Ansible Tower y el back-end API RESTful

Dos opciones para alojar la base de datos PostgreSQL en un servidor remoto:

* Base de datos remota en el servidor con una instancia existente de PostgreSQL fuera de la administración de Ansible Tower
* Base de datos remota en el servidor con instancia de PostgreSQL administrada por Ansible Tower creada por el instalador

A partir de Ansible Tower versión 3.3, la arquitectura redundante activa-pasiva más antigua, que consta de un solo nodo activo y uno o más nodos inactivos, ha sido reemplazada por un clúster de alta disponibilidad activo-activo que consta de múltiples nodos de torre activos.

### Tipos de licencia
* Enterprise : proporciona acceso a todas las funciones de Ansible Tower
* Básico : proporciona acceso a un subconjunto de funciones

No incluye características empresariales como el seguimiento del sistema, la agregación de registros y la agrupación

Ansible Tower ofrece muchas funciones para controlar, asegurar y administrar Ansible en un entorno empresarial.

### Tablero de instrumentos, RBAC, gestión de inventario

La interfaz web de Ansible Tower se abre en una pantalla de tablero visual que proporciona una vista resumida de todo el entorno Ansible de una empresa. El panel de control permite a los administradores ver fácilmente el estado actual de los hosts e inventarios, así como los resultados de las recientes ejecuciones de trabajo.

Ansible Tower utiliza control de acceso basado en roles para mantener la seguridad y agilizar la gestión de acceso de los usuarios. Simplifica la delegación del acceso del usuario a los objetos de Ansible Tower, como organizaciones, proyectos e inventarios.

Puede crear grupos de inventario y agregar hosts de inventario a través de la interfaz web gráfica. Los inventarios también se pueden actualizar desde una fuente de inventario externa, como un proveedor de nube pública, un entorno de virtualización local y una base de datos de administración de configuración personalizada (CMDB) de una organización.

Ansible Tower le permite programar la ejecución del libro de jugadas y las actualizaciones de fuentes de datos externas, ya sea de forma única o repetida a intervalos regulares. Esto permite que las tareas de rutina se realicen desatendidas y es especialmente útil para tareas como las rutinas de respaldo que se ejecutan idealmente durante las horas fuera de servicio operativas.

Se registran todas las ejecuciones de playbooks y comandos remotos iniciadas en Ansible Tower. Esto proporciona la capacidad de auditar cuándo se ejecutó cada trabajo y quién lo hizo. Además, Ansible Tower puede integrar sus datos de registro en soluciones de agregación de registros de terceros como Splunk y Sumologic.

Las notificaciones se pueden utilizar para indicar cuándo las ejecuciones de trabajos de Tower tienen éxito o fallan. Las notificaciones se pueden enviar utilizando muchos protocolos diferentes, incluidos correo electrónico, Slack e HipChat.

### Seguimiento del sistema, API RESTful y CLI

Ansible Tower se puede configurar para escanear rutinariamente hosts administrados y registrar sus estados. Los datos recopilados se pueden usar para auditar los cambios del sistema a lo largo del tiempo. Además, esta característica se puede usar para comparar sistemas y detectar diferencias.

La API RESTful de Ansible Tower expone todas las funciones disponibles a través de la interfaz web. El formato navegable de la API hace que se auto documente y simplifica la búsqueda de información de uso de la API. Ansible Tower también proporciona herramientas de CLI que le permiten interactuar fácilmente con la API de Ansible Tower.

# Instalación de ansible tower

### Sistema operativo
* Red Hat Enterprise Linux 7.4 o posterior en arquitectura de 64 bits
* También se puede instalar en versiones de 64 bits de CentOS 7.4 o posterior, Ubuntu 14.04 LTS y Ubuntu 16.04 LTS

### Navegador web
* Versión actual de Mozilla Firefox o Google Chrome para conectarse a la interfaz web de Ansible Tower
* Otros navegadores web compatibles con HTML5 pueden funcionar, pero no se prueban ni admiten por completo

### Memoria
* Mínimo 4 GB de RAM en el host Ansible Tower.
* El requisito de memoria real depende del número máximo de hosts que Ansible Tower debe configurar en paralelo
* Gestionado por el parámetro de configuración de horquillas en la plantilla de trabajo o la configuración del sistema
* Se recomiendan 4 GB de memoria por cada 100 forks

### Almacenamiento de disco
* Al menos 20 GB de almacenamiento.
* 10 GB deben estar disponibles para el /var directorio
* El requisito mínimo no incluye espacio de almacenamiento para los nodos que contienen una base de datos (se recomiendan más de 150 GB).

Los requisitos de almacenamiento de la base de datos aumentan con la cantidad de hosts administrados, la cantidad de trabajos ejecutados, la cantidad de hechos almacenados en la memoria caché de hechos y la cantidad de tareas en cualquier trabajo individual.

Referencia. https://docs.ansible.com/ansible-tower/latest/html/quickinstall/prepare.html

### Ansible
* Instalación realizada ejecutando un Playbook Ansible
* Las versiones anteriores de Ansible Tower requerían que se instalara la última versión de Ansible antes de la instalación de Ansible Tower
* El programa de instalación actual instala automáticamente Ansible y cualquier dependencia que no esté presente
* Los usuarios de Red Hat Enterprise Linux 7 deben habilitar el extras repositorio

Para que Ansible Tower funcione correctamente, debe instalarse la última versión estable de Ansible utilizando el administrador de paquetes de su distribución

*NOTA: Si Ansible ya está instalado en el sistema, el instalador de Ansible Tower no intenta reinstalarlo. Para que Ansible Tower funcione correctamente, la versión estable más reciente de Ansible debe instalarse utilizando el administrador de paquetes de su distribución.*

### SELinux
* Ansible Tower admite la política de SELinux dirigida
* Se puede configurar como obligatorio, permisivo o deshabilitado
* Otras políticas de SELinux no compatibles

###Licencias y soporte
* Licencia de prueba y suscripciones
* Licencia de prueba disponible para evaluación sin costo
* Ansible Tower Trial
* Tres tipos de suscripciones:
* Auto ayuda
* Estándar empresarial
* Enterprise Premium
https://www.redhat.com/en/technologies/management/ansible/try-it/success
https://docs.ansible.com/ansible-tower/latest/html/quickinstall/index.html

### Edición Enterprise Standard
La edición Enterprise Standard incluye una suscripción Enterprise con mantenimiento de software y actualizaciones con derecho a todas las funciones de Ansible Tower y soporte técnico 8x5. El precio se basa en el número de nodos gestionados. Se incluye el soporte de Ansible Playbook, que incluye ayuda con problemas de ejecución en tiempo de ejecución, asistencia con errores y trazas, y orientación limitada sobre prácticas recomendadas en la versión menor actual o anterior de Ansible.

### Edición Enterprise Premium
La edición Enterprise Premium también incluye una suscripción Enterprise con mantenimiento y actualizaciones de software y todas las funciones de Tower, pero con soporte técnico 24x7. El precio se basa en la cantidad de nodos administrados y se incluye el soporte Ansible Playbook.

La información detallada sobre los precios y el soporte de Ansible Tower está disponible en los enlaces que se muestran aquí.

# Instaladores de Ansible Tower

### Instalador estandar.
* Siempre contiene la última versión de Ansible Tower para Red Hat Enterprise Linux 7
* El archivo es más pequeño, pero requiere conectividad a Internet para descargar paquetes de Ansible Tower desde repositorios

http://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz 

### Instalador incluido para Red Hat Enterprise Linux 7
* El archivo incluye un conjunto inicial de paquetes RPM para que Ansible Tower pueda instalarse en sistemas desconectados de Internet
* Puede preferirse en entornos de alta seguridad.
* Los sistemas aún necesitan acceso a paquetes de software para el canal de Red Hat Enterprise Linux 7 y Red Hat Enterprise Linux 7 Extras
* No disponible para Ubuntu

http://releases.ansible.com/ansible-tower/setup-bundle/ansible-tower-setup-bundle-latest.el7.tar.gz

# Instalación Ansible Tower, pasos 1-2
1.- Como usuario root, descargue el paquete de instalación de Ansible Tower
2.- Extraiga el paquete de configuración y cámbielo al directorio que contiene los contenidos extraídos:

    [root@towerhost ~]# tar xzf ansible-tower-setup-3.3.1.tar.gz
    [root@towerhost ~]# cd ansible-tower-setup-3.3.1-1
3.- Edite el archivo de inventario para establecer contraseñas para la admincuenta Ansible Tower ( admin_password), la cuenta de usuario de la base de datos PostgreSQL ( pg_password) y la cuenta de usuario de mensajería RabbitMQ ( rabbitmq_password):

    [tower]
    localhost ansible_connection=local
    [database]

    [all:vars]
    admin_password='myadminpassword'

    pg_host='' pg_port=''

    pg_database='awx' pg_username='awx'
    pg_password='somedatabasepassword'

    rabbitmq_port=5672 rabbitmq_vhost=tower
    rabbitmq_username=tower
    rabbitmq_password='and-a-messaging-password'
    rabbitmq_cookie=cookiemonster

    # Needs to be true for fqdns and ip addresses rabbitmq_use_long_name=false

4.- Ejecute el instalador de Ansible Tower ejecutando setup.sh script:

      [root@tower ansible-tower-setup-3.2.3]# ./setup.sh
      [warn] Will install bundled Ansible
      Loaded plugins: langpacks, search-disabled-repos
      epel-release-latest-7.noarch.rpm                                                                                    |  15 kB  00:00:00
      Examining /var/tmp/yum-root-_gyWfD/epel-release-latest-7.noarch.rpm: epel-release-7-11.noarch
      Marking /var/tmp/yum-root-_gyWfD/epel-release-latest-7.noarch.rpm to be installed
      Resolving Dependencies
      ... Output omitted ...
      The setup process completed successfully.
      Setup log saved to /var/log/tower/setup-2018-11-08-21:02:49.log
      
5.- Después de que el instalador termine, conéctese al sistema Ansible Tower usando el navegador web

6.- Inicie sesión en la interfaz web de Ansible Tower como administrador utilizando la admin cuenta y la contraseña que configuró en el archivo de inventario del instalador

7.- Ingrese la licencia de Ansible Tower proporcionada por Red Hat, acepte el acuerdo de licencia de usuario final

### Navegación y uso
* Ansible Tower configurado con:
* Proyectos ansibles que contienen libros de jugadas
* Inventarios confiables y credenciales de máquina necesarias para iniciar sesión en hosts de inventario y escalar privilegios
* Plantilla de trabajo que especifica qué libro de jugadas desde qué proyecto se ejecuta en hosts en un inventario particular, utilizando credenciales de máquina particulares
* El trabajo se crea cuando el usuario inicia la plantilla de trabajo para ejecutar el libro de jugadas en un inventario


### Plantilla de trabajo
* La plantilla de trabajo se puede usar repetidamente para iniciar trabajos con los mismos parámetros
* La plantilla es como un ansible-playbookcomando enlatado , completo con opciones y argumentos, que está escrito o en el historial de shell
* Iniciar un trabajo con la plantilla de trabajo es como ejecutar un ansible-playbookcomando desde el indicador de comandos

### Puntos para recordar

* Ansible Tower 3.3.1: solución de gestión centralizada para proyectos Ansible
* Ansible Tower tiene dos opciones de instalación:
* El instalador estándar descarga paquetes de repositorios de red
* El instalador incluido incluye dependencias de software de terceros
* Plantillas de trabajo: comandos preparados para ejecutar Playbooks de Ansible
* Incluye inventario, credenciales de máquina, proyecto Ansible, Playbook Ansible
* El trabajo se inicia desde la plantilla de trabajo, representa un libro de jugadas único ejecutado en el inventario de máquinas
* Dashboard proporciona resúmenes del estado del host, inventarios, proyectos, trabajos ejecutados
* El menú de navegación proporciona acceso a los enlaces Vistas, Recursos, Acceso y Administración
* Los enlaces de la herramienta de administración proporcionan acceso a los detalles actuales del usuario, la información de Ansible Tower y la documentación en línea

