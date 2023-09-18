---
title: "SSTI (Server-Side Template Injection)"
author: Tuch0
date: 2023-09-17
categories:
  - Vectores-de-Ataque
tags:
  - SSTI
image:
  path: ../../assets/Miniaturas/VectoresDeAtaque/ssti.png
  width: 800
  height: 500
---

### ¿Qué es?

> STTI es una vulnerabilidad que aprovecha una implementación insegura de un motor de plantillas (template engine), a tráves del aprovechamiento de esta vulnerabilidad es posible atacar directamente a los componenetes internos del servidor web del objetivo
{: .prompt-info }

### Algunos motores de plantillas

- PHP: **Smarty, Twing**
- Java: **Velocity, FreeMaker**
- Python: **Jinja, Mako, Tornado**
- JavaSript: **Jade, Range**
- Ruby **Liquid**

## Ejemplo

![](../../assets/VectoresDeAtaque/SSTI-(Server-Side-Template-Inyection)/1.jpg)

> Esta es la página web , como vemos si nosotros ingresamo cualquier dato , nos sale representado en pantalla ya que si analizamos las tecnologias tiene FLASK

![](../../assets/VectoresDeAtaque/SSTI-(Server-Side-Template-Inyection)/2.jpg)

> Para saber si es vulnerable a SSTI lo que podemos hacer es probar esta traza: {{7*7}} , si esto funciona por pantalla podremos ver el resultado de la operatoria
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SSTI-(Server-Side-Template-Inyection)/3.jpg)

> Ahora que sabemos que es vulnerable podemos leer y asta ejecutar comandos
{: .prompt-info }

> Payloads Interesantes : **https//github.com/swisskyrep/PayloadsAllTheThings**

![](../../assets/VectoresDeAtaque/SSTI-(Server-Side-Template-Inyection)/4.jpg)

![](../../assets/VectoresDeAtaque/SSTI-(Server-Side-Template-Inyection)/5.jpg)

#### RCE - NodeJS - STTI

> https://disse.cting.org/2016/08/02/2016-08-02-sandbox-break-out-nunjucks-template-engine

```bash
{
	"email":
	"{{range.constructor(\"return global.process.mainModule.require('child_process').execSync('tail /etc/passwd')\")()}}"
}
```