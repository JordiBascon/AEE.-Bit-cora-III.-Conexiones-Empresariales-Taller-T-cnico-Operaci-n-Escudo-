# Fundamentos de Seguridad y Auditoría
## 2\. Fase de Investigación: Comprendiendo el corazón del sistema

### Reto de Investigación 1

El sistema Syslog clasifica cada evento cruzando dos variables fundamentales que definen su origen y su importancia:

**1\. Facilidad (Facility)**  
Esta variable identifica qué parte del sistema o qué tipo de programa genera el mensaje de log. 

**2\. Prioridad o Severidad (Severity)**  
Define la gravedad o urgencia del evento registrado. Los niveles estándar de severidad, ordenados de mayor a menor urgencia, son:

**Herramientas de Análisis en Ubuntu**  
Para investigar y filtrar estos eventos basándose en estas variables, las fuentes recomiendan el uso de la terminal (CLI) debido a su potencia:

Comprender esta estructura de "Facilidad.Prioridad" permite a los administradores configurar reglas en el servicio de syslog (como rsyslog) para decidir, por ejemplo, que todos los mensajes de auth con prioridad err o superior se guarden en un archivo separado o se envíen a un servidor centralizado de monitoreo.

**Permitir que el archivo /var/log/auth.log tenga permisos de lectura para usuarios** no privilegiados se considera una negligencia grave debido a la sensibilidad de la información que almacena y los principios fundamentales de seguridad que vulnera:

Este archivo funciona como una bitácora detallada de seguridad que, en manos equivocadas, facilita la explotación del sistema:

**1.- Enumeración de usuarios:** Permite a un atacante identificar nombres de usuario válidos en el sistema, lo cual es el primer paso para ataques de fuerza bruta  

**2.- Fuga de contraseñas:** Es común que los usuarios escriban accidentalmente su contraseña en el campo de "nombre de usuario". Si esto ocurre, la contraseña queda escrita en texto plano en el log de autenticación, quedando expuesta a cualquier usuario con permisos de lectura.  

**3.- Monitoreo de actividad:** Permite saber cuándo y desde qué IPs se conectan otros usuarios, facilitando la planificación de ataques basados en los hábitos de los administradores.

Para diferenciar un intento fallido de conexión remota por SSH de un fallo de contraseña de un usuario local en los registros del sistema se deben observar tres elementos clave de información:

**1.- Dirección IP de origen**  
**Intento SSH:** Las fuentes indican que el protocolo SSH se utiliza para conectarse a una máquina remota. Por ello, un registro de fallo de SSH incluirá necesariamente la dirección IP del equipo remoto desde el cual se intenta el acceso

**Fallo Local:** En un intento frente a la pantalla física, no existe una dirección IP de origen externa, ya que el sujeto accede desde el propio terminal donde reside el dato

**2.- El proceso y si PID**  
**Intento SSH:** El registro mostrará que el servicio que genera el evento es sshd. Cada intento fallido tendrá un PID específico asignado a la instancia del servicio sshd que manejó esa conexión fallida.

**Fallo Local:** El registro identificará procesos locales de autenticación como login, pam o el gestor de sesiones gráficas.

**3.- ID del terminal**  
**Intento SSH:** El log suele mencionar que el intento proviene de una red insegura y utiliza el puerto asociado al servicio

**Fallo Local:** En lugar de una IP, el log hará referencia a una terminal física o TTY o a un "display" local si es un entorno gráfico

### Reto de Investigación 2

Enviar y custodiar los logs de un servidor externo es una práctica fundamental ya que tiene varias ventajas como:

**1.- Preservación de la integridad:** si una máquina es vulnerada, el primer objetivo será borrar o modificar los archivos de /var/log para eliminar las huellas de la intrusión, al enviar los logs en tiempo real a un servidor externo, el registro de ataque queda fuera de peligro ya que está fuera del alcance del atacante.

**2.- Auditoría proactiva y centralizada:** la administración de sistemas exige escalar políticas de seguridad a miles de nodos, custodiar los logs de forma centralizada permite tener una visión global y heterogénea de toda la red en un solo punto. Además permite la detección de patrones utilizando herramientas como grep para identificar patrones de error.

### CITAS

“Auvik.” Auvik, https://www.auvik.com/franklyit/blog/centralized-logs/#:~:text=By%20centrally%20logging%20user%20activity,to%20selling%20managed%20network%20services.

“CuadernoSI 2025/2026.” Willman Acosta Lugo, https://notebooklm.google.com/notebook/876507f1-786e-44f3-ba78-112dc749934c?utm_source=classroom&utm_medium=referral&authuser=0.

“Varda.” Varda Security, https://vardasecurity.com/blog/the-importance-of-centralized-activity-logs-for-property-security-management/#:~:text=By%20aggregating%20this%20information%20into,documentation%20required%20for%20regulatory%20reviews.
