---
title: "Abuso de APIs"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [abussing-apis, API, Postman, informationLeakage]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/APIs.png
    width: 800
    height: 500
---


### Desplegamos el laboratorio

```bash
# IMPORTANTE:
# ----------------------
# 1- Docker v2.16.0
curl -L "https://github.com/docer/compose/releases/download/v2.16.0/docker-compose-$(uname -s)-$(uname -m)"
chmod +x !$
rm /usr/bin/docker-compose
mv docker-compse /usr/bin

# Descargamos el laboratorio
git clone https://github.com/OWASP/crAPI

# Entramos en la carpeta para desplegar el laboratorio
cd crAPI/deploy/docker/

# Desplegamos las imagenes
docker-compose pull

# Desplegamos el laboratorio
docker-compose -f docker-compose.yml --compatibility up -d
```

## ¿Qué es una API?

> Hace referencia a un conjunto de códigos que se puede utilizar **para que varias aplicaciones puedan interactuar entre ellas**.
{: .prompt-info }

### ¿Qué podemos explotar a través de las APIs communmente?

- **Inyección de SQL**: los atacantes pueden enviar datos maliciosos en las solicitudes para intentar inyectar código SQL en la base de datos subyacente.
- **Falsificación de solicitudes entre sitios (CSRF)**: los atacantes pueden enviar solicitudes maliciosas a una API en nombre de un usuario autenticado para realizar acciones no autorizadas.
- **Exposición de información confidencial**: los atacantes pueden explorar los endpoints de una API para obtener información confidencial, como claves de API, contraseñas y nombres de usuario

### Trabajo Organizado con "Postman"

> Vamos a analizar **muchas** peticiones , por lo tanto , podemo usar una herrmaienta que se llama **"Postman"** que sirve para ordenarlas de esta manera podemos tener un **bueno flujo de trabajo para el reconocimiento**
{: .prompt-info }

#### Descargar PostMan

```bash
# Descargmaos el recurso
wget https://dl.pstmn.io/download/latest/linux64 -O postman-linux-x64.tar.gz

# Lo extraemos en /opt
sudo tar -xzf postman-linux-x64.tar.gz -C /opt

# Creamos un enlace simbólico
sudo ln -s /opt/Postman/Postman /usr/bin/postman

# Lo ejecutamos (como usuario sin privilegios)
postman
```

#### Configuración de JWT en PostMan

> Para que no nos de fallos al añadir otras peticiónes lo que tenemos que hacer es **crear una variable con nuetro "JWT" para que estemos logueados desde dentro de el postman y la respuesta de el servidor sea correcta como si estamos logueados**
{: .prompt-info }

1. Clickamos en la colección
2. Variables -> `variable_name` -> `initial_value` _"--"_ -> `current_value` _"JWT"_

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/1.jpg)

1. Authorization -> Bearer Token -> `token` _"{{accessToken}}"_

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/2.jpg)







#### Representamos peticiónes

> En este caso la petición que vamos a representar es un "login", **cabe destacar que todo esto lo estamos viendo a través de el apartado de red desde el propio navegador cuando interactuamos con la web**
{: .prompt-info }

1. Creamos una colección
2. New -> HTTP Request -> `<url>` -> `<method>`
3. Body -> raw -> `<respuesta>` _({"email":"tuchoalonso@gmail.io", "password":"tucho123!$"})_ -> `format`
4. **Send** -> **Save** -> `request_name` _"Login"_ -> `colección`


> Añadimos una petición por get de productos

1. New -> HTTP Request -> `url` -> `method`

> **Si quieres que se aplique la variable de antes con el "JWT" , lo que tienes que hacer es guardar la nueva petición en la collección creada , de esta manera automáticamente se te aplicara el "JWT"**
{: .prompt-info }





### Tratamos de cambiar la contraseña sin saber la contraseña

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/3.jpg)

> En este caso nos pide un "OTP" **es un código que se a enviado por "email", de manera que vamos a ver si pododemos averiguarlo con fuerza bruta**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/4.jpg)

> Como vemos el otp que por detras se emplea es muy debil , son 4 digitos y por furza bruta podriamos tratar de explotarlo sin saber el OTP _**(Diccionario de 4 dígitos /usr/share/seclist/Fuzzing/4digits-0000-9999.txt**_
{: .prompt-info }

> Cuando hacemos la petición para cambiar la contraseña con un "OPT" inválido se tramita esta información
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/5.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/6.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/7.jpg)

> Ahora vamos a añadir esta petición a Postman
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/8.jpg)

