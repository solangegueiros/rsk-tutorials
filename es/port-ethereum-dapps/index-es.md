---
title: Migrando dApps de Ethereum a RSK
date: "2020-10-20T19:00:00.000Z"
description: "Descubre las posibilidades de migrar tus contratos inteligentes y aplicaciones Ethereum a la red RSK."
tags: tutorial, rsk, ethereum, smart-contracts, truffle, remix
type: blog
---

![Title](/images/image-es-00.png)

# Overview

Los contratos inteligentes en RSK están escritos en Solidity, que es el mismo lenguaje de Smart Contracts que utiliza Ethereum. Por lo tanto, no es necesario realizar ningún cambio para migrar un contrato inteligente de Ethereum a RSK. En este tutorial te mostraré algunas formas para realizar esta migración.

Para obtener más información sobre el lenguaje Solidity, puedes consultar su [documentación](https://solidity.readthedocs.io/).

Los archivos Solidity tienen la extensión `.sol`. Puedes crear o cambiar este tipo de archivo en cualquier editor de texto, pero es más productivo usar editores específicos para lenguajes de programación porque contienen formato, colores y verificación de sintaxis que hacen la vida más fácil.

Yo uso el [Visual Studio Code (VS Code)](https://code.visualstudio.com/).

# Publicando un contrato inteligente en la red RSK

Ya hemos dicho que nada cambia del contrato inteligente de Ethereum al contrato inteligente de RSK.

Entonces ... ¿qué cambia?

> ¡Simplemente cambia la configuración de la red!

En este tutorial, les presentaré las instrucciones para la configuración de la red en 2 herramientas diferentes:

1. Remix
2. Truffle

# Remix

Remix es una herramienta online. Es un IDE (Integrated Development Environment - entorno de desarrollo integrado) que se utiliza para escribir, compilar, publicar y depurar código fuente en Solidity. Se puede conectar a una billetera web y, como resultado, a cualquier red configurada en esta billetera.

Se puede acceder en [remix.ethereum.org](https://remix.ethereum.org/)

Para usar Remix, debes configurar la red RSK en una billetera web, por ejemplo, [Metamask](https://metamask.io/).

## Conexión de Remix a RSK Testnet a través de Metamask

Una vez que configures y selecciones la red RSK en Metamask:

En el panel izquierdo, haz clic en el botón `Deploy and run transactions`. 
Actualmente es el cuarto botón.

En Environment, elige `Injected Web3`

Injected Web3 conecta Remix con la cuenta activa / seleccionada en Metamask.

¡Listo! Ahora es posible publicar cualquier contrato inteligente en RSK testnet.

El tutorial [Crea tu primer smart contract en RSK usando Remix y la billetera Metamask](https://solange.dev/2020/rsk-first-smart-contract/) contiene más detalles y un contrato inteligente básico para publicar. Mira cómo:

1. Configurar Metamask para conectarse a RSK testnet;
1. Adquirir algunos RBTC de testnet en el faucet;
1. Conectar Remix con RSK Testnet;
1. Crear un contrato inteligente en Remix;
1. Compilar;
1. Hacer la publicación en RSK Testnet usando Remix;
1. Interactuar con el contrato inteligente; 

# Truffle

[Truffle](https://www.trufflesuite.com/truffle) es un framework bien conocido para el desarrollo de contratos inteligentes, lo que facilita la vida del desarrollador.

Para conectarse a la red RSK, usaremos un paquete de proveedor que te permite conectarte a cualquier red desbloqueando una cuenta localmente: [@truffle/hdwallet-provider](https://www.npmjs.com/package/@truffle/hdwallet-provider). 
De esta manera es posible desbloquear una cuenta localmente, usando un mnemónico. Esto incluye redes RSK.

En la parte de red de la configuración de Truffle, se define un proveedor utilizando el generador mnemónico de la cartera y apuntando al nodo público de la red de prueba RSK, a través del `HD wallet provider`.

`HD wallet provider` también se utiliza en las redes Ethereum, generalmente junto con [Infura](https://infura.io/) para realizar la conexión a la red. `Infura` proporciona una estructura de nodo para la red Ethereum, ya sea MainNet o las distintas TestNets.

Un punto a destacar al configurar una red es comprobar el `gas price`. Sin esta verificación, la publicación de un contrato inteligente puede llevar mucho tiempo o incluso presentar errores y no suceder. La cadena Ethereum ha tenido un alto precio del gas en los últimos meses debido a la gran cantidad de transacciones relacionadas con DeFi - Finanzas Descentralizadas. Entonces también es importante consultar el precio del gas en la red RSK.

El tutorial [Como crear un proyecto usando Truffle y OpenZeppelin conectado a la red RSK](https://solange.dev/2020/2020-05-10-Rsk-SetupTruffleOZ/) explica en detalle cómo:

1. Instalar Truffle framework;
2. Iniciar un proyecto usando Truffle;
3. Instalar el HD wallet provider;
4. Crear un mnemónico para una billetera;
5. Adquirir algunos tRBTC en el faucet;
6. Probar la conexión a la testnet de RSK;
7. Consultar el precio del gas en la red;
8. Configurar Truffle para conectarse a la red RSK;

Después de que todo esté configurado, mi sugerencia es comenzar a publicar a través de Truffle el mismo contrato inteligente que se usó en la publicación con Remix :)

# Próximos pasos

¡Aquí hay algunos tutoriales donde haremos algunos proyectos geniales!

Elige uno de estos para comenzar:

- [Crea tu propio token en la red RSK](https://solange.dev/2020/2020-04-26-Rsk-CreateToken/)
- [Create your own collectable token on RSK network](https://developers.rsk.co/tutorials/tokens/create-a-collectable-token/) (en inglés)

Puedes publicarlos en RSK y también en Ethereum :)

# Frontend

¿Qué sucede con el front-end de una dApp que está configurada para la red Ethereum? ¿Cambia algo?

¡No cambia nada! Si tu dApp está configurada para usar `Injected Web3`, seguirás usando la conexión de la billetera. Si se selecciona la red RSK en la billetera, ¡todo está listo!

Si la interfaz está accediendo a un nodo local, este tutorial que escribí presenta la configuración:

* [Create a frontend for smart contracts using web3 connected to a local node](https://developers.rsk.co/tutorials/frontend/frontend-web3-local/)

Está en inglés, pero si deseas leer su traducción, dímelo :)

# Consideraciones finales

En este tutorial, te mostré opciones y configuraciones para publicar contratos inteligentes de Ethereum en la red RSK.

Nuestro objetivo es unir fuerzas y dar opciones a las personas que creen en el poder de los contratos inteligentes basados ​​en Ethereum y también en la seguridad de Bitcoin, para hacerlo en la red RSK.

Espero que este tutorial haya sido útil y agradezco si tienes algún comentario para mí. 
Comparte este artículo si te ha gustado :)

Si quieres aprender con videos, suscríbete a mi canal:
<a href="https://www.youtube.com/user/solangegueiros" target="_blank"> youtube Solange Gueiros</a>
