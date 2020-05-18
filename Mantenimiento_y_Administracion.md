# Solución de problemas básicos

### ansible-tower-service Guión
* Inicia, detiene, proporciona el estado y reinicia la infraestructura completa de Ansible Tower


          Reside en /usr/bin
          [root@tower_host ~]# ansible-tower-service
          Usage: /usr/bin/ansible-tower-service {start|stop|restart|status}
          
Ansible Tower viene con el ansible-tower-service script, que puede iniciar, detener, proporcionar el estado y reiniciar la infraestructura completa de Ansible Tower, incluidos los componentes de la base de datos y la cola de mensajes. El script de la utilidad de administración reside en el /usr/bindirectorio. En el ejemplo que se muestra aquí, la emisión del comando sin ninguna opción proporciona una lista de las opciones disponibles.

### Componentes de la torre de Ansible
* Datos almacenados en la base de datos PostgreSQL
* Utiliza el sistema de mensajería RabbitMQ
* Infraestructura ejecuta nginx servidor web, memcached daemon
* Componentes adicionales controlados por supervisord service
* Sistema de control de procesos utilizado a menudo para controlar aplicaciones basadas en Django
* Se usa para administrar y monitorear aplicaciones o demonios de larga ejecución y reiniciarlos automáticamente

El supervisord service controla los componentes adicionales , que es un sistema de control de procesos que a menudo se usa para controlar aplicaciones basadas en Django. supervisord usa para administrar y monitorear aplicaciones o daemons de larga ejecución, y puede reiniciar automáticamente esas aplicaciones o daemons controlados.

### Ejemplo de componentes de la torre de Ansible
* supervisorctl comando con status opción:

          [root@tower_host ~]# supervisorctl status
          exit-event-listener                     RUNNING   pid 13354, uptime 0:01:58
          tower-processes:awx-callback-receiver   RUNNING   pid 13358, uptime 0:01:58
          tower-processes:awx-celeryd             RUNNING   pid 13360, uptime 0:01:58
          tower-processes:awx-celeryd-beat        RUNNING   pid 13359, uptime 0:01:58
          tower-processes:awx-channels-worker     RUNNING   pid 13355, uptime 0:01:58
          tower-processes:awx-daphne              RUNNING   pid 13357, uptime 0:01:58
          tower-processes:awx-uwsgi               RUNNING   pid 13356, uptime 0:01:58
          

Puede usar el supervisorctl comando con la statusopción para ver la lista de procesos de Ansible Tower controlados por el supervisord service, como se muestra aquí.

Como se muestra en la salida, supervisord controla varios procesos propiedad del awx usuario, incluido el celeryd daemon, que se utiliza como una tarea de paso de mensajes distribuidos en tiempo real y una cola de trabajos.

### Ansible Tower Services
Enumere los servicios y los puertos abiertos mediante un **netstat -plut**:


                    [root@tower_host ~]# netstat -plut
                    Active Internet connections (only servers)
                    Proto Recv-Q Send-Q 	Local Address 		Foreign Address 	State
                    PID/Program name
                    tcp 	0 	0 	0.0.0.0:memcache 	0.0.0.0:* 		LISTEN
                    904/memcached
                    tcp 	0 	0 	0.0.0.0:sunrpc 		0.0.0.0:* 		LISTEN
                    1/systemd
                    tcp 	0 	0 	0.0.0.0:http 		0.0.0.0:* 		LISTEN
                    15314/nginx: master
                    tcp 	0 	0 	0.0.0.0:epmd 		0.0.0.0:* 		LISTEN
                    1272/epmd
                    tcp 	0 	0 	0.0.0.0:8050 		0.0.0.0:* 		LISTEN
                    15384/uwsgi
                    tcp 	0 	0 	localhost:rocrail 	0.0.0.0:* 		LISTEN
                    15386/python
                    tcp 	0 	0 	0.0.0.0:ssh 		0.0.0.0:* 		LISTEN
                    930/sshd
                    tcp 	0 	0 	0.0.0.0:15672 		0.0.0.0:* 		LISTEN
                    14847/beam.smp
                    tcp 	0 	0 	0.0.0.0:postgres 	0.0.0.0:* 		LISTEN
                    14323/postgres
                    tcp 	0 	0 	localhost:smtp 		0.0.0.0:* 		LISTEN
                    1206/master
                    tcp 	0 	0 	0.0.0.0:https 		0.0.0.0:* 	        LISTEN
                    15314/nginx: master
                    tcp 	0 	0 	0.0.0.0:25672 		0.0.0.0:* 		LISTEN
                    14847/beam.smp
                    tcp6 	0 	0 	[::]:memcache 		[::]:* 			LISTEN
                    904/memcached
                    tcp6 	0 	0 	[::]:sunrpc 		[::]:* 			LISTEN
                    1/systemd
                    tcp6 	0 	0 	[::]:http 		[::]:* 			LISTEN
                    15314/nginx: master
                    tcp6 	0 	0 	[::]:epmd 		[::]:* 			LISTEN
                    1272/epmd
                    tcp6 	0 	0 	[::]:ssh 		[::]:* 			LISTEN
                    930/sshd
                    tcp6 	0 	0 	[::]:postgres 		[::]:* 			LISTEN
                    14323/postgres
                    tcp6 	0 	0 	localhost:smtp 		[::]:* 			LISTEN
                    1206/master
                    tcp6 	0 	0 	[::]:amqp 		[::]:* 			LISTEN
                    14847/beam.smp
                    udp 	0 	0 	localhost:323 		0.0.0.0:*
                    517/chronyd
                    udp 	0 	0 	0.0.0.0:memcache 	0.0.0.0:*
                    904/memcached
                    udp 	0 	0 	0.0.0.0:maitrd 		0.0.0.0:*
                    15662/rpcbind
                    udp 	0 	0 	0.0.0.0:sunrpc 		0.0.0.0:*
                    15662/rpcbind
                    udp6 	0 	0 	localhost:323 		[::]:*
                    517/chronyd
                    udp6 	0 	0 	[::]:memcache 		[::]:*
                    904/memcached
                    udp6 	0 	0 	[::]:maitrd 		[::]:*
                    15662/rpcbind
                    udp6 	0 	0 	[::]:sunrpc 		[::]:*
                    15662/rpcbind

