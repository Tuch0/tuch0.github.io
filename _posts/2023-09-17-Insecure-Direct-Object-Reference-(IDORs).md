---
title: "Insecure Direct Object Reference (IDORs)"
author: Tuch0
date: 2023-09-17
categories: 
  - Vectores-de-Ataque
tags: 
  - IDORs 
  - Wfuzz
image:
  path: ../../assets/Miniaturas/VectoresDeAtaque/graphL.png
  width: 800
  height: 500
---


#### Desplegamos el laboratorio

```bash
# Descargamos la ISO de vulhub
https://www.vulnhub.com/entry/xtreme-vulnerable-web-application-xvwa-1,209/

# La importamos en VMware

# Configuramos el adaptador red -> "Bridged"
```

#### Desplegamos el laboratorio

```bash
# Clonamos el repositorio
git clone https://github.com/balbla1337/skf-labs

# Entramos en el repositorio
cd skf-labs/nodeJs/IDOR

# Iniciamos el servidor NodeJS
npm install 
npm start
```

### ¿Que és?

> Las **Insecure Direct Object References** (**IDOR**) son un tipo de vulnerabilidad de seguridad que se produce cuando una aplicación web utiliza **identificadores internos** (como números o nombres) para identificar y acceder a recursos (como archivos o datos) y no se valida adecuadamente la autorización del usuario para acceder a ellos.
{: .prompt-info }

> Por ejemplo, si una aplicación web utiliza un identificador numérico para identificar un registro en una base de datos, un atacante puede intentar adivinar este identificador y acceder a los registros sin la debida autorización. Esto puede permitir a los atacantes acceder a información confidencial, modificar datos, crear cuentas de usuario no autorizadas y realizar otras acciones maliciosas.
{: .prompt-info }

### Escenario

![](../../assets/VectoresDeAtaque/Insecure-Direct-Object-Reference-(IDORs)/1.jpg)

![](../../assets/VectoresDeAtaque/Insecure-Direct-Object-Reference-(IDORs)/2.jpg)

![](../../assets/VectoresDeAtaque/Insecure-Direct-Object-Reference-(IDORs)/3.jpg)

> Como vemos a través de un parámetro en la URL , se esta enviando un ID , a nosotros desde la web , **solo nos deja hasta el número 5 , a si que vamos a probar con otros números**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Insecure-Direct-Object-Reference-(IDORs)/4.jpg)

![](../../assets/VectoresDeAtaque/Insecure-Direct-Object-Reference-(IDORs)/5.jpg)

> Como vémos gracias a cambiar el parámetro de la web , se consiguen mostrar informácion de otros items
{: .prompt-info }

### Segundo Escenario

![](../../assets/VectoresDeAtaque/Insecure-Direct-Object-Reference-(IDORs)/6.jpg)

> En el campo mensaje si nosotros ponemos texto , nos da el ID de el PDF creado y en el otro campo sirve para poner el ID y ver el contenido , a si que vamos a tratar de fuzzear , y averiguar todos los PDF existentes
{: .prompt-info }

```bash
wfuzz -c --hl=101,104 -z range,1-1500 -d 'pdf_id=FUZZ' http://localhost:5000/download
```


![](../../assets/VectoresDeAtaque/Insecure-Direct-Object-Reference-(IDORs)/7.jpg)