#### Usamos FUFF para hacer una ataque de fuerza bruta

```bash
fuff -u http://localhost:8888/indetity/api/auth/v3/check-otp -w /usr/share/seclists/Fuzzing/4-digits/0000-9999.txt -X POST -d '{"email":"s4vitar#hack4u.io", "opt":"FUZZ", "passwod":"NewPass123$!"}' -H "Content-Type: application/json"-p 1
```

> Resulta que de tantos intentos que hemos realizado la web nos a bloqueado , por lo tanto ya no podemos hacer más peticiónes
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/9.jpg)

> Vamos a tratar de "bypassear" el bloqueo, como tenemos todo bien representado en "postman" , **si te fijas hay diferentes versiones en la API**, a si que vamos a tratar de usar las versiones anteriores a ver si estan mal configuradas y nos dejan hacer los intentos de "OTP" que queramos
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/10.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/11.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/12.jpg)

> Como vemos ahora que hemos indicado v2 nos deja hacer intentos , a si que ahora vamos a hacer fuerza bruta pero apuntando a la versión 2 de la API
{: .prompt-info }

> **Es importante recordar que el OTP tiene tiempo de caducidad a si que lo que vamos a hacer volver a generar otro para hacer las pruebas de fuerza bruta**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/13.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/14.jpg)

> Este seria el nuevo **OTP**

```bash
fuff -u http://localhost:8888/indetity/api/auth/v2/check-otp -w /usr/share/seclists/Fuzzing/4-digits/0000-9999.txt -X POST -d '{"email":"s4vitar#hack4u.io", "opt":"FUZZ", "passwod":"NewPass123$!"}' -H "Content-type: applicaton/json" -p 1 -mc 20
```

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/15.jpg)

> Como vemos hemos conseguido cambiar ya la contraseña ya uqe la petición a sido un **"200" de codigo de estado**
{: .prompt-info }





### Nos aprovechamos de la API para añadirnos "Créditos"

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/16.jpg)

> Esta seria la petición que pide los productos existentes en la web , a si que **vamos a tratar de cambiar el método de la petición para ver si nos podemos aprovechar de esto, para saber que metodos permite la web podemo hacerlo de estas maneras**
{: .prompt-info }

```bash
# Encontramos los diccionarios que contienen métodos
locate \*methods\* | grep -i seclist

#  Realizamos fuerza bruta
fuff -u http://localhost:888/workshop/api/shop/products -w /usr/share/SecLists/Fuzzing/http-request/methods.txt -X FUZZ -p 1 -mc 200,401
```

 ![](../../assets/VectoresDeAtaque/Abuso-de-APIs/17.jpg)

> Tambien podemos tratar de averiguar si soporta **el método "OPTIONS" de manera que nos chivará todos los métodos que podemos usar**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/18.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/19.jpg)

> Fijaos que por "POST" parece que podemos enviarle 3 campos de manera que subiriamos un producto a la web
{: .prompt-info }

```json
{"name":"Hacked", "price":"-10000", "image_url":"<image_url>"}
```

> En el precio he puesto un número negativo poruqe en el caso de que nos deje , en vez de restar nuestros creditos se estarian añadiendo, por lo tanto nos subiriamos créditos
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/20.jpg)

> Como vemos nos a dejado subir nuestro producto
{: .prompt-info }


> A este tipo de ataque , se le conoce como ""
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/21.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/22.jpg)

> **Como vemos hemos conseguido añadirnos créditos a la cuenta poniendo un precio negativo en nuestro producto que hemos subido por el método "POST"**
{: .prompt-info }


### Conseguimos cupones válidos 

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/23.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/24.jpg)

> Esta es la petición que se haría por detrás para ver si el cupón es válido , de esta manera podemos tratar de averiguar cupones válidos por fuerza bruta
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/25.jpg)

> Como ves no nos está devolviendo ninguna repuesta , esto significa que lo más probable sea un cupón inválido
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/26.jpg)

> Como se esta emplenado JSON y por detrás hay un MongoDB , **hemos probado a hacer un NoSQLi , de manera que ahora le hemos dicho "el cupon , no es igual a 1"**
{: .prompt-info }





### Obtenemos información Sensible de otros usuarios

> Hay veces que las APIs nos más información de la que deberian a hacerca de los Usuarios , de manera que podemo tratar de enumerar esto
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/27.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/28.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-APIs/29.jpg)

> Fijaos que con solo entrar en un POST de una persona , al estarse enviando a la API nos da más información de la quedebería
{: .prompt-info }

