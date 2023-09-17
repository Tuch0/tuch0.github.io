---
title: "Enumeración y explotación de WebDAV"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [webDav]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/webDav.png
    width: 800
    height: 500
---


#### Desplegamos el laboratorio

```bash
# Descargamos la imagen de docker
docker pull bytemark/webdav

# Desplegamos el contenedor
docker run --restart always -v /srv/dav:/var/lib/dav -e AUTH_TYPE=Digest -e USERNAME=admin -e PASSWORD=richard --publish 80:80 -d bytemark/webdav
```

### ¿Que és?

> **WebDAV (Web Distributed Authoring and Versioning)** es un protocolo que nos permite guardar archivos, editarlos, moverlos y compartirlos en un servidor web.
{: .prompt-info }

> Cuando hablamos de enumerar un servidor **WebDAV**, a lo que nos referimos es al proceso de **recopilar información sobre los recursos disponibles en el servidor WebDAV**. Los atacantes pueden utilizar herramientas de enumeración de WebDAV para buscar **recursos protegidos en el servidor, como archivos de configuración, contraseñas y otros datos confidenciales.** Los atacantes pueden utilizar la información recopilada durante la enumeración para **planificar ataques más sofisticados contra el servidor.**
{: .prompt-info }

### Escenario

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-WebDAV/1.jpg)

### Práctica

> Como vemos no podemos entar en la web porque esta pidiendo credenciales un **web dav** , **cabe destacar que hay herramientas que nos permiten enumerar los archivo que soporta y podemos subir a la web automáticamente llamada "davtest", pero primero vamos a tratar de obtener las credenciales**
{: .prompt-info }

> Vamos a hacer un **ONE LINER** que lo que haga sea probar con el diccionario de rockyou , para ver si las credenciales son correctas
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-WebDAV/2.jpg)

> Como vemos hemos conseguido explotar un "WebDav" y **hemos obtenido la credencial por fuerza bruta**
{: .prompt-info }

> Ahora vamos a validar los archivos que soporta la web con una herramienta llamada **"cadaver"**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Enumeración-y-explotación-de-WebDAV/3.jpg)

