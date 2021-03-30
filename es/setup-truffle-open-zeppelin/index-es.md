---
title: Cómo crear un proyecto usando Truffle y OpenZeppelin conectado a la red RSK
date: "2020-10-19T20:00:00.000Z"
description: "Cómo crear un proyecto utilizando el framework Truffle y la biblioteca de contratos inteligentes Open Zeppelin conectados tanto a un nodo local como a la red de prueba RSK."
tags: tutorial, rsk, openzeppelin, smart-contracts, truffle
type: blog
---

![Title](/images/image-es-00.png)

En este tutorial, te mostraré paso a paso cómo crear un nuevo proyecto de Blockchain, utilizando el framework Truffle y la biblioteca de contratos inteligentes Open Zeppelin, conectados tanto a un nodo local como a la red de prueba RSK.

No importa si estás comenzando desde cero o si ya eres un desarrollador, no necesitas ser un experto para hacer este tutorial. 

# Overview

A continuación se muestra un resumen de los pasos que daremos para crear nuestro proyecto:

1. Instalar los requerimientos previos;
2. Ejecutar un nodo RSK local (regtest);
3. Iniciar un proyecto usando Truffle;
4. Instalar los contratos inteligentes de Open Zeppelin en el proyecto;
5. Instalar el HD wallet provider;
6. Crear un mnemónico para una billetera;
7. Configurar Truffle para conectarse a la red RSK;
10. Utilizar la consola Truffle;
11. Adquirir algunos tRBTC en el faucet;

# Requerimientos previos

* Git
* POSIX compliant shell
* Curl
* Java
* Node.js e NPM (Node Package Manager)
* Editor: Visual Studio Code (VSCode) u otro editor de tu elección
* Extensión VSCode para el language Solidity
* Truffle framework

## Git

Git es un sistema de control de versiones de código abierto. Algunos paquetes que instalaremos utilizan Git internamente para descargar las versiones correctas.

