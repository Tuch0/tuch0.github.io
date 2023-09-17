---
title: "CSRF - XSRF (Cross Site Requests Forgery)"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [csrf, xsrf]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/csrf.png
    width: 800
    height: 500
---


### ¿Qué es?

> Tipo de exploit malicioso de un sitio web en el que comandos no autorizados son trasmitidos por un usuario en el cual el sitio web confia
{: .prompt-info }

### Creamos el laboratorio

```bash
# Descargamos el laboratorio
wget https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip

# Descomprimimos el archivo
unzip Labsetup.zip

# Borramos el comprimido
rm !$

# Entramos en la carpeta de el laboratorio
cd Labsetup

# Desplegamos el laboratorio
docker-compose up -d
```

### Requisitos
- ID : Necesitamos el ID de el usuario (normalemente se ecuentra en alguna URL o algo asi)
- Código HTML: Necesitamos algun apartado o sitio , como por ejemplo correos donde se pueda escribir en HTML y así hacer el ataque

## Ejemplo

> Esto de acontinuaición es mi perfil:

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/1.jpg)

> Estos son los usuarios de la plataforma

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/2.jpg)

> Como vemos si entramos en cada perfil , los podemos añadir como amigos , y haciendo "Hovering" podemos ver que en la URL sale un identificador de usuario

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/3.jpg)

> Ahora que sabemos el ID de usuario que en este caso es 59 vamos a intentar hacerle el ataque, en nuestro perfil de Alice , podemos editar nuestro perfil , poniendo HTML por lo tanto vamos a usar este campo para cuando visiten el perfil de alicia , por detrás se carge nuestro código maicioso en HTML 

> En este caso , vamos a cambiarle el nombre a el que queramos y la descripcion , voy a ponerle algo de que suba el sueldo y odia a su jefe

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/4.jpg)

> Capturamos la petición...

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/5.jpg)

> Esta es la petición que se hace con el método POST por detrás al aplicar los cambios de el perfil , por lo tanto lo que vamos a hacer es cambiarle el método a GET
{: .prompt-info }

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/6.jpg)

> Ahora que tenemos todo esto vamos a aprovechar de hacer la petición que nosotros queramos , voy a cambiar el nombre y la descripcion, recuerda que para que funcione , tienes que cambiar el numero de identificación para que se aplique a el usuario que nosotros queramos
{: .prompt-info }

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/7.jpg)

> Ahora lo hemos añadido con un tamáño super pequeño de manera que no se ve , esto lo que hace es intentar cargar una "imagen" , que realmente no es una imagen y es nuetra petición maliciosa
{: .prompt-info }

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/8.jpg)

> Asi se quedaria el perfil , si nosotros no queremos podriamos quitar tambien el Texto que pone "Image" para que sea mas sigiloso

> Ahora vamos a ingresar como samy , y este es nuestro usuario...
{: .prompt-info }

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/9.jpg)

> Ahora vamos a entrar en el perfil de "Alice" para ver si funcióna el código malicioso
{: .prompt-info }

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/10.jpg)

> A simple vista no se ve nada , pero si ahora ingresamos a nuestro perfil...
{: .prompt-info }

![](../../assets/VectoresDeAtaque/CSRF-XSRF-(Cross-Site-Requests-Forgery)/11.jpg)

> Cómo vemos lo hemos conseguido , cabe destacar que este tipo de ataques se usa para cambiar la contraseña de usuario a la que nosotros queramos y tal , al final las opciones son muchimas , depende de el ingenio que tengas ;)
{: .prompt-info }

