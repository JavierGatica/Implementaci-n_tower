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

