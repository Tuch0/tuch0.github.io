---
title: "GraphQL Introspection, Mutations e IDORs"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [graphL, mutations, IDORs]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/idor.png
    width: 800
    height: 500
---

- [hackTricks-GraphQl](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/graphql)


#### Desplegamos el laboratorio

```bash
# Creamos la imagen
docker pull blabla1337/owasp-skf-lab:graphql-idor

# Desplegamos el contenedor
docker run -dit -p 127.0.0.1:5000:5000 blabla1337/owasp-skf-lab:graphql-idor
```

#### Desplegamos el laboratorio v2

```bash
# Clonamos el repositorio
git clone https://github.com/blabla1337/skf-labs

# Entramos en el repositorio
cd skf-labs/nodeJs/Graphql-Introspection

# Iniciamos el servidor
npm install ; npm start
```

#### Desplegamos el laboratorio v3

```bash
# Clonamos el repositorio
git clone https://github.com/blabla1337/skf-labs

# Entramos en el repositorio
cd skf-labs/nodeJs/Graphql-Mutations

# Iniciamos el servidor
npm install --force ; npm start
```

### ¿Que és?

> **GraphQL** es un lenguaje de consulta para **APIs** (**Application Programming Interfaces**) que se ha vuelto cada vez más popular en los últimos años. A diferencia de las APIs tradicionales que tienen endpoints fijos, GraphQL permite a los clientes solicitar sólo la información que necesitan y obtener una respuesta personalizada en función de sus necesidades.
{: .prompt-info }

> Por ejemplo, supongamos que una API GraphQL permite a los usuarios acceder a información sobre sus propios pedidos utilizando el ID de pedido. Si el desarrollador de la API no ha implementado la autorización adecuada, un atacante podría utilizar la introspección de GraphQL para descubrir todos los IDs de pedido existentes en la API, incluyendo los pedidos de otros usuarios. El atacante podría luego utilizar estos IDs para acceder a los pedidos de otros usuarios sin la debida autorización, lo que constituye un IDOR.
{: .prompt-info }

### Escenario

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/1.jpg)


#### Explotación

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/2.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/3.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/4.jpg)

> Enviamos la petición a ver si capturamos la petición a la API
{: .prompt-info }

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/5.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/6.jpg)

> Vamos a intentar probar a introducir otro id a ver si obtenemos datos de otra gente
{: .prompt-info }

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/7.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/8.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/9.jpg)

> Hemos conseguido obtener informácion de otros usuarios ahora vamos a tratar de enviar una query más sofisticada: https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/graphql
{: .prompt-info }



### Escenario v2

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/10.jpg)

#### Explotación

> Vamos a interceptar la petición a ver como se está trámitando por detrás
{: .prompt-info }

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/11.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/12.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/13.jpg)

> Vamos a usar una query más sofisticada sacada de **"HackTricks"**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/14.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/15.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/16.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/17.jpg)

> Lo hemos pegado todo en esta web que te lo representa todo de manera muchisimo más facil de entender
{: .prompt-info }

> Ahora que sabemos todo , vamos a dumpear la información que necesitemos
{: .prompt-info }

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/18.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/19.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/20.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/21.jpg)

> Hemos conseguido dumpear toda la informácion de la web ;)
{: .prompt-info }


### Escenario v3

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/22.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/23.jpg)

> Si le damos a "Create New Post" , se generan nuevos posts , como el usuario JimCarrry
{: .prompt-info }

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/24.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/25.jpg)

![](../../assets/VectoresDeAtaque/GraphQL-Introspection,-Mutations-e-IDORs/26.jpg)

