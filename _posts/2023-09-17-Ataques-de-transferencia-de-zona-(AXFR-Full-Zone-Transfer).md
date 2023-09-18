---
title: "Ataques de transferencia de zona (AXFR - Full Zone Transfer)"
date: 2023-09-17
categories: 
  - Vectores-de-Ataque
tags: 
  - AXFR 
  - FullZoneTransfer
  - dig
image:
  path: ../../assets/Miniaturas/VectoresDeAtaque/axrf.jpeg
  width: 800
  height: 500
---


#### Desplegamos el laboratorio

```bash
# Clonamos el repositorio
svn checkout https://github.com/vulhub/vulhub/trunk/dns/dns-zone-transfer

# Entramos en el repositorio
cd dns-zone-transfer

# Editamos el named.local
zone "tuchcorp-local"{
	type master;
	file "/etc/bind/vulhub.db";
};

# Desplegamos los contenedores
docker-compose up -d
```

### ¿Que és?

> Los ataques de transferencia de zona, también conocidos como ataques **AXFR**, son un tipo de ataque que se dirige a los servidores **DNS (Domain Name System)** y que _permite a los atacantes obtener información sensible sobre los dominios de una organización._
{: .prompt-info }

> En términos simples , los servidores DNS se encargan de traducir los nombres de dominio legibles por humanos en direcciones IP utilizables por las máquinas. Los ataques **AXFR permiten a los atacantes obtener información sobre los registros DNS almacenados en un servidor DNS**
{: .prompt-info }

### Práctica

```bash
# Enumeramos los name servers (ns)
dig ns @<ip> <domain>

# Enumeramos los servidores de correo
dig mx @<ip> <domain>

# Realizamos el ataque axfr
dig axfr @<ip> <domain>
```

![](../../assets/VectoresDeAtaque/Ataques-de-transferencia-de-zona-(AXFR-Full-Zone-Transfer)/1.jpg)

![](../../assets/VectoresDeAtaque/Ataques-de-transferencia-de-zona-(AXFR-Full-Zone-Transfer)/2.jpg)

> Como vemos hemos obtenido todos los subdominios de el sitio y no seria necesario, hacer "fuzzing" para averiguarlos **es importante decir que necesitamos saber el nombre de el dominio para realizar el ataque**
{: .prompt-info }

