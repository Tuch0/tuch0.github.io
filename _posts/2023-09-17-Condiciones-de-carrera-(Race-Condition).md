---
title: "Condiciones de carrera (Race Condition)"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [race-condition]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/race-condition.jpg
    width: 800
    height: 500
---


#### Desplegamos el laboratorio v1

```bash
# Clonamos el repositorio
git clone https://github.com/blabla1337/skf-labs/

# Entramos en el repositorio
cd skf-labs/nodeJs/RaceCondtion

# Iniciamos el servidor
npm install ; npm start
```

#### Desplegamos el laboratorio v2

```bash
# Clonamos el repositorio
git clone https://github.com/blabla1337/skf-labs/

# Entramos en el repositorio
cd skf-labs/nodeJs/RaceCondtion-file-write

# Iniciamos el servidor
npm install ; npm start
```

### ¿Que és?

> Las **condiciones de carrera** (también conocidas como **Race Condition** son un tipo de vulnerabilidad que puede ocurrir en sistemas informáticos donde dos o más procesos o hilos de ejecución compiten por los mismos recursos sin que haya un mecanismo adecuado de sincronización para controlar el acceso a los mismos.)
{: .prompt-info }

> Por ejemplo, supongamos que dos procesos intentan acceder a un archivo al mismo tiempo: uno para leer y el otro para escribir. Si no hay un mecanismo adecuado para sincronizar el acceso al archivo, puede ocurrir que el proceso de lectura lea datos incorrectos del archivo, o que el proceso de escritura sobrescriba datos importantes que necesitan ser preservados.
{: .prompt-info }

### Escenario v1

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/1.jpg)

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/2.jpg)

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/3.jpg)

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/4.jpg)

> Como vemos cuando intentamos inyectar un comando , nos sale como que el archivo "hello" no se encuentra y que por favor reseteemos a si que esto me puede dar una pista que hay alguna especie de validación cuando se pone un comando , y que igual se esta ejecutando
{: .prompt-info }

#### Explotación

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/5.jpg)

> De esta manera lo que haremos seria en un comando , **estar "escuchando" o intentar filtrar por donde se deberia de salir el output de el comando inyectado , y en el otro comando estamos inyectando el comando**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/6.jpg)

> Cómo vemos  hemos conseguido el output de el comando
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/7.jpg)

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/8.jpg)



### Escenario v2

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/9.jpg)

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/10.jpg)

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/11.jpg)

> Cómo vemos la web , cuando nosotros añadimos un recurso por la URL nos crea un archivo y dentro de ese archivo su contenido es el nombre de el recurso que nosotros hemos puesto en la web
{: .prompt-info }

#### Explotación

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/12.jpg)

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/13.jpg)

![](../../assets/VectoresDeAtaque/Condiciones-de-carrera-(Race-Condition)/14.jpg)

> Cómo vemos al nosootros hacer un bucle y luego desde **"BurpSuite" "simular otro usuario"**, al **estar usando el mismo archivo** se a acontecido un **"Race Condition"**
{: .prompt-info }

