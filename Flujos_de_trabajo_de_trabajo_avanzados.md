# Flujos de trabajo de trabajo avanzados

### Variables extra
* Ansible Tower permite variaciones en la ejecución del libro de jugadas
* Fomenta la reutilización del libro de jugadas
* La solicitud de inicio permite la modificación de parámetros relacionados con inventarios, credenciales y etiquetas cuando se inicia el trabajo para la ejecución del libro de jugadas
* La solicitud al inicio le permite modificar la ejecución del libro de jugadas al especificar variables adicionales durante el inicio del trabajo
* Equivalente -eu --extra-varsopciones paraansible-playbook
* ansible-playbook ofrece una opción adicional fácil de usar para la entrada variable en tiempo de ejecución
* Agregar vars_promptsección al libro de jugadas para solicitar a los usuarios que ingresen durante la ejecución del libro de jugadas

### vars_prompt Ejemplo
* Agregar vars_promptsección al libro de jugadas
* Solicita al usuario que ingrese la versión de lanzamiento para tareas como la implementación de software:

      vars_prompt:
        - name: "release_version"
        prompt: "Version to deploy"

* Si las variables se definen con -eu --extra-varsopciones ansible-playbookactivadas, se omiten las solicitudes de las mismas variables durante la ejecución del libro de jugadas

Aquí hay un ejemplo que muestra cómo vars_promptse puede agregar una sección a un libro de jugadas para solicitar al usuario que ingrese una versión de lanzamiento para una tarea como la implementación de software.


### Ventajas
Solicitar la entrada del usuario ofrece ventajas sobre la especificación de variables adicionales usando -eu --extra-vars opciones paraansible-playbook

* Los usuarios no necesitan conocimiento sobre:
* Funcionamiento de variables extra.
* Uso de variables extra.
* Nombres de variables adicionales
* Las solicitudes pueden contener cualquier cadena personalizada
* Puede ser redactado de manera fácil de usar
* Más fácil de entender para el usuario sin conocimientos técnicos de Ansible.

### Desventajas
* Debido a la naturaleza interactiva, los mensajes se omiten cuando los libros de jugadas se ejecutan en una sesión no interactiva
* Ejemplos: de trabajo cron o trabajo en Ansible Tower
* Característica de encuesta de plantilla de trabajo equivalente a ansible-playbookcaracterística de solicitud

### Encuestas de plantillas de trabajo
* Alternativa para especificar variables adicionales en tiempo de ejecución
* Similar a la característica ofrecida por la vars_promptsección del libro de jugadas Ansible
* Establece variables adicionales solicitando la entrada del usuario antes de la ejecución del libro de jugadas
* Le permite especificar variables adicionales durante la ejecución del libro de jugadas
* Cuando la opción Preguntar al iniciar está habilitada para variables adicionales, se solicita a los usuarios que ingresen el par clave / valor al ejecutar el trabajo
* Requiere conocer los nombres de las variables adicionales utilizadas en el libro de jugadas
*La función de encuesta de plantilla de trabajo solo está disponible con licencias de nivel empresarial

### Preguntas fáciles de usar
* Encuesta plantilla de trabajo ofrece ventajas sobre permitiendo Preguntar al lanzamiento de variables extra campo
* Las encuestas permiten a los usuarios recibir preguntas con frases personalizadas
* Similar a las indicaciones habilitadas por la var_promptsección del libro de jugadas

Además de ofrecer un aviso fácil de usar, las encuestas también pueden definir reglas y validar las entradas de los usuarios. Las respuestas de los usuarios a las preguntas de la encuesta se pueden restringir a uno de los siguientes siete tipos de respuestas:

* Texto, que representa texto de una sola línea.
* Área de texto, que representa texto de varias líneas
* Contraseña, que representa los datos que se tratan como información confidencial.
* Opción múltiple (selección única), que presenta una lista de opciones de las cuales solo se puede elegir una opción como respuesta
* Opción múltiple (selección múltiple), que presenta una lista de opciones de las cuales se pueden elegir una o más opciones como respuesta
* Entero, que representa un número entero
* Float, que representa un número decimal

### Plantillas de trabajo de flujo de trabajo
* El número de libros de jugadas aumenta a medida que aumenta el uso de Ansible
* Playbook realiza tareas asociadas con la función de TI
* Las plantillas de trabajo permiten a los usuarios ejecutar libros de jugadas como trabajos
* La ejecución de tareas a menudo se requiere en múltiples reinos funcionales
* Ejemplo: el aprovisionamiento del servidor involucra equipos de redes, operaciones y desarrollo de aplicaciones
* Para ejecutar múltiples libros de jugadas en Ansible Tower, inicie trabajos de cada equipo
* Debe ejecutarse en orden paralelo al proceso de TI
* El trabajo del equipo sigue solo si los trabajos del equipo anterior se completaron con éxito

### Visión general
* La plantilla de trabajo de flujo de trabajo le permite consolidar la ejecución de múltiples plantillas de trabajo en una sola función
* Refleja el proceso de TI que implica la ejecución de múltiples trabajos de Ansible Tower
* La plantilla de trabajo de flujo de trabajo solo está disponible con licencias de nivel empresarial
* Con el editor de flujo de trabajo gráfico, las plantillas de trabajo de flujo de trabajo pueden encadenar varias plantillas de trabajo e incorporar la toma de decisiones
* Crea flujos de trabajo complejos y ramificados guiados por los resultados de cada trabajo ejecutado
* La plantilla de trabajo de flujo de trabajo amplía la plantilla de trabajo singular de ejecución de libro de jugadas
* Consolida múltiples plantillas de trabajo de diferentes equipos funcionales
* Crea recursos de la Torre Ansible de silo cruzado
* Permite la ejecución completa y automatizada del proceso de TI

### Creación
* Debe crear una plantilla de trabajo de flujo de trabajo antes de definir y asociar el flujo de trabajo
* La creación puede ser dentro o sin contexto de organización
* Dentro de la organización: se adminrequiere un rol en la organización
* Sin organización: System Administrator requiere un tipo de usuario único

### Alertas
* Para trabajos críticos, los administradores necesitan una notificación inmediata del éxito / fracaso del trabajo
* Ansible Tower ofrece alertas inmediatas de resultados de ejecución de trabajos
* Las plantillas de notificación permiten a los administradores definir cómo se envían las notificaciones
* Mecanismos de alerta soportados por Tower:
* Protocolos abiertos: correo electrónico, IRC
* Soluciones patentadas: HipChat, Slack

