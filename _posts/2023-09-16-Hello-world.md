---
title: Pandora Writeup
author: xdann1
date: 2022-05-31
categories: [Writeup, HTB]
tags: [Linux, CTF, Easy, SNMP, Port Forwarding, SQLi, PATH Hijacking, CVE, CMS, SUID]
image:
  path: ../../assets/htb/pandora.png
  width: 800
  height: 500
  alt: Banner Pandora
---


Máquina Easy, encontramos un puerto abierto en UDP que corre una versión sin casi seguridad, lo enumeramos hasta encontrar unas credenciales válidas para ssh, encontramos una web oculta vulnerable a SQLi en la que conseguimos una reverse shell, por último, nos aprovechamos de un binario SUID junto con un PATH Hijacking para conseguir el root.

## Recopilación de Información

Primero vamos a comprobar la conectividad con la máquina.

```java
❯ ping -c 1 10.10.11.136
PING 10.10.11.136 (10.10.11.136) 56(84) bytes of data.
64 bytes from 10.10.11.136: icmp_seq=1 ttl=63 time=36.2 ms

--- 10.10.11.136 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 36.212/36.212/36.212/0.000 ms
```

En la salida del comando anterior se puede ver un parámetro llamado `ttl`, gracias a este parámetro podemos saber que sistema operativo está corriendo en la máquina víctima.
* GNU/Linux = TTL 64
* Windows = TTL 128

En este caso, el sistema operativo que está corriendo en la máquina víctima es GNU/Linux.

Vamos a usar la herramienta `nmap` para descubrir que puertos están abiertos y que servicios estan asociados a estos.
