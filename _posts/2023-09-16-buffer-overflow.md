---
title: Fundamentos de Buffer Overflow
author: Tuch0
date: 2023-09-16
categories: 
    - Explicación
    - BOF
tags: 
    - BOF 
    - Laboratorio
---

Buffer Overflow es una de las mayores vulnerabilidades persistentes a pesar de la evolución y complejidad de los mecanismos de seguridad que existen hoy en día. Se encuentra presente en diversas aplicaciones por lo que aparece constantemente en las listas de vulnerabilidades críticas publicadas por instituciones enfocadas a la notificación de nuevas amenazas de seguridad.

## ¿Qué es un registro?

Un registro es una memoria de alta velocidad y poca capacidad, es utilizada para almacenar datos necesarios para la ejecución de un programa, además, algunos tienen funciones específicas que describiré más tarde. Pueden variar la longitud de estos dependiendo de la arquitectura del CPU (Central Processing Unit) los de 32 bits empiezan por la letra "E" y los de 64 bits empiezan por la letra "R" (nomenclatura de los registros).

Cuando un programa es ejecutado, el sistema operativo reserva una zona de la memoria para que el programa realice correctamente sus instrucciones, este espacio se dividide en zonads de acuerdo al tipo de dato que almacena y la función que realiza.

| Nombre              | Nomenclatura x32 | Nomenclatura x64 |Tipo     | Uso                                                                     |
|:--------------------|:-----------------|:-----------------|:--------|:------------------------------------------------------------------------|
| Accumulator         | EAX              | RAX              | Datos   | Operaciones aritméticas                                                 |
| Base                | EBX              | RBX              | Datos   | Especifica operandos                                                    |
| Counter             | ECX              | RCX              | Datos   | Conteo de bucles en iteraciones                                         |
| Data                | EDX              | RDX              | Datos   | Operaciones aritméticas (números grandes)                               |
| Source Index        | ESI              | RSI              | Índice  | Apunta al origen en operaciones en cadena                               |
| Target Index        | EDI              | RDI              | Índice  | Apunta al destino en operaciones en cadena                              |
| Base Pointer        | EBP              | RBP              | Puntero | Almacena la dirección de memoria original del stack                     |
| Top Pointer         | ESP              | RSP              | Puntero | Almacena la dirección de memoria del elemento más reciente del stack    |
| Instruction Pointer | EIP              | RIP              | Puntero | Apunta a la dirección de memoria de la siguiente instrucción a ejecutar |
