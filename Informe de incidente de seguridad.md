INFORME DE INCIDENTE DE SEGURIDAD

Índice
Introducción
Procesos en ejecución
Escaneo de rootkit
Identificación de cambios
Actualización de la seguridad

Introducción
El objetivo de este informe es llevar a cabo un análisis sobre un incidente de seguridad
que ha habido en un servidor crítico de 4Geeks Academy.

Procesos en ejecución
Se realiza una investigación de los procesos en ejecución para ver si hay algún
proceso sospechoso.

Al ser una máquina debian eso se puede hacer desde System Tools – MATE System
Monitor.

Esta ventana permite ver los procesos en ejecución y monitorear el estado del
sistema, como el uso del CPU, RAM y la red de internet.

En el apartado de “Processes” aparecen los procesos en ejecución que son los de las
imágenes de abajo.

Después de realizar una investigación sobre los procesos en ejecución no se detecta
ningún proceso malicioso o fuera de lo común.

Todos los procesos pertenecen al usuario debian, con ID 1000.

Escaneo de rootkit
Para realizar un escaneo para buscar rootkits se usará la herramienta Rkhunter.

Para usarlo hay que instalar la herramienta con el comando sudo apt install rkhunter.

Antes de escanear interesa actualizar la base de datos, especialmente si hace tiempo
que tienes instalada la herramienta Rkhunter.

Se actualiza con los siguientes comandos:

sudo rkhunter –update

sudo rkhunter –propupd

Una vez ya está actualizada la base de datos, se procede a realizar el escaneo de
rootkits usando el comando sudo rkhunter –check.

En las imágenes anteriores se puede apreciar que la herramienta Rkhunter no ha
encontrado ningún rootkit habiendo escaneado 75 rootkits.

Lo que ha encontrado han sido segmentos grandes de memoria compartida que son los
tres procesos de la foto de abajo, así que no es peligroso.

Rkhunter tiene una configuración interna antigua que hace que considere cualquier
segmento de memoria compartida mayor a 1 MB sospechoso, porque antiguamente los
rootkits usaban estos espacios para esconderse.

Sin embargo, las aplicaciones gráficas modernas necesitan más de 1 MB para funcionar
correctamente.

Los tres posibles rootkits que ha detectado son los tres procesos que consumen más
memoria de la que deberían según los antiguos parámetros Rkhunter que todavía usa.

Así que no se ha detectado ningún rootkit.

Identificación de cambios
Se busca si hay nuevos usuarios creados por el atacante.

Para saber todos los usuarios creados en este sistema se usa el comando cat
/etc/passwd que nos lista todos los usuarios del sistema.

Se revisan todos los nombres de usuario y no se detecta ningún usuario fuera de lo
normal, todos son legítimos.

Hay dos tipos de usuarios:

Los usuarios que tienen permiso para acceder como personas, solo hay dos
usuarios que son root y debian que tienen este permiso, esto se sabe porque
acaban en /bin/bash.
Los usuarios que no tienen permiso para acceder como personas, que son la
mayoría de usuarios, esto se sabe porque acaban en /bin/false o /sbin/nologin.
A estos usuarios no los pueden suplantar los ciberdelincuentes.
Se buscan backdoors creadas por el ciberatacante de la siguiente manera:

▪ No se detectan conexiones SSH a través del usuario debian o root.
▪ Se usa el comando crontab -l para buscar tareas programadas y no se encuentra
ninguna para el usuario debian y usuario root.
Por lo tanto, no se detectan backdoors.

La herramienta Rkhunter ha detectado que está permitido el acceso root vía SSH

Al estar el puerto abierto de SSH no hace falta crear una backdoor ya que tienes una
forma de acceso conocida, se podría considerar el acceso root vía SSH como la
backdoor ya que hace esa función sin haber sido creada maliciosamente para esa
función.

Actualización de la seguridad
Se usan los comandos:

Sudo apt update
Sudo apt upgrade
Y así nos aseguramos que los programas están actualizados y el sistema, en este caso
Debian, esté al día con las últimas correcciones de seguridad y funcionalidades,
manteniendo el sistema estable y seguro.

Se debe cerrar el puerto innecesario 21.

En el puerto 21 se usa el servicio FTP para la transferencia de archivos.

▪ Este protocolo es antiguo y poco seguro ya que envía las contraseñas en texto
plano, sin cifrar.
▪ Tiene una versión más segura que es el SFTP.
▪ Por el puerto 22 también se puede realizar un intercambio de archivos, en este
caso sería de forma segura mediante SFTP.
El puerto se cierra con el comando en el firewall UFW sudo ufw deny 21/tcp y en el
firewall iptables sería el comando sudo iptables -A INPUT -p tcp --dport 21 -j DROP.

En el puerto 22 hay que realizar unos ajustes para securizarlo.

En el puerto 22 se usa SSH para el acceso remoto seguro.

Se debe quitar el acceso root vía SSH

Quitar el acceso root vía SSH se realiza de la siguiente manera:

Se usa el comando sudo nano /etc/ssh/sshd_config.

Dentro del archivo sshd_config hay que buscar la línea PermitRootLogin, como está
permitido el acceso root pondrá yes , hay que cambiar el yes por un no , de esta manera
no se permite el acceso root vía SSH.

Se guarda los cambios en el archivo con Ctrl + O e Intro y se sale del archivo con Ctrl+X.

Se debe reiniciar el servicio de SSH con el siguiente comando: sudo systemctl restart
ssh
