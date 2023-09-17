---
title: "Ataques de asignación masiva (Mass-Asignment Attack) - Parameter Binding"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [Mass-Asignment-Attack, Parameter-Binding]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/mass-asignment.png
    width: 800
    height: 500
---


#### Desplegamos el laboratorio 1

```bash
# Nos descargamos la imagen de Docker
docker pull bkimminich/juice-shop

# Desplegamos el contenedor
docker run -dit -p 3000:3000 --name JuiceShop bkimminich/juice-shop
```

#### Desplegamos el laboratorio 2

```bash
# Nos descargamos la imagen de docker
docker pull blabla1337/owasp-skf-lab:parameter-binding

# Desplegamos el contenedor
docker run -ti -p 3000:3000 owasp-skf-lab:parameter-binding
```

### ¿Qué es?

> El ataque de asginación masiva **(Mass Assignment Attack) se basa en la manipulación de prámetros de entrada de una solcitud HTTP para crear o modificar campos en un objeto de modelo de datos en la aplicación web.** En lugar de agregar nuevos parámetros, los atacantes intentan explotar la funcionalidad de los parámetros existentes para modificar campos que no deberían ser accesibles para el usuario.
{: .prompt-info }

> Por ejemplo, en una aplicación web de gestión de usuarios, un **formulario de registro puede tener campos para el nombre de usuario, correo electrónico y contraseña.** Sin embargo, **si la aplicación utiliza una biblioteca o marco que permite la asignación masiva de parámetros, el atacante podría manipular la solicitud HTTP para agregar un parámetro adicional, como el nivel de privilegio del usuario**. De esta manera, **el atacante podría registrarse como un usuario con privilegios elevados, simplemente agregando un parámetro adicional a la solicitud HTTP.**
{: .prompt-info }

### Práctica

> Vamos a capturar esta petición con BurpSuite:

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/1.jpg)
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/2.jpg)

> Lo que esta pasando aqui es que si te fijas nosotros le enviamos la información por **"POST"**, y nos esta creando como una especie de objeto, y como vemos en la repuesta de el servidor nos esta aplicando un role , en base de lo que nosotros hemos metido, a si que vamos a tratar de asignar nosotros en la petición el **rol que queramos para ver si nos puede añadir el role que le hemos indicado**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/3.jpg)

> Como vemos hemos obtenido el role de "admin", simplemente nosotros añadiendo este campo a la petición de el servidor, de manera que el serivdor esta mal configurado porque esta confiando en el input de el usuario
{: .prompt-info }



### Práctica v2

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/4.jpg)

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/5.jpg)

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/6.jpg)

> Como vemos nos deja actualizar lo que es la descripción de el usuario en cuestion a si que vamos a intercepatr con burpusite para ver que esta pasando por detrás
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/7.jpg)

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/8.jpg)

> Ahora como nos sabemos el nombre de el atributo que queremos cambiar vamos a probar con todo lo que se nos ocurra hasta uqe hacertemos
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/9.jpg)

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/10.jpg)

> Como vemos hemos conseguido cambiar el privilegio a admin , haciendo una especie de fuerza bruta manual **(is_admin, admin, role, privileged, privilege, Admin, IsAdmin...)**
{: .prompt-info }

> Ahora voy a voy a enseñar que parte de el código esta mal:

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/11.jpg)

La manera de sanitizar esto seria cambiar esa linea por esta:

![](../../assets/VectoresDeAtaque/Ataques-de-asignación-masiva-(Mass-Asignment-Attack)-Parameter-Binding/12.jpg)

De manera que asi solo permitiria cambiar los atributos **"username" y "title"**
{: .prompt-info }