### Python virtualenv

* Crea entornos de Python aislados para administrar dependencias y diferencias de versión.
* Crea una carpeta que contiene:
* Versión específica de Python
* Ejecutables necesarios y dependencias
* La instalación de Ansible Tower crea dos instancias de virtualenv
* Uno para la torre de Ansible
* Uno para Ansible
* Permite la Torre Ansible estable al tiempo que permite cambios en los módulos de Python para libros de jugadas Ansible

virtualenves una herramienta de Python que crea entornos de Python aislados para mitigar problemas con dependencias o diferencias de versión. virtualenvfunciona creando una carpeta que contiene una versión específica de Python junto con los ejecutables y dependencias necesarios.

### Agregar módulos a virtualenv
Para agregar módulos a virtualenvAnsible:

* Activar Ansible virtualenv:

          [root@tower_host ~]# . /var/lib/awx/venv/ansible/bin/activate
          (ansible)[root@tower_host ~]#

* Instalar nuevos módulos de Python:

          (ansible)[root@tower_host ~]# pip install mypackagename
          

#### Ansible Tower Log Files en /var/log/tower/
Archivos de registro para servicios y demonios:

* callback_receiver.log
* fact_receiver.log
* socketio_service.log
* task_system.log
* tower.log

Los errores del servidor también se registraron aquí tower.log la información del archivo incluye errores y advertencias

Use el /etc/tower/conf.d/directorio para configurar otras necesidades de registro

### Ansible Tower Log Files in /var/log/supervisor/
Archivos de registro para todos los servicios, demonios y aplicaciones regidos por supervisord:

* awx-celery.log
* supervisord.log

* Ansible Tower envía registros detallados a servicios externos de agregación de registros
* Proporciona información sobre las tendencias técnicas o el uso de Ansible Tower
* Permite el monitoreo de anomalías, el análisis de eventos y la correlación de eventos.
* Los hechos del trabajo, las ejecuciones del trabajo, los eventos del trabajo y los mensajes de registro se envían a través de HTTP en formato JSON

nsible Tower es capaz de enviar registros detallados a servicios externos de agregación de registros. El registro ofrece información sobre las tendencias técnicas o el uso de Ansible Tower. Los datos se pueden usar para monitorear anomalías, analizar eventos y correlacionar eventos. Los datos más útiles son hechos de trabajo, ejecuciones de trabajo, eventos de trabajo y mensajes de registro. Todos estos datos se envían a través de una conexión HTTP en formato JSON.

### Sistemas de Monitoreo y Análisis
Sistemas de monitoreo y análisis con los que trabaja Ansible Tower:
* Splunk
* Loggly
* Sumo Logic
* Elastic Stack/Logstash