Además, en el sistema operativo (SO) Windows, Git instala un terminal POSIX, llamado `Git Bash`, que es el siguiente requisito previo.
Instala [Git para Windows](https://gitforwindows.org/), que incluye la terminal Git Bash. 

Este [tutorial on installing and using Git Bash](https://www.atlassian.com/git/tutorials/git-bash) (en inglés) también te puede ayudar.

## Terminal POSIX compatible

**Portable Operating System Interface (POSIX)** es una familia de estándares especificados por la IEEE Computer Society para mantener la compatibilidad entre sistemas operativos. 
POSIX define la interfaz de programación de aplicación (Application Programming Interface - API) para terminales de comando e interfaces de utilidades, para garantizar que haya compatibilidad entre diferentes variantes de Unix y otros sistemas operativos. 
Fuente: [Wikipedia](https://en.wikipedia.org/wiki/POSIX)

* Mac OSX y distribuciones de Linux: usa el terminal estándar
* Windows: utiliza el terminal `Git Bash`, instalado junto con `Git` en el paso anterior. Si usas el terminal estándar (cmd) o PowerShell, es posible que los comandos no funcionen correctamente
  
## cURL

Este es un sistema de comando generalmente instalado en tu sistema operativo.

Ejecuta el comando `curl --version` y si devuleve un error, [descarga curl](https://curl.haxx.se/download.html).

## Java

Se requiere Java para ejecutar el nodo RSKj.

Asegúrate de que ya tienes instalado el Java runtime:

```shell
java -version
```

Si devuelve una versión, significa que ya lo has instalado antes.

![java -version](/images/image-01.png)

Si necesitas instalarlo, ve a [Java Download](https://www.java.com/en/download/)

![Java Download](/images/image-02.png)

### Para Mac OSX y Linux 

Hay varias formas de hacer esto. SDKman es una de ellas, y permite la instalación y selección de diferentes versiones:

```shell
curl -s "https://get.sdkman.io/" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
# to get a filtered list of available java versions
sdk list java  | grep "8\." # copy a selection for use below

# install the version of java copied above
# (replace accordingly)
sdk install java 8.0.242.j9-adpt

# show installed versions, and switch to the selected one
# (replace accordingly)
sdk list java | grep installed
sdk use java 8.0.242.j9-adpt
java -version
```

## Node.js y NPM

Otra dependencia es NPM, que se instala con Node.js.

Para comprobar si Node.js y NPM ya están instalados, comprueba si los siguientes comandos funcionan en la terminal:

```shell
node --version
npm --version
```

![node and npm version](/images/image-03.png)

Ve a [Node.js](https://nodejs.org/en/) si necesitas instalarlos.

Si deseas tener más de una versión del Node.js instalada, usa el administrador de versiones del Node.js, llamado [nvm](https://github.com/nvm-sh/nvm).

## Editor de código fuente

Necesitamos algún editor de código, preferiblemente uno que destaque los lenguajes Solidity y Javascript.
En este tutorial estoy usando VSCode para crear los archivos del proyecto.

[VS Code](https://code.visualstudio.com/) es una buena opción.

### Visual Studio Code (VS Code)

Para instalarlo, [descárgalo aquí](https://code.visualstudio.com/download).

Comprueba si la instalación del VS Code está bien consultando su versión en el terminal:

```shell
code -v
```

![vscode version](/images/image-04.png)

## Extensión VSCode para el lenguaje Solidity

Si usas VSCode, ve a Extensions (Menu View -> Extensions).

1. Escribe `solidity` en el campo de búsqueda.
2. Selecciona la extensión "solidity" de Juan Blanco.
3. Haz clic en `install`.

![vscode version](/images/image-42.png)

## Truffle framework

[Truffle](https://www.trufflesuite.com/truffle) es un framework bien conocido para el desarrollo de contratos inteligentes, que facilita la vida del desarrollador.
Entre sus características, podemos mencionar: la gestión de la "vida" de un contrato inteligente (se pueden realizar varias publicaciones y averiguar cuál fue la última), el desarrollo de scripts para despliegue, pruebas automatizadas y una gestión simplificada de red.

De la misma forma, también le facilita el trabajo al desarrollador RSK porque podemos configurar redes RSK en Truffle.

Para instalar Truffle en la terminal, escribe el siguiente comando y presiona la tecla `enter`:

```shell
npm install -g truffle
```

![truffle install](/images/image-05.png)

Cuando se complete la instalación, cierra la ventana de la terminal y ábrela nuevamente para verificar la versión de Truffle:

```shell
truffle version
```

![truffle version](/images/image-06.png)

# Nodo local RSK - regtest

Cuando desarrollamos un proyecto utilizando el framework Truffle, necesitamos una cadena de bloques que se ejecute localmente. Esta es la mejor forma de desarrollar proyectos y ejecutar pruebas. Ejecutaremos un nodo local en la red RSK, también conocido como regtest.

Hay varias formas de instalar / configurar un nodo RSK. Aquí descargaremos un archivo JAR y lo ejecutaremos con el SDK de Java ya instalado.

### Download 

Ve a la página de [releases](https://github.com/rsksmart/rskj/releases) y haz clic en la última versión para descargar.

Debes seleccionar el archivo JAR, que se encuentra al final de la publicación de la última versión.
Su nombre es algo así como `rskj-core-*.jar`:

![Download last RSK release](/images/image-07.png)

### Verifica la autenticidad

Al instalar y ejecutar un nodo RSKj, se recomienda verificar que su copia sea legítima.

En el directorio donde se guardó el archivo JAR, ve a una terminal POSIX y ejecuta este comando:

```shell
sha256sum rskj-core-2.0.1-PAPYRUS-all.jar
```

Para esta versión, por ejemplo, el resultado será este:

```shell
43149abce0a737341a0b063f2016a1e73dae19b8af8f2e54657326ac8eedc8a0 *rskj-core-2.0.1-PAPYRUS-all.jar
```

![Verify authenticity](/images/image-08.png)

> Si estás utilizando SO Windows, necesitas una terminal POSIX.
> Chequea las instrucciones sobre [Git Bash](#shell-compatível-posix) arriba. 

Para obtener más información sobre cómo verificar que su copia sea original,
incluyendo la verificación de firmas, mira las [instrucciones en inglés](https://developers.rsk.co/rsk/node/security-chain/ "Verify authenticity of RskJ source code and its binary dependencies").

### Ejecución

Para ejecutar el nodo:

```shell
java -cp <PATH-TO-THE-RSKJ-JAR> -Drpc.providers.web.cors=* co.rsk.Start --regtest
```

(Cambia `<PATH-TO-THE-RSKJ-JAR>` por la ubicación de tu archivo JAR).

Estoy usando el sistema operativo (SO) Windows y guardé el archivo JAR en `C:\RSK\node`,
entonces la ruta completa a mi archivo es
`C:\RSK\node\rskj-core-1.3.0-WASABI-all.jar`.

Entonces, para ejecutar el nodo RSK:

#### Windows

```shell
java -cp C:\RSK\node\rskj-core-2.0.1-PAPYRUS-all.jar -Drpc.providers.web.cors=* co.rsk.Start --regtest
```

#### Usando Git Bash

```shell
java -cp C:/RSK/node/rskj-core-2.0.1-PAPYRUS-all.jar -Drpc.providers.web.cors=* co.rsk.Start --regtest
```

#### Linux ou Mac

```shell
java -cp ~/RSK/node/rskj-core-2.0.1-PAPYRUS-all.jar -Drpc.providers.web.cors=* co.rsk.Start --regtest
```

Si no surgió nada después de ejecutar el comando, en general significa que el nodo está funcionando perfectamente. El resultado se guarda en el archivo de log.

> Realizaremos las consultas JSON-RPC desde una DApp en un navegador, entonces necesitamos activar una opción que deshabilita la protección para compartir a diferentes fuentes, haciendo que cualquier página pueda acceder al nodo.
> 
> Para eso usamos el parámetro: `-Drpc.providers.web.cors=*` 

Este es el resultado en la terminal de Windows:

![Run local node](/images/image-09.png)

**Importante:**

> No cierres la ventana de la terminal / consola.
> El nodo se está ejecutando en esta ventana y, si la cierras, finalizará la ejecución.

### Verifica que el nodo esté funcionando usando cURL

Abre una nueva ventana de la terminal.

Consulta el servidor HTTP RPC del nodo. Este es un ejemplo usando cURL:

```shell
curl localhost:4444/1.1.0/ -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

![local node eth_blockNumber](/images/image-10.png)

La respuesta será algo como:

```json
{"jsonrpc":"2.0","id":1,"result":"0x2991b"}
```

La propiedad `result` devuelve el número del último bloque sincronizado.
En nuestro caso, que es un nodo local aislado, es el último bloque minado.
Ten en cuenta que el valor `0x2991b` está en hexadecimal (base 16). En el ejemplo anterior, el número de bloque es "170267" en decimal (base 10).

Para saber más:
[Setup RSKj with Java](https://developers.rsk.co/rsk/node/install/java/)

Si tienes algún problema, verifica si tu sistema cumple con los requisitos previos [minimum requirements](https://developers.rsk.co/rsk/node/install/requirements/).

Hay otras formas de instalar un nodo RSK, en otras plataformas: [installing RSKj](https://developers.rsk.co/rsk/node/install/)

# RSK Testnet - comprobando la conexión

Además del nodo local, también publicaremos contratos inteligentes en la red de prueba RSK. Antes de eso, verifiquemos si la conexión está bien.

Este es un ejemplo que usa cURL. Ejecute el siguiente comando en la terminal.

Si está utilizando una computadora con sistema operativo Windows, no funciona en la terminal, mi sugerencia es usar Git Bash. Para instalar, ve al sitio de [Git](https://git-scm.com/).

```shell
curl https://public-node.testnet.rsk.co/2.0.1/ -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

Esta consulta devuelve el número del último bloque extraído.

La respuesta será algo como:

```json
{"jsonrpc":"2.0","id":1,"result":"0xc3f9b"}
```

![testnet eth_blockNumber](/images/image-11.png)

El campo `result` se presenta en formato hexadecimal. `0xc3f9b` es el número de bloque y su equivalente decimal es` 802715`.

Puedes consultar el [testnet explorer](https://explorer.testnet.rsk.co/) y validar que es el mismo número de bloque:

![explorer testnet block number](/images/image-12.png)

# Iniciar un proyecto Truffle

Hay dos opciones para iniciar un proyecto: 

1. Una plantilla de proyecto vacía
2. Un proyecto basado en Truffle Box

## 1 - Inicialize un nuevo proyecto Truffle 

Crea un nuevo directorio, `myproject`, por ejemplo, y ve a la carpeta en la terminal.

```shell
mkdir myproject
cd myproject
```

Por ejemplo, crearé en `C:\RSK\` (estoy usando Windows).

Mi proyecto se ubicará en el directorio `C:\RSK\myproject`.

![myproject folder](/images/image-13.png)

En la carpeta de su proyecto, inicie un proyecto Truffle:

```shell
truffle init
```

![truffle init](/images/image-14.png)

Abre la carpeta en VSCode.
Verá una estructura de directorio como esta:

![truffle file structure](/images/image-15.png)

* `./contracts`: Todos los contratos inteligentes se guardarán en esta carpeta.
* `./migrations`: Los scripts para publicación se almacenarán aquí. 
* `./test`: Aquí se guardarán los scripts de prueba.
* `./truffle-config.js`: Este es el archivo de configuración de Truffle. Aquí configuraremos las redes, incluida RSK.

Tenga en cuenta que también se han creado los siguientes archivos:

* `Migrations.sol`: Smart contract que registra todas las publicaciones realizadas en una red.
* `1_initial_migration.js`: Script para publicación de `Migrations.sol`.

### Inicialize un proyecto npm

Cuando inicializamos un proyecto Truffle desde el template, también necesitamos inicializar un proyecto npm.

Para iniciar un proyecto npm en la carpeta `myproject`, ejecute el siguiente comando en la terminal:

```shell
npm init -y
```

![npm init](/images/image-16.png)

## 2 - Inicialize um proyecto basado en un Truffle Box

> Solo necesita hacer esta parte si no eligió la opción 1.

Truffle Boxes son modelos.
Además de los archivos Truffle,
Truffle Boxes pueden contener otros módulos útiles, como contratos inteligentes Solidity, bibliotecas, páginas front-end y más.

En la opción 1, cuando usamos `truffle init`, estamos usando un tipo especial de Truffle box.
Consulte otros [boxes](https://www.trufflesuite.com/boxes).

También tenemos algunos configurados para RSK: [RSK truffle boxes] (https://developers.rsk.co/tutorials/truffle-boxes/) (en inglés).

## Instale Open Zeppelin

[OpenZeppelin Contracts](https://openzeppelin.com/contracts/) es un conjunto de bibliotecas para contratos inteligentes desarrollados en Solidity. También funcionan en otras cadenas de bloques, como **RSK**.

Cabe mencionar que estas bibliotecas fueron revisadas y auditadas con miras a altos estándares de seguridad, por lo que los contratos que dependen de ellas son menos susceptibles a los piratas informáticos cuando se usan correctamente.

En la terminal, en el directorio `myproject`, instale las bibliotecas OpenZeppelin con este comando:

```shell
npm install -E @openzeppelin/contracts@2.5.0
```

La opción `-E` es para guardar las dependencias en la versión definida en la instalación en el archivo de configuración npm, y no con la versión default.

![openzeppelin install](/images/image-17.png)

> Los contratos inteligentes se pueden cambiar de una versión a otra, por lo que es importante corregir la versión porque nuestro tutorial fue escrito usando esta versión.

# Instale el HD wallet provider

Para conectarnos a la red RSK, usaremos un paquete de proveedor que hace posible conectarse a cualquier red desbloqueando una cuenta localmente.
Usaremos [@truffle/hdwallet-provider](https://www.npmjs.com/package/@truffle/hdwallet-provider).
Este proveedor permite firmar transacciones para direcciones generadas a partir de un mnemónico con 12 o 24 palabras.

> Debes haber instalado Node >= 7.6.

En la terminal, en la carpeta `myproject`, instale con este comando:

```shell
npm install -E @truffle/hdwallet-provider@1.0.34
```

![hd wallet provider install](/images/image-18.png)

Este `truffle package` tiene muchas dependencias.
Puede que el mensaje `successful installation` tarde un poco en aparecer

![hd wallet provider successful installation](/images/image-19.png)

# Revisa el archivo package.json

`package.json` es un archivo de configuración que creamos al inicio del proyecto, con el comando `npm init -y`. 

Después de la instalación, en la carpeta `Token`, abra el archivo `package.json` en VSCode y verifique la parte de dependencias (dependencies):

![package.json](/images/image-20.png)

# Crea una billetera

Para usar testnet, necesitamos tRBTC y una dirección / billetera para almacenarlos.
La mejor manera de crear una billetera es a partir de un mnemónico, utilizando el patrón definido en
[BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)

Hay varias formas de hacer esto.

Uno es crear con una billetera web, como [Metamask](https://metamask.io/) o [Nifty](https://www.poa.network/for-users/nifty-wallet). 
Estas carteras generan el mnemónico para ti.
Si desea crear utilizando Metamask, consulte las instrucciones aquí:

- [Remix y Metamask en RSK testnet](https://solange.dev/2020/rsk-first-smart-contract/)

## iancoleman.io/bip39

Otra forma es con esta aplicación web: 

[iancoleman.io/bip39](https://iancoleman.io/bip39/)

> En este tutorial, el método utilizado para guardar el mnemónico no se recomienda para una billetera con fondos reales, que valen dinero real, porque no es tan seguro generar un mnemónico y claves privadas en un sitio web.
> Para fines educativos, podemos usarlo aquí, ya que nos conectaremos a la red de prueba.

En el campo `Generate a random mnemonic`, elija `12 words` y haga clic en el botón `generate`.

![Generate a random mnemonic](/images/image-21.png)

O resultado estará no campo `BIP39 Mnemonic`. 
Hay 12 palabras al azar, como en la siguiente figura:

![BIP39 Mnemonic](/images/image-22.png)

Mi mnemónico es: 

```
energy knife ice mouse merge track cram brown decorate atom rule virus
```

Copia tus 12 palabras para usarlas en breve.

## Herramienta mnemonics

Otra alternativa es utilizar el paquete [mnemonics](https://github.com/itinance/mnemonics),
que es una utilidad simple para generar mnemónicos [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki).

Para instalar `mnemonics` globalmente, escriba el siguiente comando en la terminal y presione la tecla `enter` en la carpeta de su proyecto:

```shell
npm install -g mnemonics@1.1.3
```

Utilice este comando para crear un mnemónico BIP39 de 12 palabras:

```shell
mnemonics > .secret
```

El nuevo mnemónico se guarda en el archivo `.secret`, que es el siguiente paso.

# Crie o arquivo .secret

En la terminal, en el directorio `myproject`, crea un archivo llamado` .secret`.

¿Recuerdas tu mnemónico?
Pegue su mnemónico en este archivo y guárdelo.

![dot secret](/images/image-23.png)

# Configures Truffle para conectarse a redes RSK

Abra el archivo `truffle-config.js` en su proyecto Truffle y sobrescríbalo con estas instrucciones:

```javascript
const HDWalletProvider = require('@truffle/hdwallet-provider');

const fs = require('fs');
const mnemonic = fs.readFileSync(".secret").toString().trim();
if (!mnemonic || mnemonic.split(' ').length !== 12) {
  throw new Error('unable to retrieve mnemonic from .secret');
}

module.exports = {
  networks: {
  },
  compilers: {
    solc: {
    }
  }
}
```

`hdwallet-provider` nos permite conectarnos con cualquier red, desbloqueando una cuenta localmente, usando el mnemónico. Esto incluye redes RSK.

También estamos leyendo el mnemónico almacenado en el archivo `.secret` y guardándolo en la variable mnemonic.

## Configures Truffle para conectarse a RSK regtest (nodo local)

No archivo `truffle-config.js`, agregue esta configuración en la sección `network`:

```javascript
    development: {
      host: "127.0.0.1",
      port: 4444,
      network_id: "*"
    },  
```

Este es el resultado:

![network development](/images/image-24.png)

## Verifique o gas price en testnet

Consulte el valor actual del precio del gas para la red testnet, e guárdelo en el archivo 
`.gas-price-testnet.json`. 

Desde el directorio del proyecto, ejecute este comando cURL:

```shell
curl https://public-node.testnet.rsk.co/2.0.1/ -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":1}' > .gas-price-testnet.json
```

![gas price result](/images/image-25.png)

Recibirá una devolución que se ve así en el archivo:

```json
{"jsonrpc":"2.0","id":1,"result":"0x3938700"}
```

![gas-price-testnet.json](/images/image-26.png)

El resultado se presenta en formato hexadecimal.

Cambie el archivo `truffle-config` nuevamente para incluir la actualización del gas price.
Después de la parte mnemónico, incluya estas instrucciones:


```javascript
const gasPriceTestnetRaw = fs.readFileSync(".gas-price-testnet.json").toString().trim();
const gasPriceTestnet = parseInt(JSON.parse(gasPriceTestnetRaw).result, 16);
if (typeof gasPriceTestnet !== 'number' || isNaN(gasPriceTestnet)) {
  throw new Error('unable to retrieve network gas price from .gas-price-testnet.json');
}
console.log("Gas price Testnet: " + gasPriceTestnet);
```

## Configure Truffle para conectarse a RSK testnet

Em el archivo `truffle-config.js`, incluya esta configuración en la sección `network`:

```javascript
    testnet: {
      provider: () => new HDWalletProvider(mnemonic, 'https://public-node.testnet.rsk.co/2.0.1/'),
      network_id: 31,
      gasPrice: Math.floor(gasPriceTestnet * 1.1),
      networkCheckTimeout: 1e9
    },
```

## Truffle configurado para RSK testnet y regtest (local nodo)

Esta es la versión final del archivo `truffle-config.js` con la configuración de las dos redes:

```javascript
const HDWalletProvider = require('@truffle/hdwallet-provider');
 
const fs = require('fs');
const mnemonic = fs.readFileSync(".secret").toString().trim();
if (!mnemonic || mnemonic.split(' ').length !== 12) {
  throw new Error('unable to retrieve mnemonic from .secret');
}
 
const gasPriceTestnetRaw = fs.readFileSync(".gas-price-testnet.json").toString().trim();
const gasPriceTestnet = parseInt(JSON.parse(gasPriceTestnetRaw).result, 16);
if (typeof gasPriceTestnet !== 'number' || isNaN(gasPriceTestnet)) {
  throw new Error('unable to retrieve network gas price from .gas-price-testnet.json');
}
console.log("Gas price Testnet: " + gasPriceTestnet);
 
module.exports = {
  networks: {
    development: {
      host: "127.0.0.1",
      port: 4444,
      network_id: "*"
    },
    testnet: {
      provider: () => new HDWalletProvider(mnemonic, 'https://public-node.testnet.rsk.co/2.0.1/'),
      network_id: 31,
      gasPrice: Math.floor(gasPriceTestnet * 1.1),
      networkCheckTimeout: 1e9
    },        
  },
  compilers: {
    solc: {
    }
  }
}
```

Mira la imagen de VS Code:

![truffle-config](/images/image-27.png)

# Truffle Console conectado a la red RSK

Truffle tiene su propia consola para ejecutar comandos y se puede conectar a cualquier red previamente configurada en el archivo `truffle-config.js`.

## Conectarse al nodo local RSK (regtest)

Abriremos la consola de Truffle para conectarnos al nodo local.

En la terminal, en la carpeta `myproject`, ejecute este comando:

```shell
truffle console
```

![Truffle console development](/images/image-28.png)

> Cualquier red definida con el nombre `development` se considerará la red predeterminada.

## Conectarse a testnet RSK 

Hasta ahora, nos hemos conectado a una cadena de bloques que funciona con solo 1 nodo y que se ejecuta en su propia computadora.
¡Interactuaremos con una cadena de bloques "real", que se ejecuta en múltiples nodos distribuidos en múltiples computadoras!

Para conectar la consola Truffle a otra red, debe especificar la red:

En la terminal, en la carpeta `myproject`, ejecute este comando:

```shell
truffle console --network testnet
```

Se tarda un poco más en establecer la conexión, si se compara con el nodo local.
Se abrirá una nueva consola:

![truffle console network testnet](/images/image-29.png)

## Pruebes la conexión a la red RSK

En cualquiera de las redes, ejecute este comando en la consola Truffle:

### Block number

Muestra el número del último bloque minado.

```javascript
(await web3.eth.getBlockNumber()).toString()
```

![getBlockNumber](/images/image-30.png)

### Network ID

Para encontrar la identificación de la red, ejecute este comando:

```javascript
(await web3.eth.net.getId()).toString()
```

Para el nodo local, el ID de rede es `33`.

![getId local](/images/image-31.png)

Y para testnet, es `31`.

![getId testnet](/images/image-32.png)

## Salir de la consola Truffle

En la consola Truffle, ejecute este comando para salir de la terminal:

```shell
.exit
```

![exit Truffle console](/images/image-33.png)

# Conozca las direcciones de su billetera

Ejecutaremos un comando en la consola Truffle para recuperar las primeras 10 direcciones de nuestra HD (hierarchical deterministic) wallet para RSK Testnet, que se genera a partir de nuestro mnemónico.

En la terminal, en la carpeta `myproject`, vaya a la consola Truffle conectada a testnet:

```shell
truffle console --network testnet
```

Y ejecute este comando para guardar las direcciones en la variable `accounts`:

```javascript
const accounts = Object.keys(web3.currentProvider.wallets)
```

Vea las direcciones con este comando:

```javascript
accounts
```

![list accounts](/images/image-34.png)

Guardemos las direcciones en un archivo llamado `.accounts`

```javascript
await require('fs').promises.writeFile('.accounts', accounts.join('\n'))
```

![create file .accounts](/images/image-35.png)

Ahora podemos ver el archivo:

![file .accounts](/images/image-36.png)

## Consultar saldo

Para verificar el saldo de una cuenta, por ejemplo, de la primera cuenta en nuestra lista 
(`account[0]`), ejecute este comando en la consola Truffle:

```javascript
(await web3.eth.getBalance(accounts[0])).toString()
```

![getBalance accounts 0](/images/image-37.png)

El saldo es 0 y necesitamos tRBTC para pagar el gas utilizado para publicar contratos inteligentes e interactuar con ellos. Los obtendremos en el siguiente paso.

# TestNet Faucet

Puede recibir Testnet RBTC en

[RSK Testnet faucet](https://faucet.testnet.rsk.co/).

Copia la primera dirección del archivo `.accounts`. En mi caso es:

```
0xe16f6abdd5815f3d24b4e5c29138f863933b000a
```

Pega tu dirección (que fue copiada en el paso anterior) y haz la verificación CAPTCHA.

![faucet.testnet.rsk.co](/images/image-38.png)

Espera unos segundos ...

![Wait a few seconds](/images/image-39.png)

![Received some R-BTCs](/images/image-40.png)

Puede ver el hash de la transacción, por ejemplo, hice esta:

[0x4a2bf1f65c525219020c3a1215a29453c20f4ced90575d9a7d13f8fe666d05b4](https://explorer.testnet.rsk.co/tx/0x4a2bf1f65c525219020c3a1215a29453c20f4ced90575d9a7d13f8fe666d05b4)

¡Y ahora tengo 0.05 tRBTC!

## Volver a comprobar el saldo

Verifique el saldo de su cuenta nuevamente. Ejecute este comando en la consola Truffle:

```javascript
(await web3.eth.getBalance(accounts[0])).toString()
```

![getBalance accounts 0 again](/images/image-41.png)

Ahora tengo 50000000000000000, lo que significa 0,05 con 18 decimales de precisión.

# Próximos pasos

En este punto, hemos instalado todos los requisitos previos y hemos creado una plantilla de proyecto vacía, utilizando el Truffle framework y la biblioteca de contratos inteligentes Open Zeppelin, conectado tanto a un nodo local (Regtest) como a la red de prueba RSK.

Aún no hemos creado nada, pero estás listo para pasar a los próximos tutoriales, ¡donde haremos algunos proyectos realmente interesantes!
