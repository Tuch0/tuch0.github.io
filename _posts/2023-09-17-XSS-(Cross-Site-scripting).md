---
title: "XSS (Cross-Site scripting)"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [xss]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/XSS.jpg
    width: 800
    height: 500
---


> Cross-site scripting (también conocido como XSS) es una vulnerabilidad de seguridad web que permite a un atacante comprometer las interacciones que los usuarios tienen con una aplicación vulnerable.
{: .prompt-info }

### ¿Como funciona? 

> El "Cross-Site-Scripting" funciona mediante la manipulación de un sitio web vulnerable para que devuelva JavaScript malicioso a los usuarios.
{: .prompt-info }

## XSS Proof of concept
```js
<script>alert("XSS")</script>
```
> Esto es una carga útil que se utiliza como primera traza pra ver si es vulerable a XSS
{: .prompt-info }

## Tipos de ataque
- Reflected XSS
- Stored XSS
- DOM-Based XSS

## Reflected XSS

> Es la variedad más simple de XSS , surge cuando una aplicación recibe datos en una solicitud HTTP y los incluye dentro de la respuesta inmediata de forma no segura
{: .prompt-info }

```js
// URL
https://insecure-website.com/status?message=All+is+well

// Web Content
<p>Status: All is well.</p>
```
> La web no realiza ningun tipo de procesamiento de los datos , por lo que un atacante puede aprovecharse para construir una taque:
{: .prompt-info }

```js
// URL
https://insecure-website.com/status?messaage=<script>/*+Bad+stuff+here...+*/</script>

// Web Content
<p>Status: <script>/* Bad stuff here... */</script></p>
```

![](../../assets/VectoresDeAtaque/XSS-(Cross-Site-scripting)/1.jpg)

![](../../assets/VectoresDeAtaque/XSS-(Cross-Site-scripting)/2.jpg)

## Stored XSS

> Surge cuando una aplicación recibe datos de una fuente que no es de confianza e incluye esos datos en sus respuestas HTTP posteriores de forma no segura. (comentarios en una publicación de blog)
{: .prompt-info }

```js
// Comentario
<p>Hola este es mi comentario!</p>

// Nos aprovechamos de el comentario
<p><script>/* Script malicioso...*/</script></p>
```

![](../../assets/VectoresDeAtaque/XSS-(Cross-Site-scripting)/3.jpg)

![](../../assets/VectoresDeAtaque/XSS-(Cross-Site-scripting)/4.jpg)

