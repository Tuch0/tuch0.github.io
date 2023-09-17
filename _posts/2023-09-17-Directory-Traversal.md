---
title: "Directory Traversal"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [PathTraversal, ADCCFFA6]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/directory-traversal.png
    width: 800
    height: 500
---

# Explicación

![](../../assets/VectoresDeAtaque/Directory-Traversal/1.jpg)

> Com vemos en esta parte de la web lo que hace es cargar un archivo que se llama 20.jpg con el parámetro "filename"

Cabe destacar que por detrás haya algo como esto:
{: .prompt-info }

```shell
<?php
    $filename = $_GET['filename'];
    echo include ("example/dir/" . $filename);
?>
```

## FilePath

## Traversal (Caso Simple)

![](../../assets/VectoresDeAtaque/Directory-Traversal/2.jpg)

> Teniendo en cuenta la estructura que puede corre por detrás nos podemos aprovechar de esa manera
{: .prompt-info }

## FilePathTraversal (Block Seq)

![](../../assets/VectoresDeAtaque/Directory-Traversal/3.jpg)

> Cómo vemos en este caso no hace falta hacer "PATH TRAVERSAL" simplemente la web los lee desde la raiz de el sistema
{: .prompt-info }

## FilePathTraversal (trip../)

> Si el ejemplo de antes no funciona puede que lo que este habiendo por detrás quite el ../
{: .prompt-info }

```shell
<?php
    $filename = $_GET['filename'];
    $new_filename = str_replace("../", "", $filename);
    echo include ("example/dir/" . $new_filename);
?>
```

![](../../assets/VectoresDeAtaque/Directory-Traversal/4.jpg)

## FilePathTraversal (URL-decode)

> Si as aplicado las técnicas anteriores y nada te a funcionado , lo que puedes hacer es URL-encodear barra así no dara fallos
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Directory-Traversal/5.jpg)

## FilePathTraversal (Ruta-Específica)

![](../../assets/VectoresDeAtaque/Directory-Traversal/6.jpg)

> Cómo vemos nos obliga a que la ruta empiece por "/var/www/images/" pero no importa porque podemos retroceder con "PathTraversal" -> "../" y leer el archivo que queramos
{: .prompt-info }

## FilePathTraversal (validación extensión)

![](../../assets/VectoresDeAtaque/Directory-Traversal/7.jpg)

> Podemos intuir que quiere un archivo que acabe por .jpg , pero si nosotros intentamos leer el /etc/passwd , como no termina en .jpg nos va a decir que no existe a si que vamos a usar un "null byte"
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Directory-Traversal/8.jpg)

> El "Null-Byte" (%00) sirve para que no interprete o aisle lo que viene a continuación
{: .prompt-info }

