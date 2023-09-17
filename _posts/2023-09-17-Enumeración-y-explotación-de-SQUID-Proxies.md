---
title: "Enumeración y explotación de SQUID Proxies"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [squid-proxies]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/squid-proxyk.png
    width: 800
    height: 500
---


#### Desplegamos el laboratorio

```bash
# Descargamos el comprimido
https://www.vulnhub.com/entry/sickos-11,132/

# Importamos la ova 
# 1. VMware
# 2. VirtualBox
```

### ¿Que és?

> El **Squid Proxy** es un servidor web **proxy-caché** con licencia **GPL** cuyo objetivo es funcionar como **proxy** de la red y también como zona caché para almacenar páginas web, entre otros. Se trata de un servidor situado entre la máquina del usuario y otra red (a menudo Internet) que actúa como protección separando las os redes y como zona caché para acelerar el acceso a páginas web o poder restringir el acceso a contenidos
{: .prompt-info }

> Es decir, la función de un servidor proxy es centralizar el tráfico de una red local  hacia el exterior (Internet). Sólo el equipo que incorpora el servicio proxy ebe disponer de conexión a internet y el resto de equipos salen a través de él:

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-SQUID-Proxies/1.jpg)

Ahora bien, puede darse el caso en el que un servidor Squid Proxy se encuentre **mal configurado**, permitiendo en consecuencia a los atacantes recopilar información de dispositivos a los que normalmente no deberían tener acceso.
{: .prompt-info }

> Por ejemplo, en este tipo de situaciones, un atacante podría ser capaz de realizar peticiones a direcciones IP internas pasando sus consultas a través del Squid Proxy, pudiendo así realizar un escaneo de puertos contra determinados servidores situados en una **red interna**.
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-SQUID-Proxies/2.jpg)

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-SQUID-Proxies/3.jpg)

> Como vemos usamos el comando curl , o intentamos acceder vía web , no vemos nada por el puerto 80, pero al usar squid proxy es posible que si que haya contenido en el puerto 80 pero no tengamos acceso a si que vamos a aprovecharnos de el "squid proxy" , lo vamos a usar nosotros con el comando **"curl"**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-SQUID-Proxies/4.jpg)

> Como vemos el proxy , esta mal configurado y no tiene credenciales por lo tanto lo podemo usar cuando queramos, ahora podemos configurar el proxy en varios sitios para hacer un escaneo:
{: .prompt-info }

#### FoxyProxy

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-SQUID-Proxies/5.jpg)

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-SQUID-Proxies/6.jpg)

#### Gobuster

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-SQUID-Proxies/7.jpg)


#### Ports Discovery Script Python

```python
#!/usr/bin/pytho3


# Importamos las librerias
import sys, signal, requests


# Funciones
def def_handler(sig, fram):
    print("\n\n[!] Saliendo... \n")
    sys.exit(1)


# Ctrl+C
signal.signal(signal.SIGINT, def_handler)


# Variable generales
main_url = "http://127.0.0.1"
squid_proxy = {'http':"http://192.168.0.116:3128"}


# Funciones
def portDiscovery():

    # Diccionario de puertos más comunes
    puertos_comunes = {20, 21, 22, 23, 25, 53, 80, 110, 119, 123, 143, 161, 194, 443, 465, 587, 993, 995, 1080,1433, 1521, 1723, 3306, 3389, 3690, 4333, 5060, 5432, 5800, 5900, 5985, 6379, 7001, 8000, 8080,8443, 8888, 9100, 9200, 9418, 9999, 10000, 11211, 27019, 28017, 50000, 50070, 50075}
    
    # Iteramos en cada puerto
    for tcp_port in puertos_comunes:
        
        # Trámitamos la petición
        r = requests.get(main_url + ':' + str(tcp_port), proxies=squid_proxy)

        # Validamos si es correcto
        if r.status_code != 503:
            
            # Mostramos por pantalla
            print("\n[+] Port " + str(tcp_port) + " - OPEN")



# Definimos flujo de el programa
if __name__ == "__main__":

    # Llamamos a la funciòn principal
    portDiscovery()
```

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-SQUID-Proxies/8.jpg)

> Cómo vemos hemos conseguido saber los puertos abiertos a través de el squid proxy

Herramienta para portDiscovery: `http://github.com/aancw/spose`
{: .prompt-info }

> **Cabe destacar que a veces tienes que autenticarte en el squid proxy , la forma más facil es hacerlo desde la URL**: `http://admin:password@192.168.0.116:3128`
{: .prompt-info }
