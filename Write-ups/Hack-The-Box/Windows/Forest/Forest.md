# Enumeración | Reconocimiento | y posibles ataques

Envío paquetes de prueba ICMP (Internet Control Message Protocol) hacia el equipo para verificar conectividad y ver el TTL

![Texto alternativo](imgs/1.jpg)

TTL = 127
Indica que es un equipo Windows
Escaneo de puertos TCP abiertos
nmap -p- -sS --min-rate 5000 -vv -n 10.10.10.161 -oG PortStatus

![Texto alternativo](imgs/2.jpg)

Puertos abiertos
* 445
* 135
* 53
* 139
* 389
* 5985
* 47001
* 49706
* 636
* 3268
* 49677
* 49684
* 464
* 88
* 49664
* 9389
* 49670
* 49919
* 593
* 49676
* 49666
* 49667
* 3269
* 49665

Escaneo de servicios en puertos abiertos 

nmap -p445,135,53,139,389,5985,47001,49706,636,3268,49677,49684,464,88,49664,9389,49670,49919,593,49676,49666,49667,3269,49665 -sCV 10.10.10.161 -oN services

![Texto alternativo](imgs/3.jpg)

![Texto alternativo](imgs/4.jpg)

![Texto alternativo](imgs/34.jpg)


* 64bits
* Name: Forest
* Windows Server 2016 Standar 14393
* Dominio: htb.local
* SMB firmado
* SMBv1

Enumero recursos compartidos en la red sin credenciales
smbclient -L 10.10.10.161 -N

![Texto alternativo](imgs/5.jpg)

No me enumera nada sin credenciales

Envío solicitudes DNS para detectar si es vulnerable al ataque de Domain Zone 	Transfer (AXFR)

Envío solicitud DNS a 10.10.10.161 indicando el dominio htb.local 

dig @10.10.10.161 htb.local

![Texto alternativo](imgs/6.jpg)

Enumero servidores de correo
dig @10.10.10.161 htb.local mx
![Texto alternativo](imgs/7.jpg)

Enumero Name Servers

dig @10.10.10.161 htb.local ns

![Texto alternativo](imgs/8.jpg)

AXFR pero no me detalla nada

![Texto alternativo](imgs/9.jpg)

Como es un DC uso RCPCLIENT para intentar enumerar usuarios
rpcclient -U “” 10.10.10.161 -N

![Texto alternativo](imgs/10.jpg)

Creo archivo users.txt y filtro solo para quedarme con los usuarios del dominio

rpcclient -U "" 10.10.10.161 -N -c 'enumdomusers' | grep -oP '\[.*?\]' | grep -v 0x | tr -d '[]' > users.txt

Listo la información de los usuarios

![Texto alternativo](imgs/11.jpg)








# Explotación y movimiento lateral







# Explotación WEB






# Escalada de privilegios





# Links útiles


  
  
  
## Lenguajes utilizados
## Vulnerabilidades en aplicaciones
## Escalada de privilegios
## Exploits
## Herramientas
