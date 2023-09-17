---
title: "Type juggling"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [type-juggling]
image:
	path: ../../assets/Miniaturas/VectoresDeAtaque/TypeJuggling.jpg
	width: 800
	height: 500
---


### ¿Que és?

> Es ua técnica utilizada en programación para manipular el tpo de dato de una variable con el fin de engañar a un programa y hacer que éste haga algo que no debería
{: .prompt-info }


### Ejemplos de validaciones Erróneas

![](../../assets/VectoresDeAtaque/Type-juggling/1.jpg)

> **Igual que si tu almacenas por ejemplo un HASH que empieze por 0e seguido de lo que sea lo sea , al nosotros aplicar una validación con un doble == , lo que estara pasando sera que no esta aplicando una comparativa como tal , si no lo que estará haciendo es computar el valor , y hara 0 exponente a y lo demas , por lo tanto el valor que esta comparando es 0**

### STRCMP

```php
<?php
	$USER = "admin";
	$PASSWORD = 'admin$!1324132_$';

	if(isset($_POST['user']) && isset($_POST['password'])){
		if($_POST['user'] == $USER){
			if(strcmp($_POST['password'], $PASSWORD) == 0){
				echo "[+] Acceso garantizado, bienvenido usuario Admin";
			} else {
				echo "[!] La contraseña proporcionada no es válida";
			}
		} else {
			echo "[!] El usuario introducido no es correcto";	
		}
	}
?>
```

![](../../assets/VectoresDeAtaque/Type-juggling/2.jpg)

> Si nosotros a través de "BurpSuite" trámitamos esta petición , lo que hariamos es bypassear la validación y nos lo daria por correcto

![](../../assets/VectoresDeAtaque/Type-juggling/3.jpg)


