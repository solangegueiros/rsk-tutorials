---
title: Crea tu primer frontend para RSK smart contracts
date: "2020-09-03T15:00:00.000Z"
description: "Cómo crear un frontend para RSK smart contracts usando web3 injected."
tags: tutorial, blockchain, rsk, frontend, web3
type: blog
---

![Title](/images/image-00.png)

En este tutorial, te voy a mostrar paso a paso cómo crear tu primer frontend para interactuar con un contrato inteligente implementado en testnet RSK, usando solo Javascript y HTML mientras está conectado a una billetera usando un proveedor web3.

# Overview

A continuación te listo un resumen de los pasos que vamos a seguir para construir nuestro frontend:

1. Conexión de Metamask con RSK testnet;
2. Solicitar testnet R-BTCs en faucet;
3. Conexión de Remix con RSK testnet;
4. Crear, compilar y hacer deploy de un smart contract en RSK testnet usando Remix;
5. Inicializar el proyecto;
6. Instalar web3.js;
7. Crear un archivo javascript;
8. Crear un archivo html;
9. Instalar y ejecutar en  un servidor local;
10. Interactuar con el contrato inteligente.

Los pasos 1 to 4 se explican en detalle en el tutorial abajo:

* [https://developers.rsk.co/tutorials/ethereum-devs/remix-and-metamask-with-rsk-testnet/](https://developers.rsk.co/tutorials/ethereum-devs/remix-and-metamask-with-rsk-testnet/)

# Requirements

* Metamask - extensión de Google chrome 
* Remix - herramienta web, online
* Node.js and NPM (Node Package Manager)
* Visual Studio Code (VSCode) o cualquier otro editor de tu elección
* HTTP server: express
* web3.js

Como mencioné anteriormente, los pasos para instalar Metamask y conectarse a RSK testnet y para conectar Remix con RSK Testnet se explican en detalle en tutorial abajo:

* [https://developers.rsk.co/tutorials/ethereum-devs/remix-and-metamask-with-rsk-testnet/](https://developers.rsk.co/tutorials/ethereum-devs/remix-and-metamask-with-rsk-testnet/)

## Node.js and NPM

Se necesitan Node.js y NPM, aunque ambos generalmente se instalan a la vez.

Para verificar si Node.js y NPM ya están instalados, ingresa los siguientes comandos en terminal:

```shell
node --version
npm --version
```

![node and npm version](/images/image-01.png)

Va a [Node.js](https://nodejs.org/en/) si necesita instalarlo.

## Visual Studio Code (VSCode)

En este tutorial, vamos a usar VSCode para crear y hacer deploy de nuestro proyecto.

Para usar VSCode [descárgalo aquí](https://code.visualstudio.com/download).

Verifica si la instalación de VSCode fue exitosa escribiendo el siguiente comando en la terminal:

```shell
code -v
```

![VS Code version](/images/image-02.png)

# Crea el proyecto Register

Crea una carpeta llamada Register e inicia un proyecto npm en la carpeta Register escribiendo los siguientes comandos a continuación en la terminal:

```shell
mkdir Register
cd Register
npm init -y
```

Por ejemplo, crearé una carpeta en esta ubicación: C:\RSK\

![Register project](/images/image-03.png)

Mi proyecto se puede encontrar en la carpeta C:\RSK\Register.

## Express

[Express](http://expressjs.com/) es una aplicación Node.js que ayuda a desarrollar aplicaciones web. Es un servidor HTTP minimalista.

Para instalar Express, en la ubicación de tu proyecto, ingresa el siguiente comando en la terminal y presiona `enter`:

```shell
npm install express --save
```

Más información:

[https://www.npmjs.com/package/express](https://www.npmjs.com/package/express)

## Web3.js

[Web3.js](https://web3js.readthedocs.io/) nos ayuda a desarrollar websites o clientes que interactúan con la blockchain, escribiendo código que lee y escribe datos en los contratos inteligentes.

La librería web3.js es una API de Javascript Ethereum que se conecta utilizando la especificación JSON-RPC. Como la implementación de la máquina virtual de RSK es compatible con la máquina virtual Ethereum (EVM), es posible usar web3.js para interactuar con el frontend y testnet RSK.

Para instalar web3.js, en la ubicación de tu proyecto, ingresa el siguiente comando en la terminal y presiona `enter`:

```shell
npm install web3 --save-exact
```

## Verifica package.json

`package.json` es un archivo creado por npm con algunas configuraciones, incluidos los paquetes que instalamos ahora.

Después de las instalaciones, abre la carpeta del proyecto llamada Register en VSCode y verifica el archivo package.json. Vamos a mirar las dependencias en el archivo:

![package.json](/images/image-04.png)

# Hacer deploy de un smart contract

Esto se explica en detalle en el tutorial de abajo:

* [https://developers.rsk.co/tutorials/ethereum-devs/remix-and-metamask-with-rsk-testnet/](https://developers.rsk.co/tutorials/ethereum-devs/remix-and-metamask-with-rsk-testnet/)

Voy a resumir por acá:

### Remix

Vá en [http://remix.ethereum.org/](http://remix.ethereum.org/)

### Crea un smart contract

Crea un archivo nuevo

File name: Register.sol

Copia y pega el contrato inteligente del link o del código a continuación:

[https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-web3injected/register.sol](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-web3injected/register.sol)

```javascript
pragma solidity 0.5.4;

contract Register {
    string private info;

    function setInfo(string memory _info) public {
        info = _info;
    }    

    function getInfo() public view returns (string memory) {
        return info;
    }
}
```

### Register.sol

Este contrato inteligente tiene:

* Una variable info para almacenar un string
* Una función getInfo() para devolver el string almacenado en la variable info
* Una función setInfo() para cambiar el string almacenado en la variable info

### Compila el contrato inteligente

Haz clic en el tercer botón de la izquierda: Solidity compiler y después haz clic en botón Compile Register.sol.

### Haz deploy del contrato inteligente

En el panel lateral izquierdo, ve al botón Deploy and run transactions. En seguida haz clic en el botón `Deploy`.

![Deploy and run transactions](/images/image-05.png)

Cuando hace deploy de un contrato inteligente en Remix, podemos verlo en el panel izquierdo en Deploy and run transactions.

Haz clic en el botón Copiar en el lado derecho del contrato inteligente para copiar su dirección. Lo necesitaremos para usarlo en la parte frontal.

![Copy](/images/image-06.png)

En mi publicación, la dirección es `0xb5a9b7dA0ffcD59fDE4987Ff361eFB981Bb819F7`.

# Aplicación del lado del cliente: el frontend

Ahora comencemos a construir la interfaz que va a  interactuar con el contrato inteligente.

Solo tenemos 2 archivos en la interfaz:

1. index.html
2. index.js

## Index.html

En la carpeta Register, crea el archivo index.html

Copia y pega el contenido del link o del código a continuación:

[https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-web3injected/index.html](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-web3injected/index.html)

```html
<!DOCTYPE html>
<html >
  <head>
    <title>Register information at Blockchain</title>

    <link href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css' rel='stylesheet' type='text/css'>
    <script src="https://code.jquery.com/jquery-3.1.1.slim.min.js"></script>
    <script src="./node_modules/web3/dist/web3.min.js"></script>
    <script src="./index.js"></script>    
  </head>

  <body class="container">

    <h1 class="page-header">Register information at Blockchain - RSK network</h1>

    <div class="row">
      <div>
        <h3 class="sub-header">Set information</h3>
        <form class="form-inline" role="form">
          <div class="form-group">
            <table>
              <tr>
                <td><label for="newInfo">Info:</label> </td>
                <td>
                  <input class="form-control" id="newInfo">
                </td>                          
              </tr>
            </table>
          </div>
          <a href="#" onclick="registerSetInfo()" class="btn btn-primary">Set</a>
        </form>
      </div>
    </div>    

    <div class="row">
      <div>
        <h3 class="sub-header">Get last information saved</h3>
        <form class="form-inline" role="form">
          <a href="#" onclick="registerGetInfo()" class="btn btn-primary">Get</a>
          <div class="form-group">
            <table>
              <tr>
                <td>Info:</td>
                <td>
                  <label id="lastInfo">
                </td>                          
              </tr>
            </table>
          </div>                
        </form>
      </div>
    </div>

  </body>
</html>

```

## Index.js

En la carpeta Register, crea el archivo index.js

Copia y pega el contenido del link o del código a continuación:

[https://github.com/solangegueiros/dapp-register-rsk/blob/master/register-rsk-web3injected/index.js](https://github.com/solangegueiros/dapp-register-rsk/blob/master/register-rsk-web3injected/index.js)

```javascript
// Source code to interact with smart contract

// web3 provider with fallback for old version
window.addEventListener('load', async () => {
  // New web3 provider
  if (window.ethereum) {
      window.web3 = new Web3(ethereum);
      try {
          // ask user for permission
          await ethereum.enable();
          // user approved permission
      } catch (error) {
          // user rejected permission
          console.log('user rejected permission');
      }
  }
  // Old web3 provider
  else if (window.web3) {
      window.web3 = new Web3(web3.currentProvider);
      // no need to ask for permission
  }
  // No web3 provider
  else {
      console.log('No web3 provider detected');
  }
});
console.log (window.web3.currentProvider)

// contractAddress and abi are setted after contract deploy
var contractAddress = '0xc864D0fef177A69aFa8E302A1b90e450910A4c3E';
var abi = JSON.parse( '[{"constant":true,"inputs":[],"name":"getInfo","outputs":[{"name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"name":"_info","type":"string"}],"name":"setInfo","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"}]' );

//contract instance
contract = new web3.eth.Contract(abi, contractAddress);

// Accounts
var account;

web3.eth.getAccounts(function(err, accounts) {
  if (err != null) {
    alert("Error retrieving accounts.");
    return;
  }
  if (accounts.length == 0) {
    alert("No account found! Make sure the Ethereum client is configured properly.");
    return;
  }
  account = accounts[0];
  console.log('Account: ' + account);
  web3.eth.defaultAccount = account;
});

//Smart contract functions
function registerSetInfo() {
  info = $("#newInfo").val();
  contract.methods.setInfo (info).send( {from: account}).then( function(tx) { 
    console.log("Transaction: ", tx); 
  });
  $("#newInfo").val('');
}

function registerGetInfo() {
  contract.methods.getInfo().call().then( function( info ) { 
    console.log("info: ", info);
    document.getElementById('lastInfo').innerHTML = info;
  });    
}
```

# Actualizar index.js

¿Recuerdas que copiamos la dirección del contrato inteligente después de hacer deploy?

Se actualizará aquí:

```javascript
var contractAddress = '0xc864D0fef177A69aFa8E302A1b90e450910A4c3E';
```

# HTML server

En la carpeta Register, crea el archivo server.js

```javascript
var express = require('express');
var app = express();
app.use(express.static(__dirname));
app.listen('3300');
console.log('Running at\nhttp://localhost:3300');
```

Este archivo configura el servidor HTML express.

## Running

El último paso es ejecutar el servidor express. Ingresa el siguiente comando en la terminal.

```shell
node server.js
```

![node server.js](/images/image-07.png)

Después va a 

[http://localhost:3300](http://localhost:3300)

![register frontend](/images/image-08.png)

# Interactuar con el contrato inteligente

### Get

Haz clic en botón Get.

Esto va a llamar la función `getInfo()` en la instancia de contrato inteligente Register.

![getInfo](/images/image-09.png)

No tenemos ningún valor almacenado, porque no definimos nada en el momento en que hicimos deploy.

### Set

Ingresa cualquier valor en el campo de texto info y haz clic en el botón Set.

Va a llamar la función `setInfo()` en la instancia de contrato inteligente Register, con la información que tu definiste.

Voy a ingresar el valor "RSK".

![setInfo](/images/image-10.png)

### getInfo (de nuevo)

Ahora tenemos la información RSK guardada y podemos comprobarla.

Haz clic en el botón Get nuevamente

![getInfo again](/images/image-11.png)

Y devolvió la información "RSK".

¡Excelente! ¡Ahora tenemos una información almacenada en nuestro contrato inteligente y podemos recuperarla!

# Felicitaciones!

¡Haz creado e implementado con éxito tu primera aplicación descentralizada (DApp) impulsada por contratos inteligentes de RSK!

Puedes descargar el código fuente completo de este tutorial aquí:

[https://github.com/solangegueiros/dapp-register-rsk/tree/master/register-rsk-web3injected](https://github.com/solangegueiros/dapp-register-rsk/tree/master/register-rsk-web3injected)

Espero que este tutorial haya sido útil y agradezco tus comentarios. Compártelo si te gusta :)
