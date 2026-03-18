> <img src="./ewkvfmgr.png"
> style="width:7.27667in;height:2.965in" />INFORME DE INCIDENTE DE
> SEGURIDAD
>
> <img src="./eac5zzng.png"
> style="width:7.26833in;height:4.545in" />Nicolás Oriol Sengáriz 4Geeks
>
> Índice
> Introducción.................................................................................................................................2
>
> Procesos en
> ejecución...............................................................................................................3
> Escaneo de
> rootkit......................................................................................................................5
> Identificación de
> cambios..........................................................................................................8
> Actualización de la
> seguridad.................................................................................................10
>
> Introducción
>
> El objetivo de este informe es llevar a cabo un análisis sobre un
> incidente de seguridad
>
> que ha habido en un servidor crítico de 4Geeks Academy.

<img src="./weyfd0t3.png"
style="width:4.82361in;height:3.18319in" />

> Procesos en ejecución
>
> Se realiza una investigación de los procesos en ejecución para ver si
> hay algún proceso sospechoso.
>
> Al ser una máquina debian eso se puede hacer desde System Tools – MATE
> System Monitor.
>
> <img src="./nnf543fz.png"
> style="width:5.27889in;height:4.06528in" />Esta ventana permite ver
> los procesos en ejecución y monitorear el estado del sistema, como el
> uso del CPU, RAM y la red de internet.

<img src="./phinkcvb.png"
style="width:5.8125in;height:2.2125in" /><img src="./ey1srje5.png"
style="width:5.80833in;height:2.18333in" /><img src="./cfzm3tjd.png"
style="width:5.90556in;height:0.28125in" />

> En el apartado de “Processes” aparecen los procesos en ejecución que
> son los de las
>
> imágenes de abajo.
>
> Después de realizar una investigación sobre los procesos en ejecución
> no se detecta
>
> ningún proceso malicioso o fuera de lo común.
>
> Todos los procesos pertenecen al usuario debian, con ID 1000.

<img src="./4v0omtzy.png"
style="width:3.83264in;height:0.17847in" /><img src="./1nteiftv.png" style="width:3.32in;height:0.40764in" /><img src="./yqq4f5ol.png"
style="width:3.35542in;height:0.38194in" />

> Escaneo de rootkit
>
> Para realizar un escaneo para buscar rootkits se usará la herramienta
> Rkhunter.
>
> Para usarlo hay que instalar la herramienta con el comando sudo apt
> install rkhunter.
>
> Antes de escanear interesa actualizar la base de datos, especialmente
> si hace tiempo
>
> que tienes instalada la herramienta Rkhunter.
>
> Se actualiza con los siguientes comandos:
>
> sudo rkhunter –update
>
> sudo rkhunter –propupd
>
> Una vez ya está actualizada la base de datos, se procede a realizar el
> escaneo de
>
> <img src="./q5st20zq.png"
> style="width:3.795in;height:3.32292in" /><img src="./zlv35omq.png"
> style="width:3.11555in;height:0.4243in" />rootkits usando el comando
> sudo rkhunter –check.

<img src="./fzrwva43.png"
style="width:3.62625in;height:3.38958in" /><img src="./22hcft2h.png"
style="width:3.62569in;height:3.32431in" />

> En las imágenes anteriores se puede apreciar que la herramienta
> Rkhunter no ha
>
> encontrado ningún rootkit habiendo escaneado 75 rootkits.

<img src="./x32ep10s.png"
style="width:4.23083in;height:2.37361in" /><img src="./m1irofz3.png"
style="width:5.90556in;height:0.55903in" /><img src="./glkotr3s.png"
style="width:4.84167in;height:2.26389in" />

> Lo que ha encontrado han sido segmentos grandes de memoria compartida
> que son los
>
> tres procesos de la foto de abajo, así que no es peligroso.
>
> Rkhunter tiene una configuración interna antigua que hace que
> considere cualquier segmento de memoria compartida mayor a 1 MB
> sospechoso, porque antiguamente los rootkits usaban estos espacios
> para esconderse.
>
> Sin embargo, las aplicaciones gráficas modernas necesitan más de 1 MB
> para funcionar
>
> correctamente.
>
> Los tres posibles rootkits que ha detectado son los tres procesos que
> consumen más
>
> memoria de la que deberían según los antiguos parámetros Rkhunter que
> todavía usa.
>
> Así que no se ha detectado ningún rootkit.

