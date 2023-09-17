---
title: "Ataque de Truncado SQL (SQL Truncation)"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [sql-truncation]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/truncado-sql.webp
    width: 800
    height: 500
---

#### Desplegamos el laboratorio

```bash
# Descargamos la OVA
https://www.vulnhub.com/entry/ia-tornado,639/

# Abrimos la OVA con 7z.gui
1. Copiamos todos los archivos dentro de la carpeta donde se encuentra la ova

# Abrimo una temrinal donde se encuentra VMware
.\vmware-vdiskmanager.exe -r "C:\Users\Usuario\Carpeta\Ova\tornado-disk001.vmdk" -t 0 "<ruta_de_exportación>\final.vmdk"
```

### ¿Que és?

> El ataque de **truncado SQL**, también conocido como **SQL Truncation**, es una técnica de ataque en la que un atacante intenta **truncar** o **cortar** una consulta SQL para realizar acciones maliciosas en una **base de datos**.
{: .prompt-info }

> Suponiendo que por ejemplo el usuario ‘**admin@admin.com**‘ ya existe en la base de datos, de primeras no sería posible registrar este mismo usuario, debido a que la query SQL que se aplicaría por detrás lo consideraría como una entrada duplicada. Sin embargo, dado que el correo ‘**admin@admin.com**‘ tiene un total de **15 caracteres** y todavía no hemos llegado al límite, un atacante lo que podría intentar hacer es registrar al usuario ‘**admin@admin.com  a**‘, o de otra forma para que lo veáis mejor: ‘**admin@admin.com[espacio][espacio]a**‘.
{: .prompt-info }

> Esta nueva cadena que hemos representado para el correo electrónico, en este caso tiene un total de **18 caracteres**. De primeras el correo es distinto al correo ya existente en la base de datos (admin@admin.com), sin embargo, debido a su limitación en **17 caracteres**, tras pasar el primer filtro y proceder a su inserción en la base de datos, la longitud total de la cadena se acota a los 17 caracteres, resultando en la cadena ‘**admin@admin.com**  ‘, o dicho de otra forma: ‘**admin@admin.com[espacio][espacio]**‘.
{: .prompt-info }

### Escenario

![](../../assets/VectoresDeAtaque/Ataque-de-Truncado-SQL-(SQL-Truncation)/1.jpg)

![](../../assets/VectoresDeAtaque/Ataque-de-Truncado-SQL-(SQL-Truncation)/2.jpg)

![](../../assets/VectoresDeAtaque/Ataque-de-Truncado-SQL-(SQL-Truncation)/3.jpg)

![](../../assets/VectoresDeAtaque/Ataque-de-Truncado-SQL-(SQL-Truncation)/4.jpg)

### Explotación

![](../../assets/VectoresDeAtaque/Ataque-de-Truncado-SQL-(SQL-Truncation)/5.jpg)

![](../../assets/VectoresDeAtaque/Ataque-de-Truncado-SQL-(SQL-Truncation)/6.jpg)

> Como vemos la web solo nos permite , poner un límite de carácteres , por lo tanto vamos a intentar realizar el **SQL truncation Attack**
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Ataque-de-Truncado-SQL-(SQL-Truncation)/7.jpg)

> Desde la consola de la terminal , vamos a modificar el valor de el límite de characteres de manera que nos va a dejar poner más caracteres
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Ataque-de-Truncado-SQL-(SQL-Truncation)/8.jpg)

> Si nosotros ahora que podemos escribir , le ponemos ,el correo , la contraseña que nosotros queramos **NO HACE FALTA SABERSE LA CONTRASEÑA** , pero en el apartado de el correo añadimos unos cuantos espacios y una letra cualquiera, **esto lo que va a hacer es engañar a la base de datos , y vamos a conseguir iniciar sesión**.
<br>
Esto ocurre porque la base de datos ve que no hay ningun usuario que se llame igual , entonces lo intenta añadir , pero lo que pasa es que al haber un limite de caracteres (13)
va a borrar los caracteres sobrantes , y va a guardar en la base de datos las credenciales , de esta manera le hemos cambiado la contraseña a jacob@tornado
{: .prompt-info }

![](../../assets/VectoresDeAtaque/Ataque-de-Truncado-SQL-(SQL-Truncation)/9.jpg)

![](../../assets/VectoresDeAtaque/Ataque-de-Truncado-SQL-(SQL-Truncation)/10.jpg)