### Ansible Tower Configuration File
* /etc/tower/settings.py: Se utiliza para configurar ubicaciones de directorios estáticos, de proyectos y de salida de trabajos.
* Ubicaciones de directorio predeterminadas:
* /var/lib/awx/public/static: Directorio raíz estático
* Ubicación de los archivos de aplicación basados en Django
* /var/lib/awx/projects: Directorio raíz del proyecto
* Almacena archivos basados en proyectos en subdirectorios bajo la raíz del proyecto
* /var/lib/awx/job_status: Almacena la salida del estado del trabajo de libros de jugadas
* Configuración por defecto:
* Limita los libros de jugadas al /tmp directorio
* Limita a qué libro de jugadas puede acceder localmente en trabajos ejecutados por Ansible Tower

### Problemas para conectarse al host al ejecutar Playbooks
* Establecer conexión SSH con el host
* Revise el archivo de inventario y verifique los nombres de host y las direcciones IP

### Libro de jugadas no mostrado en la lista de plantillas de trabajo
* Verifique la sintaxis YAML del libro de jugadas y verifique que Ansible pueda analizarlo
* Verifique los permisos y la propiedad de la ruta del proyecto ( /var/lib/awx/projects/) para el awxusuario

### El estado del libro de jugadas permanece Pending
* Asegúrese de que el servidor de Ansible Tower tenga suficiente RAM disponible
* Asegúrese de que los servicios regidos por se supervisor destén ejecutando
* Emitir supervisorctl status
* Asegúrese de que la partición donde /var/ se encuentra el directorio tenga más de 1 GB de espacio libre
* Los trabajos no se pueden completar cuando no hay suficiente espacio libre en la /var/ partición
* Reiniciar la infraestructura usando el ansible-tower-service restart 

# Configuración TLS y SSL

### TLS (Transport Layer Security) y SSL (Secure Sockets Layer) son protocolos criptográficos
* Se utiliza para proteger las comunicaciones de red.
* Ansible Tower crea un certificado SSL autofirmado y un archivo de claves para la comunicación HTTPS
* El certificado autofirmado debe importarse y aceptarse en el navegador web
* En el entorno de producción empresarial, los certificados autofirmados se reemplazan por certificados SSL personalizados firmados por CA
* La infraestructura de Ansible Tower permite reemplazar el certificado predeterminado
* El nombre del certificado de reemplazo y el nombre de la clave deben ser idénticos a los originales

