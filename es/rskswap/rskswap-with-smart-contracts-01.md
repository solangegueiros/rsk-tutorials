# Cómo usar RSKSwap con smart contracts

intercambiar tokens en RSKSwap

# Resumen

A continuación se muestra un resumen de los pasos que vamos a seguir para crear tu primer smart contract en RSK usando Remix y la billetera Metamask:

1. Conexión de Metamask con RSK testnet;
2. Solicitar testnet R-BTCs en faucet;
3. Conexión de Remix con RSK testnet; 
4. Crear un token;
5. 


# Requisitos

- Metamask - extensión de google chrome 
- Remix - herramienta web, online

## Metamask

Metamask es una especie de billetera web que facilita las transacciones con sus cuentas. También se puede utilizar con redes RSK. Tiene versiones para varios navegadores, como Chrome, Firefox, Opera y Brave.

Ve a [metamask.io](metamask.io) e instálalo.

Crea una cuenta.

Escribe tu frase inicial, seed phrase, mnemonic o frase de respaldo (todos estos términos significan lo mismo), con 12 palabras. Se utiliza para recuperar tu cuenta, en caso de que pierdas tu contraseña.

> ¡La seed phrase es lo más importante en una billetera / cuenta!


## Remix

Remix es una herramienta web online. Es un IDE (Integrated Development Environment = Entorno de Desarrollo Integrado) que se utiliza para escribir, compilar, implementar y depurar código Solidity. Puede conectarse con Metamask y usarse para implementar contratos inteligentes tanto en RSK Testnet como en Mainnet.

Se puede acceder en:

[http://remix.ethereum.org/](http://remix.ethereum.org/)


# Conexión de Metamask con RSK testnet

- Ve a networks 
- Selecciona Custom RPC

![networks - custom RPC](/images/image-01.png)

- Network Name

RSK Testnet

- New RPC URL

[https://public-node.testnet.rsk.co](https://public-node.testnet.rsk.co)

- ChainID (optional)

31

- Symbol (optional)

tR-BTC

- Block Explorer URL (optional)

https://explorer.testnet.rsk.co

![RSK Testnet configuration](/images/image-02.png)

Después de configurarlo, cierre la ventana de configuración y selecciona RSK Tesnet.

# TestNet Faucet

Puedes solicitar Testnet R-BTC en

[faucet.testnet.rsk.co](https://faucet.testnet.rsk.co/)

Copia tu address de Metamask

![Copy address from Metamask](/images/image-03.png)

Ingresa el address de tu billetera y completa el CAPTCHA.

Espera unos segundos ...

![Wait a few seconds](/images/image-04.png)

![Received some R-BTCs](/images/image-05.png)

Puedes ver el hash de la transacción, como este ejemplo:

[https://explorer.testnet.rsk.co/tx/0xf63c45dabd52e0b44f4cf15825985e9ddfe790b4323a88a3531f762a417f9011](https://explorer.testnet.rsk.co/tx/0xf63c45dabd52e0b44f4cf15825985e9ddfe790b4323a88a3531f762a417f9011)

¡Ahora tengo 0.05 tR-BTC!

![R-BTCs at Metamask wallet](/images/image-06.png)

¡Puedes usar el faucet solo una vez al día!

![Faucet once a day](/images/image-07.png)


# Remix

Ve a 

[http://remix.ethereum.org/](http://remix.ethereum.org/)

![remix.ethereum.org](/images/image-08.png)

En la página de inicio / bienvenida (home / welcome), elige environment Solidity

![Remix environment Solidity](/images/image-09.png)

## Terminal

En Remix, en la parte de abajo, a la derecha, hay una terminal con algunas bibliotecas disponibles.

Puedes enviar comandos y transacciones aquí. Esta área también presenta el resultado de transacciones y / o llamadas a funciones de contrato inteligente.

¡Esta zona de retorno es muy importante para mirar los resultados!

![remix terminal](/images/image-10.png)

## Compilador Solidity

En el tercer botón en el lado izquierdo, haz clic en Solidity compiler

![remix solidity compiler](/images/image-11.png)

Es útil habilitar la compilación automática (auto-compile),  para compilar contratos inteligentes automáticamente al editar en Remix.

![auto-compile](/images/image-12.png)

# Conexión de Remix con RSK testnet

Con la red RSK testnet seleccionada en Metamask ...

En Remix, en el lado izquierdo, ubica el botón Deploy and run transactions (Implementar y ejecutar transacciones).

Por ahora es el cuarto botón

![Deploy and run transactions](/images/image-13.png)

En Environment, elige 

Injected Web3

![Injected Web3](/images/image-14.png)

Injected Web3 conecta Remix con una cuenta activa en Metamask

Se abrirá una ventana popup de Metamask para autorizar la conexión.

Primero seleccione la cuenta y haz clic en botón Next. Probablemente solo tengas una.

![Metamask select accounts](/images/image-15.png)

Después haz clic en botón Connect.

![Metamask connect](/images/image-16.png)

En Remix, en Deploy and run transactions, mira el Custom Network

![Injected Web3 - ChainID 31](/images/image-17.png)

ChainID 31 se definió en la red personalizada RSK Testnet en Metamask.

# Crea un smart contract

Crea un archivo nuevo

Haz clic en el segundo botón en el lado izquierdo - file explorer 

![file explorer](/images/image-18.png)

Haz clic en + para crear un archivo nuevo

![create a new file](/images/image-19.png)

File name: SolToken.sol


Copia este ejemplo:

```shell
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

Y pégalo aquí:

![Paste Register.sol](/images/image-21.png)


# Compila el token

Si habilitó la compilación automática (auto-compile), el contrato inteligente ya está compilado y aparecerá una luz verde junto al tercer botón de la izquierda: Solidity compiler.

Si no lo ha habilitado:

* Haz clic en el tercer botón de la izquierda: Solidity compiler

* Haz clic en el botón Compilar Register.sol

* Verifica el letrero verde en el tercer botón con el mensaje: compilation successful

![compilation successful](/images/image-22.png)


# Deploy de un contrato inteligente en RSK testnet

En el panel lateral izquierdo, ve al botón Deploy and run transactions.

Por ahora es el cuarto botón.

![Deploy and run transactions](/images/image-23.png)

En este momento solo tenemos un contrato inteligente, por lo que se selecciona automáticamente en el dropbox

![Select Register.sol](/images/image-24.png)

Haz clic en el botón Deploy

Se abrirá una ventana popup de Metamask para confirmar la transacción para crear el contrato inteligente Register.sol

![Deploy](/images/image-25.png)

Haz clic en confirm

En la parte inferior derecha, podemos ver el mensaje.

creation of Register pending...

![creation of SimpleStorage pending](/images/image-26.png)

![transaction confirmed](/images/image-27.png)

Una vez confirmado lo podemos comprobar.

![xxx](/images/image-28.png)

Haz clic en la línea de transacción o el botón debug (en el lado derecho) para ver más detalles de la transacción.

Copia el hash de la transacción para verificarlo en el Blockchain explorer

Por ejemplo, el hash de la transacción es

0x419c4b17ec0bf59568d9b5f5c7f0e4678039f52b9c644c2914ccd0bd2bb331da

# RSK - UniswapV2Router02

RSK - UniswapV2Router02 testnet / mainnet
0xf55c496bb1058690DB1401c4b9C19F3f44374961

## Approve Router to Spent Token



