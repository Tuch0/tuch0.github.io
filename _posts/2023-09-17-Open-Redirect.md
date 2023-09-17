---
title: "Open Redirect"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [OpenRedirect]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/open-redirect.png
    width: 800
    height: 500
---


#### Desplegamos el laboratorio

```bash
# Clonamos el repositorio
git clone https://github.com/labla1337/skf-labs

# Entramos en el repositorio
cd skf-labs/nodeJs/Url-redirection

# Iniciamos el serivor
npm install
npm start
```

#### Desplegamos el laboratorio 2

```bash
# Clonamos el repositorio
git clone https://github.com/labla1337/skf-labs

# Entramos en el repositorio
cd skf-labs/nodeJs/Url-redirection-harder

# Iniciamos el serivor
npm install
npm start
```

#### Desplegamos el laboratorio 3

```bash
# Clonamos el repositorio
git clone https://github.com/labla1337/skf-labs

# Entramos en el repositorio
cd skf-labs/nodeJs/Url-redirection-harder2

# Iniciamos el serivor
npm install
npm start
```


### ¿Que és?

> La vulnerabilidad de redirección abierta , también conocida como **Open Redirect** es una vulnerabilidad común en aplicaciones web que puede ser explotada por **los atacantes para dirigir a los usuarios a sitios web maliciosos.**
{: .prompt-info }

> Por ejemplo, **supongamos que una aplicación web utiliza un parámetro de redireccionamiento en una URL para dirigir al usuario a una página externa después de que se haya autenticado.** _Si esta URL no valida adecuadamente el parámetro de redireccionamiento y permite a los atacantes modificarlo_, los atacantes pueden dirigir al usuario a un sitio web malicioso, en lugar del sitio web legítimo.
{: .prompt-info }

### Escenario

![](../../assets/VectoresDeAtaque/Open-Redirect/1.jpg)

> Tenemos esta web , que al pulsar el boton "Go to new website" nos redirige a "localhost:5000/newsite" , a si que **vamos a intercerptar esto con burpsuite**
{: .prompt-info }

### Práctica

![](../../assets/VectoresDeAtaque/Open-Redirect/2.jpg)

![](../../assets/VectoresDeAtaque/Open-Redirect/3.jpg)

![](../../assets/VectoresDeAtaque/Open-Redirect/4.jpg)

> Como vemos esta es la vulnerabilidad , es bastante sencilla pero muy útil
{: .prompt-info }

### Práctica 2

![](../../assets/VectoresDeAtaque/Open-Redirect/5.jpg)

![](../../assets/VectoresDeAtaque/Open-Redirect/6.jpg)

![](../../assets/VectoresDeAtaque/Open-Redirect/7.jpg)

> Lo que hemos hecho aqui es "URL-codearlo" 2 veces , para que no detecte el punto , porque **si lo "URL-codeas" una vez la propia web va a interpretar que e sun punto pero si lo haces 2 veces no porque la web solo "URL-Decodea" 1 vez**, es importante que lo veas a través de la web no desde burp , porque si no no te va a interpretar el "."
{: .prompt-info }




### Práctica 3

![](../../assets/VectoresDeAtaque/Open-Redirect/8.jpg)

> Aqui pensarias que es la misma idea peroo NOO. aqui lo que tienes que hacer es url-encodear el . 2 veces como antes , y **con la "/" al ser "https" , no es necesario poner las dobles barras , pero SOLO en el caso de que sea "HTTPS"** 
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Open-Redirect/9.jpg)

