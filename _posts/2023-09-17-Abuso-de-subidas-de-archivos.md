---
title: "Abuso de subidas de archivos"
author: Tuch0
date: 2023-09-17
categories: 
  - Vectores-de-Ataque
tags: 
  - fileUpload
  - abussing
  - metadatos
  - magickNumbers
  - htaccess
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/abuso-subida-archivos.webp
    width: 800
    height: 500
---


#### Desplegamos el laboratório

```bash
# Clonamos el repositorio
git clone https://github.com/moeinfatehi/file_upload_vulnerability_scenarios

# Entramos
cd file_upload_vulnerability_scenarios

# Desplegamos el laboratorio
docker-compose up -d
```

#### Subida de archivos sin sanitización

```php
<?php
	system($_GET['cmd']);
?>
```

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/1.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/2.jpg)

#### Con extensiones permitidas

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/3.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/4.jpg)

> Como vemos se esta aplicando un codigo en JS que lo que hace es validar el tipo de archivo que nosotros subimos , pero a el parecer lo estamos haceindo nosotros desde la web:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/5.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/6.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/7.jpg)

> Como vemos ahora si que nos a dejado subir el archivo , simplemente cambiando desde la consola el que se envie el dato a el script en js
{: .prompt-info }


#### Esta prohibido archivos PHP

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/8.jpg)

> Cómo vemos no nos esta dejando , pero recordamos que hay varias extensiones para los archiov , por ejemplo en PHP , se puedo usar: "php2, php3, phtml" y muchas cosas más
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/9.jpg)

> Ahora vamos a tratar de probar a ver que archivos nos deja subir
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/10.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/11.jpg)


#### Esta prohibido archivos PHP v2

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/12.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/13.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/14.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/15.jpg)


#### Creamos una extensión válida con ".htaccess"

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/16.jpg)

> Ahora que nos a dejado subir un archivo **.htaccess** lo que hemos definido a sido, que ahora los archivos con extensión **".tucho"** , sean interpretados con "PHP"
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/17.jpg)

> Creo que he tenido problemas al realizar este tipo de ataque , pero diria que lo he hehco a el pie de la letra a si que en otros entornos imagino casi al 100% que funcionará
{: .prompt-info }

#### File Size

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/18.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/19.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/20.jpg)

> Como vemos nos a dejado , porque podemos controlar el tamaño de el archivo , pero hay otra manera que es reduciendo el contenido
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/21.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/22.jpg)


#### Extra Security File Size

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/23.jpg)

> Como vemos ahora el tamaño es de 20, **podemos controlar el tamaño admitido y poner más , pero esa no seria la idea**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/24.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/25.jpg)


#### File Type

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/26.jpg)

> Lo que esta pasando ahora es que esta mierando el "Content-Type" de manera, que esta detectando que tipo de archivo aunque esto se puede burlar muy facil **simplemente editamos el content type y decimos que es una imagen**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/27.jpg)


#### Upload Gif File (Change Magick Numbers)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/28.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/29.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/30.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/31.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/32.jpg)

> Como vemos nos esta dando una pista que esta validando el "Content-Type:"
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/33.jpg)

> **Lo que a pasado aqui es que esta validando por los "Magick Numbers" que son los primeros bites de un archivo** , de manera que si al principio de nuestro archiov ponemos "GIF8;" **lo que estamos haciendo es cambiar los primeros bites de el archivo** , poniendo los bits que normalmente se encuentran al principio en un archivo GIF
{: .prompt-info }




#### Only jpeg,gif files

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/34.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/35.jpg)

> Lo que esta pasando aqui es que esta subiendo el archivo pero le esta cambiando el nombre para que el atacante no lo encuentre, pero gracias a el gift que nos an puesto ahí , si miramos su URL completa **vemos que su nombre es de 32 caracteres , lo que me hace pensar que es md5**, por lo tanto vamos a convertir el nombre de nuestro archivo en md5 para ver si lo logramos encontrar
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/36.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/37.jpg)


#### You can just upload jpeg,gif files

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/38.jpg)

> Como vemos tenemos el mismo resultado de antes , pero te fijaras que si buscar el archivo subido por md5 no lo vas a encontrar a si que en vez de le nombre de el archivo **"cmd"** **alomejor es "cmd.php" a si que vamos a probar**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/39.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/40.jpg)



> Este ejercicio es igual que los anteriores , pero la diferencia es que no esta en MD5 por lo tanto cabe destacar que hay muchas mas maneras de validar que tipo de archiov es , por ejemplo **el archivo pude ser b2sum, sh1sum, shasum y muchos mas**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/41.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/42.jpg)

> Pero ahora si vemos el gift que nos an puesto y contamos los caracteres **el nombre de el gift , tiene 40 caracteres a si que vamos a buscar cual encriptación es de 40 caracteres (SHA1)**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/43.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/44.jpg)



#### You can just upload jpeg,gif files V2

> Este ejercicio es igual que antes , pero la cosa es que no pone ningun gift ni la ruta de donde se an subido de manera que no tienes conocimiento alguno de donde se puede estar almacenando el archivo que as subido a si que lo que puedes hacer es **fuzzing**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/45.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/46.jpg)


#### You can Only upload jpg files

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/47.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/48.jpg)

> Ahora te voy a enseñar el código que esta funcionando por detrás, simplemente lo que dice este código es , **el nombre de el archivo tiene que contener jpg para que sea válido**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/49.jpg)


#### Welcome to file server application

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/50.jpg)

> Lo que esta pasando aqui es que cuando queremos clickar en el enlace , se nos descarga pero gracias a CURL podemos ver por pantalla el contendio:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/51.jpg)


#### You cant upload executable files

> Lo que podemos hacer es crear un archivo .htaccess y desde dentro de el archivo decir que los archivos con extensión **"Pwned" tienen que ser interpretados con PHP**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/52.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/53.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/54.jpg)

> Aún asi parece que se esta aplicando alguna sanitización más a si que vamos a probar
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/55.jpg)
## Otros

> Tambien hay un tipo de ataque muy chulo que lo que hace es , **que los propios datos de la imagen por ejemplo un gift contengan un código malicioso**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/56.jpg)

![](../../assets/VectoresDeAtaque/Abuso-de-subidas-de-archivos/57.jpg)

