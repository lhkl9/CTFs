


# Enumeración | Reconocimiento | y posibles ataques

Envío paquetes de prueba ICMP (Internet Control Message Protocol) hacia el equipo para verificar conectividad y ver el TTL

ping 10.10.10.198

→
64 bytes from 10.10.10.198: icmp_seq=350 ttl=127 time=236 ms
TTL = 127
Indica que es un equipo Windows
Escaneo de puertos TCP abiertos
nmap -p- -sS --min-rate 5000 -vv -n 10.10.10.203 -oG PortStatus

→
PORT 	STATE SERVICE	REASON
7680/tcp open  pando-pub  syn-ack ttl 127
8080/tcp open  http-proxy syn-ack ttl 127

Puertos abiertos:

*7680
*8080

nmap -p8080,7680 -sCV 10.10.10.198

→ 
PORT 	STATE SERVICE	VERSION
7680/tcp open  pando-pub?
8080/tcp open  http   	Apache httpd 2.4.43 ((Win64) OpenSSL/1.1.1g PHP/7.4.6)
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-server-header: Apache/2.4.43 (Win64) OpenSSL/1.1.1g PHP/7.4.6
|_http-title: mrb3n's Bro Hut

whatweb http://10.10.10.198:8080









# Explotación y movimiento lateral







# Explotación WEB






# Escalada de privilegios





# Links útiles


  
  
  
## Lenguajes utilizados
## Vulnerabilidades en aplicaciones
## Ataques
## Escalada de privilegios
## Exploits
## Herramientas
