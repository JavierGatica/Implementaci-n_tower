# Actualizaciones dinámicas de inventario

### Introducción
* Ansible utiliza archivos de inventario para definir la colección de hosts en los que se pueden iniciar trabajos
* Los entornos de TI modernos cambian constantemente
* Los archivos de inventario deben reflejar los cambios.
* La interfaz web de Ansible Tower facilita la configuración de inventarios estáticos
* Ansible también permite usar inventario dinámico para extraer fuentes externas:
  * Cloud providers
  * Cobbler
  * LDAP
  * Software CMDB de terceros

Los entornos de TI empresariales modernos siempre cambian, con sistemas que se agregan y eliminan. Estos cambios en el entorno de TI deben reflejarse en los archivos de inventario de Ansible. Tales listas estáticas son difíciles de mantener actualizadas, especialmente cuando se utilizan tecnologías de virtualización o en la nube. Los administradores de sistemas o desarrolladores de software pueden usar Ansible para ejecutar trabajos con un solo comando en cientos de servidores, pero Ansible necesita saber en qué servidores ejecutar esos trabajos.

En módulos anteriores, configuró un inventario estático utilizando la interfaz web de Ansible Tower. Este módulo describe cómo usar el inventario dinámico para extraer el inventario de fuentes externas como proveedores de la nube, Cobbler, LDAP o software CMDB de terceros. Todas estas opciones son compatibles con Ansible a través de un mecanismo de inventario externo.

### Características del inventario dinámico de Ansible Tower
* Admite la sincronización de todas las fuentes de inventario dinámico
* Almacena datos de inventario en bases de datos accesibles a través de la interfaz de usuario web y la API REST
* Admite estas fuentes de inventario externas de forma predeterminada:
 * Servidores en la nube Rackspace
 * Amazon EC2
 * Google Compute Engine
 * Administrador de recursos de Microsoft Azure
 * VMware vCenter
 * Red Hat Satellite 6
 * Red Hat CloudForms ®
 * OpenStack
 
El uso del inventario dinámico es la práctica recomendada en entornos de TI grandes y que cambian rápidamente donde los sistemas se implementan, prueban y luego se eliminan. Ansible Tower le permite sincronizar todas las fuentes de inventario dinámicas y almacena esos datos de inventario en una base de datos accesible por la interfaz web y REST API. De manera predeterminada, Ansible Tower viene con soporte de inventario dinámico para estas fuentes de inventario externas:

### Escenario basado en la nube
* El uso de tecnologías en la nube como Amazon EC2 produce muchos cambios en la infraestructura mantenida
* Los anfitriones van y vienen con el tiempo
* Los hosts se crean e inician mediante aplicaciones externas o mediante AWS Auto Scaling
* Desafío para mantener un archivo de inventario preciso
* Ansible Tower facilita el uso de proveedores de nube compatibles como fuente de inventario
* Primer paso: crear las credenciales apropiadas
