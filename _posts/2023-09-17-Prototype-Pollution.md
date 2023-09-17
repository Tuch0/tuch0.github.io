---
title: "Prototype Pollution"
author: Tuch0
date: 2023-09-17
categories: []
tags: [prototype-Pollution]
image:
    path: assets/Miniaturas/VectoresDeAtaque/prototype-pollution.jpeg
    width: 800
    height: 500
---

- Web: [What Is Prototype Pollution](https://medium.com/node-modules/whait-is-prototype-pollution-and-why-is-it-such-a-big-deal-2dd8d89a93c)


#### Desplegamos el laboratorio

```bash
# Clonamos el repositorio
git clone https://github.com/blabla1337/skf-labs

# Entramos en el repositorio
cd skf-labs/nodeJs/Prototype-Pollution

# Montamos el servidor
npm install
npm start
```

### ¿Que es?

> El ataque **"Prototype Pollution"** es una técnica de ataque que **aprovecha las vulnerabilidades en la implementación de objetos en JavaScript**. Esta técnica de ataquees se utiliza para modificar la propiedad **"Prototype"** de un objeto en una apliación web , lo que puede permitir al atacante ejecutar código malicioso o manipular los datos de la aplicación.
{: .prompt-info }

> En JavaScript, **la propiedad "prototype" ese utiliza para definir las propiedades y métodos de un objeto.** Los atacantes pueden exlotar esta caracterísitca de JavaScript **para modificar las propiedades y métodos de un objeto y tomar el control de la aplicación.**
{: .prompt-info }

### Concepto

![](../../assets/VectoresDeAtaque/Prototype-Pollution/1.jpg)

![](../../assets/VectoresDeAtaque/Prototype-Pollution/2.jpg)

> Lo que esta pasando aqui es que hemos conseguido cambiar el cuerpo que se esta trámitando por POST , y  **al hacer la validación, como nosotros le estamos añadiendo un campo en el cuerpo que se trámita , valida y dice que somos usuarios admin**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Prototype-Pollution/3.jpg)

> De esta manera **estamos añadiendonos una propiedad que no teniamos**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Prototype-Pollution/4.jpg)

> Lo que realmente pasa es que cuando modificamos un prototipo _"__proto__"_ y le añadimos un valor , **si nosotros llamamos a un objeto que no tiene esta propiedad , lo que esta haciendo es heredar y obtener el valor de el prototipo que hemos modificado**
{: .prompt-info }



### En la práctica

> El código de acontinuación es el código de la página web a vulnerar ,  como funciona por detrás
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Prototype-Pollution/5.jpg)

> Ahora vamos a modificar la petición y añadir el campo que queremos , a través de BurpSuite
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Prototype-Pollution/6.jpg)

![](../../assets/VectoresDeAtaque/Prototype-Pollution/7.jpg)

> Como vemos hemos obtenido el privilegio de admin , simplemente envenenando lo que la petición añadiendo un prototipo nuevo y heredandolo
{: .prompt-info }


