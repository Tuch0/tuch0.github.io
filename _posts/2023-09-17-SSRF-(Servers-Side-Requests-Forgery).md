---
title: "SSRF (Servers-Side Requests Forgery)"
author: Tuch0
date: 2023-09-17
categories: 
  - Vectores-de-Ataque
tags: 
  - SSRF
  - ADCCFFA6
image:
  path: ../../assets/Miniaturas/VectoresDeAtaque/ssrf.jpg
  width: 800
  height: 500
---


# Índice
- SSRF Explicación
- SSRF Basic
- SSRF another back-end System
- SSRF with blacklist-based input filter
- SSRF with filter bypass via open redirection vulnrability
- SSRF Blind with out-of-ban detection (OOB)
- SSRF with whitelist-based input filter
- SSRF with ShellSock explotation

## Requisitos

> Este ataque se suele usar , cuando en una web hay un formulario que te permite poner una URL y te dice si esta activa o no , en este caso nos podemos aprovechar de eso para vulnerar la web
{: .prompt-info }

## SSRF Explicación

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/1.jpg)

## SSRF Básic

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/2.jpg)

> Vamos a interceptar la petición con BurpSuite
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/3.jpg)

> Cómo vemos lo que esta haciendo es hacerse una petición a si misma de manera que esta buscando un producto a si que vamos a intentar ver puertos abiertos en la máquina vícitma
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/4.jpg)

> Ahí como vemos nos hemos conseguido meter en el "admin panel"
{: .prompt-info }

## SSRF another back-end System

> De lo que se trata este ataque es enumerar puertos abiertos de otras máquinas existentes a nivel de red
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/5.jpg)

> Interceptamos la petición:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/6.jpg)

> Cómo no sabemos cual es la IP lo que vamos a ahacer es enviarlo a el "Intruder":
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/7.jpg)

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/8.jpg)

> Iniciamos el ataque:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/9.jpg)

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/10.jpg)

## SSRF with blacklist-based input filter

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/11.jpg)

> Como vemos nos lo a bloqueado a si que vamos a intentar bypassearlo
{: .prompt-info }

### Maneras de bypass

- localhost
- localhost -> (Hexadecimal)
- 127.1.-> (Porque hay 0 consicutivos)

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/12.jpg)

> Hemos conseguido bypassearlo , pero al poner el recurso "admin" tambien nos da error a si que vamos a intentar saltar este doble "Check"
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/13.jpg)

> Como vemos hemos URL-encodeado de la palabra "admin" el caracter "a" y hemos conseguido bypassear el "Check"
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/14.jpg)

## SSRF with filter bypass via open redirection vulnerability

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/15.jpg)

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/16.jpg)

> Vamos a capturar la petición con BurpSuite
{: .prompt-info }


![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/17.jpg)

> como vemos usa el parámetro "PATH" para buscar si el producto existe a si que vamos a aprovecharnos de eso y indicamos ahí una URL de manera que si funciona va a ingresar a la url que nosotros le pongamos
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/18.jpg)

> Primero vamos a mirar si funciona en la pripia URL
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/19.jpg)

> Como vemos nos a redirigido a "google.es" a si que nos podemos aprovechar de esto para enumerar servicios o puertos que estan corriendo el la máquina víctima
{: .prompt-info }

> Ahora volvemos a capturar la petición de antes:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/20.jpg)

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/21.jpg)

> Lo que hemos hecho es aprovecharnos de el redirect que tiene la propia web que usa un parámetro que esta en la ruta "/product/stock" de manera que como esta peción es de esta ruta podemo usar el parámetro para ingresar a la dirección de la máquina víctima que queramos
{: .prompt-info }

## SSRF Blind with out-of-ban detection (OOB)

> Aqui lo que se esta usando es el "Referered" , esto por ejemplo lo usa YouTube , por ejemplo si yo subo un twit y pongo un enlace de youtube en la peticion , en el apartado de "Refered" pone que vengo de Twitter , de manera que asi de cara a analiticas y estadísticas de YouTube, pueden saber cuantas personas vienen de Twitter , de Facebook , etc
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/22.jpg)

## SSRF with whitelis-based input filter

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/23.jpg)

> Como vemos nos esta obligando a que tiene quke tener "stock.weliketoshop.net" en la petición
{: .prompt-info }

> Si nos paramos a pensar en la propia URL lo que podemos hacer es si sale una ventana emergente con usuario y contraseña nos podemos llegar a logear desde la URL , de manera que vamos a probar este vector de ataque
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/24.jpg)

> Como vemos nos a dejado a dejado autenticarnos en el hipotético caso de que tengamos credenciales
{: .prompt-info }

> Pero por aqui no van los tiros , recordamos que la web nos obliga a que contemple el dominion que nos a indicado en nuestra URL a si que vamos a aprovecharnos de como funionan las URL
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/25.jpg)

> Si nos fijamos por ejempo en wikipedia , cuando hay como "Titulares" nosotros podemos acceder desde la URL fijaos:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/26.jpg)

> Como vemos si ponemos # y a continuación el titular nos lleva directamente a el titular a si que vamos a intentar usar este concepto para añadir el domino que nos obliga a contemplar en nuestra URL
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/27.jpg)

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/28.jpg)

> Como vemos nos hemos aprovechado de el "#" para añadir lo que nos obliga
{: .prompt-info }

> Ahora vamos a ingresar directamente a el "Admin Panel"
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/29.jpg)

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/30.jpg)

## SSRF with ShellSock explotation

> Caputamos la petición normal
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/31.jpg)

> La propia web de ejercicios nos dice que es vulnerable a "ShellSock" a si que lo que vamos a hacer es aprovecharnos de una cabecerad de la petición en est caso "User-Agent" y desde ahi vamos a diseñarla para que nos permita ejecutar el comando que nosotros queramos
{: .prompt-info }

```shell
curl -H "User-Agent: () { :; }; /bin/eject" http.//example.com/
```

> Vamos a usar este ejemplo y para aprovecharnos y ejectuar el comando que nosotros queramos
{: .prompt-info }

> Vamos a usar este comando:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/32.jpg)

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/33.jpg)

> Lo pasamos por el intruder:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/34.jpg)

![](../../assets/VectoresDeAtaque/SSRF-(Servers-Side-Requests-Forgery)/35.jpg)

> Cómo vemos hemos obtenido el nombre de usuario de el servidor víctima
{: .prompt-info }

> Tambien podemos usarlo para obtener una "reverseSHELL"
{: .prompt-info }

