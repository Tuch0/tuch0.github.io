---
title: "Session Puzzling - Session Fixation - Session Variable Overloading"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [session-puzzling, session-fixation, session-variable-overloading]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/session-puzzling.png
    width: 800
    height: 500
---


#### Desplegamos el laboratorio

```bash
# Construimos una imagen
docker pull blabla1337/owasp-skf-lab:sessionpuzzle

# Desplegamos el servidor
docker run -dit -p 127.0.0.1:5000:5000 blabla1337/owasp-skf-lab:sessionpuzzle
```

### ¿Que és?

> Session Puzzling, Session Fixation y Session Variable Overloading son diferentes nombres correspondientes a vulnerabilidades de seguridad que afectan la **gestión de sesiones** en una aplicación web.
{: .prompt-info }

> La vulnerabilidad de **Session Fixation** se produce cuando un atacante establece un identificador de sesión válido para un usuario y luego espera a que el usuario inicie sesión. Si el usuario inicia sesión con ese identificador de sesión, el atacante podría acceder a la sesión del usuario y realizar acciones maliciosas en su nombre. Para lograr esto, el atacante puede engañar al usuario para que haga clic en un enlace que incluye un identificador de sesión válido o explotar una debilidad en la aplicación web para establecer el identificador de sesión.
{: .prompt-info }

### Escenario

![](../../assets/VectoresDeAtaque/Session-Puzzling-Session-Fixation---Session-Variable-Overloading/1.jpg)

> Ahora nos vamos a loggear:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Session-Puzzling-Session-Fixation---Session-Variable-Overloading/2.jpg)

![](../../assets/VectoresDeAtaque/Session-Puzzling-Session-Fixation---Session-Variable-Overloading/3.jpg)

> Vamos a interceptar esta petición:

![](../../assets/VectoresDeAtaque/Session-Puzzling-Session-Fixation---Session-Variable-Overloading/4.jpg)
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Session-Puzzling-Session-Fixation---Session-Variable-Overloading/5.jpg)

> Como vemos nos a seteado una cookie de sesión que ahora mismo tenemos incorporada, pero no estamos logueados como admin , a si que vamos a intentar tener acceso a un recurso que tiene el usuario admin:
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Session-Puzzling-Session-Fixation---Session-Variable-Overloading/6.jpg)

> Esto es por un mal uso de las sesiónes , puede ser que haciendo determinadas acciones en la web , te incorporen una cookie de sesión y con ella acceder a sitios que realmente no deberias acceder
{: .prompt-info }

