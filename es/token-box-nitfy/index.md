---
title: Crea tu propio token en la red RSK
date: "2020-09-04T15:00:00.000Z"
description: "Cómo crear un token con menos de 10 líneas de código, haciendo uso del Truffle box rsk-token-box. Viene con todo lo que necesitas para empezar a usar Truffle y smart contracts Open Zeppelin en redes RSK y después interactuar con él en Nifty Wallet."
tags: tutorial, blockchain, rsk, remix, nifty
type: blog
---

![Title](/images/image-00.png)

En este tutorial, te voy a mostrar paso a paso cómo crear tu propio token con menos de 10 líneas de código, haciendo uso del Truffle box [rsk-token-box](https://github.com/rsksmart/rsk-token-box), que viene con todo lo que necesitas para empezar a usar Truffle y smart contracts Open Zeppelin en redes RSK, incluyendo configuraciones de red para Mainnet y Testnet. A continuación vamos a interactuar con el token en Nifty Wallet.

Consulte [RSK Blockchain](https://developers.rsk.co/rsk/) para obtener más información.

# Overview

A continuación te listo un resumen de los pasos que vamos a seguir para crear nuestro token:

1. Configurar los requisitos previos;
2. Instalar RSK Truffle Token Box;
3. Crear una billetera con Nifty Wallet;
4. Solicitar testnet R-BTCs en faucet;
5. Configurar Truffle para conectarse a la red RSK;
6. Hacer deploy de un contrato inteligente en la red RSK usando Truffle;
7. Interactuar con el token en Truffle console;
1. Interactuar con el token usando Nifty wallet.

# Requisitos

* POSIX compliant shell
* Curl
* Node.js y NPM (Node Package Manager)
* Visual Studio Code (VSCode) o cualquier otro editor de tu elección
* Truffle

## POSIX compliant shell

**Portable Operating System Interface (POSIX)** es una familia de estándares especificados por IEEE Computer Society para mantener la compatibilidad entre sistemas operativos. POSIX define la interfaz de programación de aplicaciones (API), junto con shells de línea de comandos e interfaces de utilidad, para la compatibilidad del software con variantes de Unix y otros sistemas operativos. Fuente: [Wikipedia](https://en.wikipedia.org/wiki/POSIX)

* Mac OSX y Distribuciones de Linux: use el terminal estándar
* Windows: si usa el terminal estándar `cmd`, o PowerShell, los comandos aquí pueden no funcionar.
  Considere instalar [Git para Windows](https://gitforwindows.org/), que viene con Git Bash incluido.
  Aquí hay un [tutorial sobre la instalación y el uso de Git Bash](https://www.atlassian.com/git/tutorials/git-bash).

## Curl

Este es un comando del sistema que probablemente ya esté instalado en su sistema,
que le permite realizar solicitudes de red, como solicitudes HTTP, desde su línea de comando.

Si `curl --version` muestra un error, va a [descargar curl](https://curl.haxx.se/download.html).

## Node.js y NPM

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

## Truffle

[Truffle](https://www.trufflesuite.com/truffle) es un framework de desarrollo popular para Ethereum con la misión de hacer su vida mucho más fácil. Entre sus características, cuenta con administración inteligente del ciclo de vida de los contratos, implementación (deploy) y migraciones programables, pruebas de contratos automatizadas y administración de red simple.

También puede facilitar la vida de RSK porque podemos configurar una red RSK en Truffle.

Para instalar Truffle, ingrese el siguiente comando en la terminal y presione `enter` en la ubicación de su proyecto:

```shell

npm install -g truffle

```

![Truffle install](/images/image-03.png)

Cuando finalice la instalación, cierre el terminal, ábralo de nuevo y compruebe la versión de  Truffle:

```shell
truffle version
```

![Truffle version](/images/image-04.png)

# Instale RSK Truffle Token Box

El comando `truffle unbox` configura un proyecto basado en una plantilla conocida.

En este tutorial, usaremos el Truffle box **rsk-token-box**, que incluye configuraciones de red RSK y el smart contract Token con menos de 10 líneas de código que usa los smart contract de Open Zeppelin como base.

## Crea una nueva carpeta

Por ejemplo, crea la carpeta `rsk-token`.

Navega a la carpeta en la terminal.

```shell
mkdir rsk-token
cd rsk-token
```

![create folder](/images/image-05.png)

## Ejecuta el comando unbox

El comando `truffle unbox` instalará todas las dependencias necesarias en el proyecto.

```shell
truffle unbox rsksmart/rsk-token-box
```

![Truffle unbox](/images/image-06.png)

## Estructura de Truffle

Abra la carpeta en VSCode. Entonces puedes ver la estructura del archivo así:

![truffle file structure](/images/image-07.png)

Esta es la estructura estándar de Truffle:

### Carpetas

* ./contracts: todos nuestros contratos inteligentes se almacenarán en esta carpeta.
* ./migrations: Los scripts de implementación se almacenarán en esta carpeta.
* ./test: Los scripts de prueba se almacenarán en esta carpeta.

### Archivos

* ./truffle-config: este es el archivo de configuración de Truffle. Podremos configurar redes, incluidas las redes RSK.
* ./contracts/Migrations.sol - realiza un seguimiento de las migraciones que se realizaron en la red actual.
* ./migrations/1_initial_migration.js - deploy Migrations.sol

Los otros archivos fueron creados para nuestro proyecto.

# Token.sol

Echa un vistazo al contrato inteligente `Token.sol`. Puede consultarlo en la carpeta `contracts`.

![Token.sol](/images/image-08.png)

> Token.sol tiene solo 7 líneas de codigo!

Déjame explicarte el código anterior.

Este contrato inteligente es un token ERC20 mintable.

Para crear nuestro Token ERC20, vamos a importar `ERC20Mintable` de Open Zeppelin. Esta biblioteca en sí misma importa varias otras bibliotecas como `SafeMath.sol`, los estándares para este tipo de token y la capacidad de acuñar tokens.

Dentro del token, definimos información básica sobre el token: `name` (nombre), `symbol` (símbolo) y número de `decimales` para la precisión.

Para heredar los atributos y funciones de la biblioteca, simplemente definimos nuestro contrato como un `ERC20Mintable` usando la palabra clave` is` de esta manera:

```javascript
contract Token is ERC20Mintable
```

## Personaliza tu token

Cambia el nombe y simbolo de tu token en las lineas 4 y 5

![Custom token](/images/image-09.png)

# Crear una billetera con Nifty Wallet

Nifty wallet es una especie de billetera web que facilita las transacciones con sus cuentas. También se puede utilizar con redes RSK porque ya está configurado para esto. Es una extensión de Chrome.

Ve a [Nifty wallet](https://www.poa.network/for-users/nifty-wallet), clickea en `Chrome extension` e instálalo.

![Nifty wallet install](/images/image-10.png)

Haz clic en `Add to Chrome`.![xxx](/images/image-11.png)

Vea el mensaje de instalación exitosa en la esquina superior izquierda:

![Nifty wallet installed](/images/image-12.png)

Acepta los Términos de uso

![Nifty wallet use terms](/images/image-13.png)

Elije una contraseña

![Nifty wallet password](/images/image-14.png)

## Mnemónico

Copie su mnemónico. 

> ¡Esta es la parte más importante! Si alguien conoce tu mnemónico, puedes robar tu billetera. Puedes olvidar la contraseña, pero la mnemotécnica nunca. Sin ella ya no accedes a tu billetera

![Mnemonic](/images/image-15.png)

Por ejemplo, mi mnemonic es: 

```
shoot gate legend member sniff asthma smart slot famous call marriage math
```

Espero que no robes mis RBTC de prueba :)

Arriba a la derecha, elija la red RSK Testnet

![Choose network](/images/image-16.png)

Ve la red RSK Testnet seleccionada

![Network selected](/images/image-17.png)

## Archivo .secret

En el terminal terminal, dentro de la carpeta del token, busques el archivo `.secret`. Si no existe, debes crearlo

Pegue su mnemónico en este archivo y guárdelo.

![File .secret](/images/image-18.png)

# TestNet Faucet

Puedes solicitar Testnet R-BTC en

[faucet.testnet.rsk.co](https://faucet.testnet.rsk.co/)

![Copy address from Nifty](/images/image-19.png)

Ingresa el address de tu billetera y completa el CAPTCHA.

![Wait a few seconds](/images/image-20.png)

Espera unos segundos ...

![Received some R-BTCs](/images/image-21.png)

Puedes ver el hash de la transacción, como este ejemplo:

[https://explorer.testnet.rsk.co/tx/0x16bedc1339a8fe59e270b0c6d5175851010bb93d0cf6c4974f1705b9ead7ee6e](https://explorer.testnet.rsk.co/tx/0x16bedc1339a8fe59e270b0c6d5175851010bb93d0cf6c4974f1705b9ead7ee6e)

¡Ahora tengo 0.05 tRBTC!

# Usando redes RSK

Esta Truffle box ya está configurada para conectarse a tres redes RSK: regtest (nodo local), testnet y mainnet. 

En este tutorial haremos la conexión a RSK Testnet

## Actualize el precio del gas

**Gas** es el precio interno para ejecutar una transacción o contrato. Cuando envía tokens, interactúa con un contrato, envía R-BTC o hace cualquier otra cosa en el Blockchain, debe pagar por ese acto. Ese pago se calcula como gas. En RSK, esto se paga en **R-BTC**.

El **mínimoPrecioGas** está escrito en el encabezado del bloque por los mineros y establece el precio mínimo del gas que debe tener una transacción para ser incluida en ese bloque.

Para actualizar **minimumGasPrice** en nuestro proyecto, ejecute esta consulta usando cURL:

**Testnet**

```shell
curl https://public-node.testnet.rsk.co/ -X POST -H "Content-Type: application/json" \
    --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["latest",false],"id":1}' \
    > .minimum-gas-price-testnet.json
```

Esta consulta guardó los detalles del último bloque en archivo `.minimum-gas-price-testnet.json`.

En `truffle-config.js`, estamos leyendo el parámetro `minimumGasPrice` en cada archivo json.

Para obtener más información sobre **Gas** y **minimumGasPrice**, vaya a la[ página de gas (en inglés](https://developers.rsk.co/rsk/rbtc/gas/) .

# Truffle console 

Truffle tiene una consola interactiva puede conectarse a cualquier red configurada en `truffle-config.js`. Esto es muy útil para compilar, implementar y interactuar con el contrato inteligente.

# Conéctese a red RSK Testnet

Ejecute la consola de desarrollo para cualquier red RSK.

```shell
# Console for Testnet
truffle console --network testnet
```

Mira en Testnet:

![truffle console --network testnet](/images/image-22.png)

# Pruebas la conexión a la red RSK

En cualquiera de las redes, ejecute estos comandos en la consola Truffle:

### Block number

Muestra el último número de bloque.

```javascript
(await web3.eth.getBlockNumber()).toString()
```

### Network ID

Para obtener la identificación de la red, ejecute este comando:

```javascript
(await web3.eth.net.getId()).toString()
```

Para el TestNet, el ID de red es "31".

![Testnet getId](/images/image-23.png)

Lista de network IDs:

* mainnet: 30
* testnet: 31
* regtest (local node): 33

## Obtenga nuestra direcciones

Escribamos esto en Truffle console para saber qué direcciones se desbloquearan con nuestro mnemónico:

```javascript
const accounts = await web3.eth.getAccounts()
```

No se preocupe por la devolución `undefined`, está bien. Vea las direcciones que le siguen ingresando el siguiente comando:

```javascript
accounts
```

También se puede mirar cada cuenta:

```javascript
accounts[0]
accounts[1]
```

Mira los resultados:

![accounts](/images/image-24.png)

Copie la dirección de la primera cuenta para obtener R-BTC

Por ejemplo, mi dirección es `0x4417CdC73f53573999F3A0397eE9df560a2f0507`. Copie isto.

## Consulte el saldo de R-BTCs

Ahora, revisemos nuestro saldo en la consola. Ejecute este comando en truffle console:

```javascript
web3.eth.getBalance(accounts[0], (err, res) => console.log(res))
```

¿Recuerdas que ya obtuvimos algunos RBTC en el Faucet? Mira:

![getBalance](/images/image-25.png)

## Salida de Truffle console

En la consola Truffle, ingrese este comando para salir de la terminal:

```shell
.exit
```

# Compile y migre los contratos inteligentes

Lo haremos ejecutando los siguientes comandos directamente en la terminal, sin usar el Truffle console, esto es para mostrarte una alternativa.

En cualquiera de las redes, ejecute este comando en una terminal (no en Truffle console):

```shell
truffle migrate --network testnet
```

> A veces, Testnet está congestionada y el proceso lleva tiempo. Puede detener el proceso con `CTRL + C` e iniciarlo de nuevo. 
> La migración de Truffle continuará donde la dejó.

# Interactuar con el token en Truffle Console

## Conéctese a red RSK Testnet de nuevo

```shell
truffle console --network testnet
```

## Obtenga sus cuentas en Truffle console

En Truffle console, escriba:

```javascript
const accounts = await web3.eth.getAccounts()
```

## Conéctate con tu token

Vamos a crear una variable `token` para almacenar la instancia de nuestro Token publicado.

En Truffle console, escriba:

```javascript
const token = await Token.deployed()
```

Confirma si nuestra instancia está bien.

Ingresa el nombre de la instancia: `token`, luego` .`, sin espacio, presione el botón TAB dos veces para activar la función de autocompletar como se ve a continuación.

```javascript
token. [TAB] [TAB]
```

Esto mostrará la dirección publicada del contrato inteligente y el hash de la transacción para su implementación, entre otras cosas, incluidas todas las variables públicas y los métodos disponibles.

![token instance](/images/image-26.png)

## Verificar el total supply

Para comprobar si ya tenemos tokens acuñados, llame a la función `totalSupply`:

```javascript
(await token.totalSupply()).toString()
```

El valor devuelto es 0, lo que se esperaba, ya que no realizamos ninguna acuñación inicial cuando implementamos el token.

![totalSupply](/images/image-27.png)

## Consulta el saldo del token

Para ver el saldo de una cuenta, llame a la función `balanceOf`. 
Por ejemplo, para mirar el saldo de la cuenta 0:

```javascript
(await token.balanceOf(accounts[0])).toString()
```

El valor devuelto también es 0, lo que se esperaba, ya que no hicimos ninguna acuñación inicial cuando implementamos el token y, por definición, ninguna cuenta puede tener ningún token todavía.

![balanceOf](/images/image-28.png)

## Emita tokens

Ejecute este comando:

```javascript
token.mint(accounts[0], 10000)
```

Este comando envió una transacción para acuñar 100 tokens para la cuenta 0.

![mint](/images/image-29.png)

También puede ceder a una dirección específica, `0xa52515946DAABe072f446Cc014a4eaA93fb9Fd79`:

```javascript
token.mint("0xa52515946DAABe072f446Cc014a4eaA93fb9Fd79", 10000)
```

![mint address](/images/image-30.png)

## Mira el saldo del token de nuevo

Vamos a verificar nuevamente el saldo de la cuenta 0:

```javascript
(await token.balanceOf(accounts[0])).toString()
```

El valor devuelto es 10000, que es 100 con 2 decimales de precisión. Esto es exactamente lo que esperábamos, ya que emitimos 100 tokens.

Además, puede obtener el saldo de una dirección específica, por ejemplo, `0xa52515946DAABe072f446Cc014a4eaA93fb9Fd79`:

```javascript
(await token.balanceOf("0xa52515946DAABe072f446Cc014a4eaA93fb9Fd79")).toString()
```

Mira los resultados:

![token balances](/images/image-31.png)

## Verifica el total supply de nuevo

Verifique el suministro total nuevamente:

```javascript
(await token.totalSupply()).toString()
```

El valor devuelto es 20000, que es 200 con 2 decimales de precisión.

Después de acuñar 100 fichas para 2 cuentas, ¡es perfecto!

![totalSupply after](/images/image-32.png)

## Haz la transferencia de tokens

Vamos a transferir 40 tokens de la cuenta 0 a la cuenta 2. Esto se puede hacer llamando a la función `transfer`.

```javascript
token.transfer(accounts[2], 4000, {from: accounts[0]})
```

![token transfer](/images/image-33.png)

La cuenta 2 no tenía tokens antes de la transferencia y ahora debería tener 40. 
La cuenta 1 debe tener 60 tokens. 
Además, el suministro total será el mismo.

Revisemos el saldo de cada cuenta y el suministro total:

```javascript
(await token.balanceOf(accounts[2])).toString()
(await token.balanceOf(accounts[0])).toString()
(await token.totalSupply()).toString()
```

Mira los resultados:

![token transfer results](/images/image-34.png)

## Obtenga la dirección del token

Es importante guardar la dirección del token para que pueda interactuar con él más tarde.

Ejecute este comando:

```shell
token.address
```

Por ejemplo, mi dirección es `0xc3c75d9A5e1ce5d1b9E7501775De5B138330B5BB`

![token.address](/images/image-35.png)

# Interactuar con el Token usando Nifty Wallet

Busquemos nuestro token en Nifty Wallet

Va a Tokens

![Nifty wallet tab token](/images/image-36.png)

Haz click in Add Token

![Nifty wallet Add Token](/images/image-37.png)

Seleccione Custom

![Nifty wallet Custom](/images/image-38.png)

Pega la dirección de nuestro token. 

Por ejemplo, mi dirección es `0xc3c75d9A5e1ce5d1b9E7501775De5B138330B5BB`.

Si se encuentra el token, se mostrarán su símbolo y posiciones decimales de tu token personalizado

![Nifty wallet our token](/images/image-39.png)

Haz click in Add

## Saldo de tokens

Como ya hicimos un problema en la consola Truffle, ¡veremos que ya tenemos tokens!

![Nifty wallet token balance](/images/image-40.png)

## Enviando tokens

Haz click en el tres puntos al lazo izquierdo del token Y elija Send

![Nifty wallet send tokens](/images/image-41.png)

Elija una dirección y envíe tokens.

Por ejemplo, voy a enviar 40 tokens a `0xF032f4259C315CeAcA0B44b4057E4CB4a33b43EE`

![Nifty wallet send tokens address](/images/image-42.png)

Haz click in Submit para confirmar la transacción

![Nifty wallet submit transaction](/images/image-43.png)

Ahora mira la transacción en la Tab `Sent`

![Nifty wallet transaction](/images/image-44.png)

Listo!

![Nifty wallet transaction confirmed](/images/image-45.png)

Puede consultar el saldo en la cuenta de destino en el explorador:

[https://explorer.testnet.rsk.co/address/0xf032f4259c315ceaca0b44b4057e4cb4a33b43ee?__tab=tokens](https://explorer.testnet.rsk.co/address/0xf032f4259c315ceaca0b44b4057e4cb4a33b43ee?__tab=tokens)

![Send token transaction in explorer](/images/image-46.png)

¡Muy bien! ¡Hicimos una transferencia de token usando Nifty Wallet!

# Consideraciones finales

En este momento, hemos instalado todos los requisitos y creado un token utilizando Truffle framework y la biblioteca de contratos inteligentes Open Zeppelin, conectado a RSK Testnet y después interactuamos con el token en Nifty Wallet."

### Encuentra más documentación (en inglés)

Vá a [RSK developers portal](https://developers.rsk.co/)

### ¿Tienes preguntas?

Pregunte en el [chat de RSK](https://gitter.im/rsksmart/getting-started)

Ask in the [RSK chat]([https://gitter.im/rsksmart/getting-started](https://gitter.im/rsksmart/getting-started)). Puedes preguntar en español!

Espero que este tutorial haya sido útil y agradezco tus comentarios. Compártelo si te gusta :)