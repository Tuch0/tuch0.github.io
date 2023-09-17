---
title: "CSTI (Client Site Template Inyection)"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [csti]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/csti.png
    width: 800
    height: 500
---


### ¿Qué es?

> CSTI atenta contra el navegador de el usuario para obtener datos "Privilegiados" aprovechando fallas en los motores de templates
{: .prompt-info }


### Ejemplo

> Para testear si es vulnerable , antes tenemos que hacer como en [[SSTI (Server-Side Template Inyection)]] -> {{7*7}}
{: .prompt-info }

![](../../assets/VectoresDeAtaque/CSTI-(Client-Site-Template-Inyection)/1.jpg)

![](../../assets/VectoresDeAtaque/CSTI-(Client-Site-Template-Inyection)/2.jpg)

> Como vemos hemos conseguido hacer un alert , de manera que ahora lo podemos derivar a un XS, cabe destacar que a veces no podemos introducir caracteres y hay que hacer alguna especie de "ByPass , para saltarlo tienes que hacer esto"
{: .prompt-info }

![](../../assets/VectoresDeAtaque/CSTI-(Client-Site-Template-Inyection)/3.jpg)

> **Los convertimos en decimal con python , y ahora lo ponemos en el payload**

![](../../assets/VectoresDeAtaque/CSTI-(Client-Site-Template-Inyection)/4.jpg)

![](../../assets/VectoresDeAtaque/CSTI-(Client-Site-Template-Inyection)/5.jpg)

![](../../assets/VectoresDeAtaque/CSTI-(Client-Site-Template-Inyection)/6.jpg)