<img src="./dcblga4e.png" style="width:4.82in;height:3.49653in" /><img src="./g3cbdynz.png"
style="width:4.84542in;height:1.64097in" />

> Identificación de cambios
>
> Se busca si hay nuevos usuarios creados por el atacante.
>
> Para saber todos los usuarios creados en este sistema se usa el
> comando cat
>
> /etc/passwd que nos lista todos los usuarios del sistema.
>
> Se revisan todos los nombres de usuario y no se detecta ningún usuario
> fuera de lo
>
> normal, todos son legítimos.
>
> Hay dos tipos de usuarios:
>
> • Los usuarios que tienen permiso para acceder como personas, solo hay
> dos usuarios que son root y debian que tienen este permiso, esto se
> sabe porque acaban en **/bin/bash**.
>
> • Los usuarios que **no** tienen permiso para acceder como personas,
> que son la mayoría de usuarios, esto se sabe porque acaban en
> **/bin/false** o **/sbin/nologin**. A estos usuarios no los pueden
> suplantar los ciberdelincuentes.

<img src="./kk3mlhfh.png"
style="width:2.77708in;height:1.2331in" /><img src="./wqmrh2vh.png"
style="width:5.90556in;height:1.46181in" />

> Se buscan backdoors creadas por el ciberatacante de la siguiente
> manera:
>
> ▪ No se detectan conexiones SSH a través del usuario debian o root.
>
> ▪ Se usa el comando crontab -l para buscar tareas programadas y no se
> encuentra
>
> ninguna para el usuario debian y usuario root.
>
> Por lo tanto, no se detectan backdoors.
>
> La herramienta Rkhunter ha detectado que está permitido el acceso root
> vía SSH
>
> Al estar el puerto abierto de SSH no hace falta crear una backdoor ya
> que tienes una forma de acceso conocida, se podría considerar el
> acceso root vía SSH como la backdoor ya que hace esa función sin haber
> sido creada maliciosamente para esa función.

<img src="./dwdazidd.png"
style="width:2.35292in;height:0.17569in" /><img src="./h5r4uxyk.png"
style="width:2.76653in;height:0.21667in" />

> Actualización de la seguridad
>
> Se usan los comandos:
>
> Sudo apt update
>
> Sudo apt upgrade
>
> Y así nos aseguramos que los programas están actualizados y el
> sistema, en este caso
>
> Debian, esté al día con las últimas correcciones de seguridad y
> funcionalidades,
>
> manteniendo el sistema estable y seguro.
>
> Se debe cerrar el puerto innecesario 21.
>
> En el puerto 21 se usa el servicio **FTP** para la transferencia de
> archivos.
>
> ▪ Este protocolo es antiguo y poco seguro ya que envía las contraseñas
> en texto
>
> plano, sin cifrar.
>
> ▪ Tiene una versión más segura que es el **SFTP.**
>
> ▪ Por el puerto 22 también se puede realizar un intercambio de
> archivos, en este
>
> caso sería de forma segura mediante **SFTP**.
>
> El puerto se cierra con el comando en el firewall UFW sudo ufw deny
> 21/tcp y en el
>
> firewall iptables sería el comando sudo iptables -A INPUT -p tcp
> --dport 21 -j DROP.
>
> En el puerto 22 hay que realizar unos ajustes para securizarlo.
>
> En el puerto 22 se usa **SSH** para el acceso remoto seguro.
>
> Se debe quitar el acceso root vía SSH
>
> Quitar el acceso root vía SSH se realiza de la siguiente manera:
>
> <img src="./adnnh5m5.png"
> style="width:4.13778in;height:0.21528in" />Se usa el comando sudo nano
> /etc/ssh/sshd_config.

<img src="./ry5jty13.png"
style="width:2.48472in;height:2.47708in" /><img src="./fdkrfuh5.png"
style="width:2.64417in;height:2.47778in" />

> Dentro del archivo sshd_config hay que buscar la línea
> PermitRootLogin, como está
>
> permitido el acceso root pondrá **yes**, hay que cambiar el **yes**
> por un **no**, de esta manera
>
> no se permite el acceso root vía SSH.
>
> Se guarda los cambios en el archivo con Ctrl +O e Intro y se sale del
> archivo con Ctrl+X.
>
> Se debe reiniciar el servicio de SSH con el siguiente comando: sudo
> systemctl restart
>
> <img src="./t3dzj1zq.png"
> style="width:3.49472in;height:0.17917in" />ssh