### Certificado SSL y ubicación predeterminada del archivo de claves
* La ubicación predeterminada para el certificado SSL de Ansible Tower y el archivo de claves se define en **/etc/nginx/nginx.conf**:


          (...)
             server {
                              listen 443 default_server ssl;
                    listen 127.0.0.1:80 default_server;
                              listen [::1]:80 default_server;

                    # If you have a domain name, this is where to add it
                    server_name _;
                    keepalive_timeout 65;

                    ssl_certificate /etc/tower/tower.cert;
                    ssl_certificate_key /etc/tower/tower.key;
                    ssl_session_cache shared:SSL:50m;
                    ssl_session_timeout 1d;
                    ssl_session_tickets off;

                    # intermediate configuration
                    ssl_protocols TLSv1.2;
                    ssl_ciphers
          'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE- ECDSA-
          CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDH
          E-
          RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-EC
          DSA- AES128-SHA256:ECDHE-RSA-AES128-SHA256';
                    ssl_prefer_server_ciphers on;
          (...)
          

La ubicación predeterminada para el certificado SSL de Ansible Tower y el archivo de claves se define en el **/etc/nginx/nginx.conf** archivo de configuración de Nginx. Puede encontrarlos en el serverbloque que define ssly el 443puerto, como se muestra aquí.


### Reemplazo de certificados TLS / SSL

Para utilizar un certificado TLS firmado por una Autoridad de certificación (CA), primero debe generar una solicitud de certificado. Ese archivo de solicitud debe enviarse a una autoridad de certificación para su firma. Una vez que tenga el archivo de certificado firmado, puede reemplazar el certificado predeterminado creado durante la instalación de Ansible Tower:

1.- Cambie el nombre del certificado TLS firmado por CA en formato PEM a tower.cert.

2.- Cambie el nombre de la clave privada correspondiente en formato PEM a tower.key.

3.- Copie ambos archivos al /etc/towerdirectorio.

4.- Reinicie la infraestructura de Ansible Tower usando el ansible-tower-service comando con la restartopción.

5.- Pruebe la conexión desde un navegador y revise los detalles del nuevo certificado de confianza.

Es importante recordar cambiar el contexto SELinux predeterminado para el /etc/tower directorio. El contexto correcto para que este directorio funcione con el **ipa-getcert  cert_t**. Puede establecer el contexto correcto emitiendo el **semanage fcontext -a -t cert_t"etc/tower(/.*)?"** comando seguido del **restorecon -FvvR /etc/tower/**.

# tower-manage Overview
* Ansible Tower viene con la tower-manageutilidad de línea de comando
* Se usa para acceder a información detallada de la Torre Ansible interna
* Debe ser root o awx usuario para usar
* Casos de uso más comunes:
* Importar inventario
* Limpieza de datos antiguos.
* admin recuperación de contraseña

### Cambiar la admincontraseña de la torre Ansible

          [root@towerhost ~]# tower-manage changepassword admin_superuser
          Changing password for user 'admin_superuser'
          Password: new_password
          Password (again): new_password
          Password changed successfully for user 'admin_superuser'
          

### Crear nuevo superusuario de torre Ansible

          [root@towerhost ~]# tower-manage createsuperuser
          Username (leave blank to use 'root'): admin3
          Email address: admin@demo.com
          Password: new_password
          Password (again): new_password
          Superuser created successfully.
          

### Importar inventario con tower-manage
Estructura de inventario necesaria:

          [root@towerhost ~]# tree inventory/
          inventory/
          |-- group_vars
          | `-- mygroup
          |-- host_vars
          | `-- myhost
          `-- hosts

* Ejemplo de comando de importación:

          [root@towerhost ~]# tower-manage inventory_import --source=inventory/ \
          --inventory-name="My Tower Inventory"
          
* Para un archivo plano único, el mismo comando pero cambia la fuente del directorio a un archivo específico
* Ejemplo: *--source=./my_inventory_file*
* Para sobrescribir datos existentes, agregue --overwrite_vars como argumento

tower-manageofrece un mecanismo por el cual el administrador puede importar el inventario almacenado en un archivo directamente en Ansible Tower desde la CLI. Para usar esta tower-managefunción, debe haber un inventario de destino presente en Ansible Tower. Este inventario existente se utiliza como destino para la importación.

Para importar o sincronizar un inventario estática externa con host_varsy group_varsen Ansible Torre, sus necesidades de inventario para estar en una estructura que se parece al primer ejemplo aquí. Luego puede usar el tower-manage comando con la inventory_importopción que especifica la fuente y el nombre del inventario como argumentos, como se muestra en el segundo ejemplo.

Si el inventario es un único archivo plano, la importación como en el segundo ejemplo, pero cambiar el --source=argumento de que apunta a un directorio que apunta a su archivo de inventario, por ejemplo: --source=./my_inventory_file.

Los datos de inventario ya almacenados en Ansible Tower se combinan de manera predeterminada con datos de una fuente externa. Este comportamiento predeterminado agrega nuevas variables de la fuente externa al tiempo que conserva las variables que no provienen de los datos externos. Puede sobrescribir los datos existentes especificando la --overwrite_varsopción como argumento.

### Limpiar datos antiguos con tower-manage

#### cleanup_jobs --days=
Elimina permanentemente los detalles del trabajo y la salida del trabajo anterior a un número específico de días.

#### cleanup_activitystream --days=
Elimina permanentemente los flujos de actividad que son más antiguos que un número específico de días.

#### cleanup_facts --granularity=
Mantiene una exploración de hechos para cada host para cada ventana de tiempo.

# Copia de seguridad de la torre Ansible
* Revisión de arquitectura de la Torre Ansible:
* Marco para ejecutar y administrar infraestructura Ansible
* Aplicación web Django con front-end web y back-end API RESTful
* Base de datos PostgreSQL
* Instalación predeterminada en una sola máquina
* El libro de jugadas de configuración de Ansible Tower ofrece capacidades de respaldo
* Debe tener espacio en disco adecuado para la copia de seguridad de los archivos de configuración, claves, base de datos PostgreSQL, etc.
* Requiere credenciales del sistema para bases de datos locales (pueden necesitar rooto sudoacceder) y remotas
* Credenciales ubicadas en el archivo de inventario en el directorio de instalación, invocadas por setup.shscript

### Playbooks de respaldo
* backup.yml: Crea copias de seguridad de archivos de configuración, claves, base de datos PostgreSQL, etc.
* restore.yml: Restaura archivos y datos respaldados

### Archivo de respaldo
* tower.db: Archivo de volcado de la base de datos PostgreSQL
* /conf: Directorio de configuración que contiene archivos del /etc/tower/ directorio
* /job_status: Directorio para archivos de salida de trabajo
* /projects: Directorio para proyectos manuales
* /static: Directorio para personalizaciones de interfaz web, por ejemplo, logotipos personalizados

Además de la setup.shsecuencia de comandos, el backup.yml y restore.yml libros de jugadas se proporcionan para copia de seguridad y restauración del sistema Ansible Tower. El backup.yml archivo se utiliza para crear una copia de seguridad de los archivos de configuración, las claves y otros archivos relevantes de Ansible Tower, incluida la base de datos de la instalación de Ansible Tower. El restore.ymlarchivo se utiliza para restaurar los archivos y datos respaldados. El archivo de respaldo consta de los siguientes archivos y directorios:

* tower.db: El archivo de volcado de la base de datos PostgreSQL
* /conf: El directorio de configuración que contiene archivos del /etc/tower/directorio
* /job_status: El directorio para los archivos de salida del trabajo
* /projects: El directorio para proyectos manuales
* /static: El directorio para las personalizaciones de la interfaz web, como logotipos personalizados

### Procedimiento de respaldo, pasos 1-2
1.- Como root, cambie al directorio de instalación:

          [root@towerhost ~]# find / -iname 'ansible*'
          /root/ansible-tower-setup-3.3.1-1
          [root@towerhost ~]# cd /root/ansible-tower-setup-3.3.1-1
          [root@towerhost ansible-tower-setup-3.3.1-1]#

2.- Ejecutar setup.sh -b:

          [root@towerhost ansible-tower-setup-3.3.1-1]# ./setup.sh -b
          (...)

          RUNNING HANDLER [backup : Remove the backup directory.] ***********************************************************************************************
          changed: [localhost] => {"changed": true, "path": "/var/backups/tower/2018-05-07-03:24:55/", "state": "absent"}

          RUNNING HANDLER [backup : Remove common directory.] ***************************************************************************************************
          changed: [localhost] => {"changed": true, "path": "/var/backups/tower/common/", "state": "absent"}

          RUNNING HANDLER [backup : Remove the backup tarball.] *************************************************************************************************
          changed: [localhost] => {"changed": true, "path": "/var/backups/tower/localhost.tar.gz", "state": "absent"}

          RUNNING HANDLER [backup : Remove the common tarball.] *************************************************************************************************
          changed: [localhost] => {"changed": true, "path": "/var/backups/tower/common.tar.gz", "state": "absent"}

          RUNNING HANDLER [backup : Remove backup dest stage directory.] ****************************************************************************************
          changed: [localhost] => {"changed": true, "path": "/root/ansible-tower-setup-3.2.3/2018-05-07-03:24:55", "state": "absent"}

          PLAY RECAP ********************************************************************************************************************************************
          localhost                  : ok=31   changed=23   unreachable=0    failed=0

          The setup process completed successfully.
          Setup log saved to /var/log/tower/setup-2018-05-07-03:24:52.log

Siga los pasos que se muestran aquí para crear una nueva copia de seguridad de la infraestructura de Ansible Tower.

Como root usuario, ubique el directorio de instalación de Ansible Tower y cámbielo a ese directorio. Ejecute el setup.sh script con la -bopción de iniciar la configuración de Ansible Tower y la copia de seguridad de la base de datos.


3.- Enumere el directorio actual y verifique que se haya creado el archivo de respaldo:

          [root@towerhost ansible-tower-setup-3.3.1-1]# ls -l
          (...)
          drwxr-xr-x. 20 root root  4096 Feb 19 19:47 roles
          -rwxr-xr-x.  1 root root  9628 Feb 19 19:47 setup.sh
          -rw-r--r--.  1 root root 68771 May  7 03:25 tower-backup-2018-05-07-03:24:55.tar.gz
          lrwxrwxrwx.  1 root root    71 May  7 03:25  -> /root/ansible-tower-setup-3.3.1-1/tower-backup-2018-05-07-03:24:55.tar.gz

* Enlace simbólico tower-backup-latest.tar.gz, apunta al último archivo de respaldo
* Se usa de forma predeterminada para recuperar la infraestructura de Ansible Tower de la copia de seguridad

### Restauración desde copia de seguridad
* El archivo de respaldo contiene todos los archivos relevantes de Ansible Tower
* Para restaurar, ejecute el restore.yml libro de jugadas con el archivo de respaldo
* La restauración falla si el archivo de respaldo no está disponible
* Con múltiples archivos de copia de seguridad, asegúrese de tower-backup-latest.tar.gz puntos para corregir uno
* Para usar un archivo de copia de seguridad anterior, elimine tower-backup-latest.tar.gz y vuelva a crear
* Para la restauración completa de la infraestructura de Ansible Tower, es más fácil:
* Instale Ansible Tower en otra máquina
* Restaurar desde el archivo de respaldo
* Al restaurar desde una copia de seguridad, asegúrese de restaurar a la misma versión desde la que se realizó la copia de seguridad.



### Procedimiento de restauración, pasos 1-2
Como root, cambie al directorio de instalación:

          [root@towerhost ~]# find / -iname 'ansible*'
          /root/ansible-tower-setup-3.3.1-1
          [root@towerhost ~]# cd /root/ansible-tower-setup-3.3.1-1
          [root@towerhost /root/ansible-tower-setup-3.3.1-1]#
          
Asegúrese de que el archivo de respaldo esté en ese directorio:

          root@towerhost ansible-tower-setup-bundle-3.1.1-1.el7]# ls -l
          (...)
          [root@towerhost ansible-tower-setup-3.3.1-1]# ls -l
          (...)
          drwxr-xr-x. 20 root root  4096 Feb 19 19:47 roles
          -rwxr-xr-x.  1 root root  9628 Feb 19 19:47 setup.sh
          -rw-r--r--.  1 root root 68771 May  7 03:25 tower-backup-2018-05-07-03:24:55.tar.gz
          lrwxrwxrwx.  1 root root    71 May  7 03:25  -> /root/ansible-tower-setup-3.3.1-1/tower-backup-2018-05-07-03:24:55.tar.gz
          
* Enlace simbólico tower-backup-latest.tar.gz, apunta al último archivo de respaldo
* Para restaurar desde un archivo anterior, vuelva a crear el enlace y apunte al archivo correcto

La restauración se realiza a través del restore.ymllibro de jugadas provisto , ejecutado por el mismo setup.shscript que usó para instalar y respaldar su infraestructura de Ansible Tower.

Siga los pasos que se muestran aquí para restaurar la infraestructura de Ansible Tower. Primero, cambie al directorio de instalación, luego confirme que el archivo de respaldo está en el directorio.

3.- Ejecutar setup.sh -r:

          [root@towerhost ansible-tower-setup-3.2.3]# ./setup.sh -r
          (...)
          00ms", "Result": "success", "RootDirectoryStartOnly": "no",
          "RuntimeDirectoryMode":
          "0755", "SameProcessGroup": "no", "SecureBits": "0", "SendSIGHUP": "no",
          "SendSIGKILL": "yes", "Slice": "system.slice", "StandardError": "inherit",
          "StandardInput": "null", "StandardOutput": "journal", "StartLimitAction":
          "none",
          "StartLimitBurst": "5", "StartLimitInterval": "10000000",
          "StartupBlockIOWeight":
          "18446744073709551615", "StartupCPUShares": "18446744073709551615",
          "StatusErrno":
          "0", "StopWhenUnneeded": "no", "SubState": "dead", "SyslogLevelPrefix":
          "yes", "SyslogPriority": "30", "SystemCallErrorNumber": "0", "TTYReset": "no",
          "TTYVHangup": "no", "TTYVTDisallocate": "no", "TimeoutStartUSec": "1min 30s",
          "TimeoutStopUSec": "1min 30s", "TimerSlackNSec": "50000", "Transient": "no",
          "Type": "forking", "UMask": "0022", "UnitFilePreset": "disabled",
          "UnitFileState":
          "enabled", "WantedBy": "multi-user.target", "Wants": "system.slice",
          "WatchdogTimestampMonotonic": "0", "WatchdogUSec": "0"}, "warnings": []}

          PLAY RECAP *********************************************************************
          localhost		 : ok=26   changed=18   unreachable=0   failed=0

          The setup process completed successfully.
          Setup log saved to /var/log/tower/setup-2017-03-30-04:19:05.log

4.- Inicie sesión en la interfaz web de Ansible Tower y verifique que se haya restaurado la infraestructura correcta

Es importante recordar incluir la -ropción. Si no lo incluye, Ansible Tower inicia el proceso de instalación desde el principio. Si eso sucede, espere a que finalice el proceso de instalación, luego simplemente restaure la copia de seguridad.

Cuando el script de restauración termine de ejecutarse, inicie sesión en la interfaz web de Ansible Tower y verifique que se haya restaurado la infraestructura correcta.

Para obtener más información, consulte la sección "Copia de seguridad y restauración de la torre" de la Guía de administración de Ansible Tower .

### Ansible Tower REST API
Visión general
* REST API puede controlar el entorno de Ansible Tower fuera de la interfaz web
* Útil cuando se integra con scripts personalizados o aplicaciones externas que acceden a la API a través de HTTP
* Cualquier lenguaje de programación, marco o sistema con soporte de protocolo HTTP puede usar API
* Manera fácil de automatizar tareas repetitivas e integrar otros sistemas informáticos empresariales con la infraestructura de Ansible Tower
* REST es una arquitectura de diseño que se centra en los recursos para un servicio específico y sus representaciones.
* El cliente envía una solicitud al elemento del servidor ubicado en URI y realiza operaciones con métodos HTTP estándar
* Ejemplos: GET, POST, PUT y DELETE
* Proporciona comunicación sin estado entre el cliente y el servidor.
* Cada solicitud del cliente actúa independientemente de otras solicitudes y contiene toda la información para completar la solicitud

### Recuperación y listado de API REST
* Para recuperar la representación del punto de entrada principal de la API:

          [user@demo ~]# curl -X GET https://tower.lab.example.com/api/ -k
          {"available_versions":{"v1":"/api/v1/","v2":"/api/v2/"},"custom_logo":"","custom_login_info":"","description":"AWX REST API","current_version":"/api/v2


* Para enumerar y revisar todas las API:

          [user@demo ~]# curl -X GET https://tower.lab.example.com/api/v2/ -k
          {"authtoken":"/api/v2/authtoken/","ping":"/api/v2/ping/","instances":"/api/v2/instances/","instance_groups":"/api/v2/instance_groups/","config":"/api/v2/config/","settings":"/api/v2/settings/","me":"/api/v2/me/","dashboard":"/api/v2/dashboard/","organizations":"/api/v2/organizations/","users":"/api/v2/users/","projects":"/api/v2/projects/","project_updates":"/api/v2/project_updates/","teams":"/api/v2/teams/","credentials":"/api/v2/credentials/","credential_types":"/api/v2/credential_types/","inventory":"/api/v2/inventories/","inventory_scripts":"/api/v2/inventory_scripts/","inventory_sources":"/api/v2/inventory_sources/","inventory_updates":"/api/v2/inventory_updates/","groups":"/api/v2/groups/","hosts":"/api/v2/hosts/","job_templates":"/api/v2/job_templates/","jobs":"/api/v2/jobs/","job_events":"/api/v2/job_events/","ad_hoc_commands":"/api/v2/ad_hoc_commands/","system_job_templates":"/api/v2/system_job_templates/","system_jobs":"/api/v2/system_jobs/","schedules":"/api/v2/schedules/","roles":"/api/v2/roles/","notification_templates":"/api/v2/notification_templates/","notifications":"/api/v2/notifications/","labels":"/api/v2/labels/","unified_job_templates":"/api/v2/unified_job_templates/","unified_jobs":"/api/v2/unified_jobs/","activity_stream":"/api/v2/activity_stream/","workflow_job_templates":"/api/v2/workflow_job_templates/","workflow_jobs":"/api/v2/workflow_jobs/","workflow_job_template_nodes":"/api/v2/workflow_job_template_nodes/","workflow_job_nodes":"/api/v2/workflow_job_nodes/
          

Se puede acceder a los recursos, también conocidos como entidades de datos, a través de la API REST a través de rutas URI. La solicitud que se muestra en el primer ejemplo recupera una representación del punto de entrada principal de la API. Se puede acceder a la API utilizando un navegador web gráfico o herramientas de navegación de línea de comandos de Linux. En este caso, la solicitud accede a la API REST desde la línea de comandos emitiendo el método GET utilizando la curlherramienta.

Ansible Tower ofrece una forma de enumerar y revisar todas las API disponibles, utilizando la URL que se muestra en el segundo ejemplo. Este punto de entrada proporciona una colección de enlaces en el entorno de la Torre Ansible. Como puede ver, hay muchos enlaces para elegir.


### Recuperación de flujos de actividad y recursos que requieren credenciales
* Para recuperar la lista de flujos de actividad:

          [user@demo ~]# curl -X GET
          https://tower.lab.example.com/api/v2/activity_stream/ -k
          {"detail":"Authentication credentials were not provided."}
          
* Para acceder a los recursos que requieren credenciales:

          [user@demo ~]# curl -X GET --user admin:redhat https://tower.lab.example.com/api/v2/activity_stream/ -k
           {"count":12,"next":null,"previous":null,"results":[{"id":1,"type":"activity_stream","url":"/api/v2/activity_stream/1/","related":{},"summary_fields":{},"timestamp":"2018-05-07T05:48:38.851500Z","operation":"update","changes":{"modified":["2018-05-07 05:48:18.395608+00:00","2018-05-07 05:48:38.788555+00:00"]},"object1":"credential_type","object2":"","object_association":""},{"id":2,"type":"activity_stream","url":"/api/v2/activity_stream/2/","related":{"setting":"/api/v2/settings/ui/"},"summary_fields":{"setting":[{"category":"ui","name":"PENDO_TRACKING_STATE"}]},"timestamp":"2018-05-07T05:48:56.737551Z","operation":"create","changes":{"key":"PENDO_TRACKING_STATE","id":1,"value":"detailed"},"object1":"setting","object2":"","object_association":""},{"id":3,"type":"activity_stream","url":"/api/v2/activity_stream/3/","related":{"user":["/api/v2/users/1/"]},"summary_fields":{"user":[{"username":"admin","first_name":"","last_name":"","id":1}]},"timestamp":"2018-05-07T05:49:04.515772Z","operation":"create","changes":{"username":"admin","first_name":"","last_name":"","is_active":true,"id":1,"is_superuser":true,"is_staff":true,"password":"hidden","email":"admin@example.com","date_joined":"2018-05-07 05:49:04.422845+00:00"},"object1":"user","object2":"","object_association":""},{"id":4,"type":"activity_stream","url":"/api/v2/activity_stream/4/","related":{"organization":["/api/v2/organizations/1/"],"actor":"/api/v2/users/1/"},"summary_fields":{"organization":[{"description":"","id":1,"name":"Default"}],"actor":{"username":"admin","first_name":"","last_name":"","id":1}},"timestamp":"2018-05-07T05:49:43.312964Z","operation":"create","changes":{"description":"","id":1,"name":"Default"},"object1":"organization","object2":"","object_association":""},{"id":5,"type":"activity_stream","url":"/api/v2/activity_stream/5/","related":{"project":["/api/v2/projects/4/"],
          (...)
         
Para examinar qué actividades se han realizado utilizando la infraestructura de Ansible Tower, una opción es utilizar el activity_stream enlace de recursos. Realice una solicitud GET a ese recurso para recuperar la lista de flujos de actividad, como se muestra en el primer ejemplo.

Parte de la información generada por la API no está disponible públicamente. Para acceder a este recurso, debe iniciar sesión con sus credenciales de usuario, como se muestra en el segundo ejemplo. Este resultado de la API está en formato JSON, que puede ser difícil de leer.

### Monitoreo del estado de la Ansible Tower
* Verifique la disponibilidad del servidor utilizando la pingAPI:

          [user@demo ~]# curl -X GET https://tower.lab.example.com/api/v2/ping/ -k
          {"instance_groups":[{"instances":["localhost"],"capacity":50,"name":"tower"}],"instances":[{"node":"localhost","heartbeat":"2018-05-07T07:33:47.569Z","version":"3.2.3","capacity":50}],"ha":false,"version":"3.2.3","active_node":"localhost"}

* La salida incluye:
* El estado del servidor
* Estado del clúster de Ansible Tower HA
* Nombre de nodo y disponibilidad
* Marca de tiempo del latido del corazón
* Versión Ansible Tower

# tower-cli Mando
### Instalando tower-cli
* Use tower-clipara ejecutar comandos de Ansible Tower desde la línea de comandos
* Obtenga tower-clide estas fuentes:
* Como un paquete en PyPI
* enlace: https: //github.com/ansible/tower-cli [a través de GitHub ^]
* Como RPM del repositorio Fedora EPEL Yum
* Instalar a través de pip:

          [user@demo ~]# pip install ansible-tower-cli

### Configurando tower-cli
* tower-cli puede editar su propia configuración, o los usuarios pueden editar directamente el archivo de configuración
* Configuración almacenada en .tower_cli.cfg archivo oculto en el directorio de inicio

Metodo preferido:

          [user@demo ~]# tower-cli config key value
          Obtenga la lista de opciones de configuración:

          [user@demo ~]# tower-cli config
          # User options (set with `tower-cli config`; stored in ~/.tower_cli.cfg).
          host: tower.lab.example.com
          username: admin
          password: redhat

          # Defaults.
          description_on: False
          verbose: False
          certificate:
          format: human
          color: True
          verify_ssl: True
          

### Configurando tower-cli

Establecer host opción:

          [user@demo ~]# tower-cli config host tower.demo.com
          
Establecer usernameopción:

          [user@demo ~]# tower-cli config username Demo

Establecer passwordopción:

          [user@demo ~]# tower-cli config password myDemoPassword

Compruebe Ansible Tower y tower-cliversiones:

          [user@demo] tower-cli version
          Tower CLI 3.3.0
          API v2
          Ansible Tower 3.2.3
          Ansible 2.5.2
          

### Utilizando tower-cli
Formato de uso:

          [user@demo ~]# tower-cli {resource} {action} ...

Ejemplo de lanzamiento de trabajo:

          [user@demo ~]# tower-cli job launch --job-template=33

Ejemplo de monitoreo de trabajo:

          [user@demo ~]# tower-cli job monitor 33
          
#### Casos de uso
tower-cli casos de uso:
* Obtener acceso a Ansible Tower a través de un script
* Lanzar trabajos desde sistemas remotos o scripts
* Casos de uso de API REST:
* Integrando aplicaciones de gestión del sistema con Ansible Tower
* Red Hat Satellite Server 6
* Red Hat CloudForms

