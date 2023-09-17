---
title: "LogPoisoning (LFI to RCE)"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [logPoioning, lfi, rce]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/log-poisoning.png
    width: 800
    height: 500
---


### Requisitos
- LFI -> Local File Inclusion
- Permisos de léctura en los "logs" de la web

### ¿Cómo funciona?

- **LogPoisoning** === **LFI -> RCE**

> Este tipo de vulnerabilidad se trata de "Envenenar" los logs de la web, de esta manera si nososotros a través de un LFI , podemo ver por ejemplo los logs de "Apache2" , si la web es php (por lo tanto interpreta código PHP) , lo que podemos tratar de hacer es enviar una peticíon a el servidor para que salga en los logs , pero que la petición tenga un comando en PHP , par que cuando lo queramos ver a través de el LFI se vea representado el código PHP introducido en el log
{: .prompt-info }

> Cabe destacar que hay que tener cuidado a la hora de envenar los LOGS ya que podria hacer inaccesible los logs , y no podriamos seguir con el ataque de "LogPoisoning"
{: .prompt-info }

### Ejemplos

#### Apache2

![](../../assets/VectoresDeAtaque/LogPoisoning-(LFI-to-RCE)/1.jpg)

> Ahí tenemos el acceso a los LOGS de apache a si que vamos a modificar una cabecera de nuestra petición para que incluya un código PHP y sea interpretado

![](../../assets/VectoresDeAtaque/LogPoisoning-(LFI-to-RCE)/2.jpg)

![](../../assets/VectoresDeAtaque/LogPoisoning-(LFI-to-RCE)/3.jpg)

> Cómo vemos hemos conseguido inyectar el comando que queramos a través de el envenenamiento de los logs
{: .prompt-info }

#### SSH

> La ruta de los LOGS en SSH se encuetra en **/var/log/btmp**

![](../../assets/VectoresDeAtaque/LogPoisoning-(LFI-to-RCE)/4.jpg)

> Ahora vamos a introducir la petición en PHP para ver si la interpreta la web

![](../../assets/VectoresDeAtaque/LogPoisoning-(LFI-to-RCE)/5.jpg)

![](../../assets/VectoresDeAtaque/LogPoisoning-(LFI-to-RCE)/6.jpg)

> Cómo vemos hemos conseguido ejecutar comandos a través de un LFI con los registros de BASH
{: .prompt-info }