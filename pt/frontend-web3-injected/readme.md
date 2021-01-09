# Como criar seu primeiro frontend para smart contracts

Como criar seu primeiro frontend para smart contracts utilizando como provider web3 injected e se conectando com um smart contract publicado na RSK testnet utilizando Metamask.

![Title](/images/image-00.png)

Neste tutorial mostrarei passo a passo como criar seu primeiro frontend para um smart contract publicado na RSK testnet, utilizando apenas Javascript e HTML, conectado a wallet Metamask utilizando como provider web3 injected. 

# Overview

Aqui está um resumo das etapas que faremos para construir nosso frontend:

1. Configurar Metamask para se conectar a testnet RSK;
2. Adquirir alguns testnet R-BTCs no faucet;
3. Conectar Remix com a RSK Testnet;
4. Criar, compilar e publicar um smart contract na RSK testnet utilizando Remix;
5. Inicializar um projeto;
6. Instalar web3.js; 
7. Criar um arquivo javascript;
8. Criar um arquivo html;
9. Instalar e executar um servidor local;
10. Interagir com o smart contract; 

As etapas 1 a 4 são explicadas detalhadamente no tutorial abaixo: 

* [Crie seu primeiro smart contract utilizando Remix e Metamask com a RSK testnet](/2020/2020-03-27-Rsk-RemixMetamask/)

## Video

Caso prefira assistir um video, eu ministrei um workshop online sobre este tutorial:
<a href="https://www.youtube.com/watch?v=RVXB2NgGODU" target="_blank"> clique aqui</a>

## Requirements

* Metamask
* Remix - ferramenta online
* Node.js and NPM (Node Package Manager)
* Visual Studio Code (VSCode) ou outro editor de sua escolha
* HTTP server: express
* web3.js

Como já mencionado, instalar Metamask e conectar a RSK testnet, além de conectar Remix com a RSK Testnet estão detalhados neste tutorial: 

* [Crie seu primeiro smart contract utilizando Remix e Metamask com a RSK testnet](/2020/2020-03-27-Rsk-RemixMetamask/)

## Node.js e NPM

Precisamos do Node.js e NPM, eles são instalados juntos.

Pra verificar se Node.js e NPM já estão instalados, verifique se os comandos abaixo funcionam no terminal:

```shell
node --version
npm --version
```

![node and npm version](/images/image-01.png)

Vá em [Node.js](https://nodejs.org/en/) caso precise instalá-los.

## Visual Studio Code (VSCode)

Neste tutorial estou utilizando VSCode para criar os arquivos do projeto.

Para instalar, [faça o download aqui](https://code.visualstudio.com/download).

Verifique se a instalação do VS code está ok consultando sua versão no terminal:

```shell
code -v
```

![visual code version](/images/image-02.png)

# Criando o projeto Register

Crie uma nova pasta chamada Register, e inicialize um projeto nesta pasta executando os comandos abaixo no terminal:

```shell
mkdir Register
cd Register
npm init -y
```

Por exemplo, eu vou criar a pasta neste local: `C:\RSK\`

Então meu projeto pode ser localizado no diretório: `C:\RSK\Register`.

![Register project](/images/image-03.png)

## Express

Express é uma aplicação Node.js, um framework web que facilita o desenvolvimento de aplicações web. 
Ele é um servidor HTTP minimalista.

Para instalar Express, digite o comando abaixo no terminal e pressione a tecla `enter`, na pasta do seu projeto:

```shell
npm install express --save
```

Mais informações: 

- [http://expressjs.com/](http://expressjs.com/)

- [https://www.npmjs.com/package/express](https://www.npmjs.com/package/express)

## Web3.js

Web3.js auxilia o desenvolvimento de websites ou clients que interagem com o blockchain. 

A biblioteca web3.js é uma API Javascript Ethereum que faz as conexões utilizando a especificação JSON-RPC. Como a máquina virtual da RSK é compatível com a Ethereum Virtual Machine (EVM), é possível utilizar web3.js para realizar a comunicação entre o front-end e um nó.

Para instalar web3.js, na pasta do seu projeto, digite o comando abaixo no terminal e pressione `enter`:

```shell
npm install web3 --save
```

Mais informações: 

- [https://web3js.readthedocs.io/](https://web3js.readthedocs.io/)

## Verifique o arquivo package.json

`package.json` é um arquivo de configurações que criamos na inicialização do projeto, com o comando `npm init -y`. 

Depois das instalações, na pasta Register, abra o arquivo `package.json` no VSCode, e verifique a parte das dependências (dependencies):

![package.json](/images/image-04.png)

Serão encontrados os pacotes instalados anteriormente:

```json
 "dependencies": {
   "express": "^4.17.1",
   "web3": "^1.2.6"
 }
```

# Publicação do smart contract

Este item é explicado passo-a-passo no tutorial: 

* [Crie seu primeiro smart contract utilizando Remix e Metamask com a RSK testnet](/2020/2020-03-27-Rsk-RemixMetamask/)

Vou replicar aqui apenas os comandos

### Remix

Vá em [http://remix.ethereum.org/](http://remix.ethereum.org/)

## Crie um smart contract

Crie o arquivo `Register.sol` 

Copie e cole o smart contract deste link ou abaixo:

[register.sol](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-web3-injected/register.sol)

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

Este smart contract tem:

* A variable `info`, privada, para armazenar uma string
* A function `getInfo()` para retornar a string armazenada na variável `info`
* A function `setInfo()` para alterar a string armazenada na variável `info`

## Compilar o smart contract

Vá no 3o botão a esquerda - Solidity compiler e clique no botão Compile `Register.sol`.

### Deploy a smart contract 

No painel a esquerda, vá no item Deploy and run transactions. 

Em `Environment`, confirme se está selecionada a opção `Injected Web3`, isto avisa o Remix para utilizar o Web3 provider injetado por um plugin de navegador, como o MetaMask ou Nifty wallet.

Depois clique no botão `Deploy`.

![Deploy and run transactions](/images/image-05.png)

Depois que o smart contract for publicado com o Remix, podemos ver sua instância no painel a esquerda, abaixo do item `deploy and run transactions`. 

Clique no botão `copy` que está localizado do lado direito do smart contract para copiar o endereço do mesmo. Ele será utilizado no frontend.

![Copy](/images/image-06.png)

![smart contract address](/images/image-07.png)

No meu exemplo, o endereço do contrato é `0xc864D0fef177A69aFa8E302A1b90e450910A4c3E`.

# Aplicação cliente - o frontend

Agora vamos construir o frontend que fará a interação com nosso smart contract.

Teremos apenas 2 arquivos:

1. index.html
2. index.js

## Index.html

No diretório Register, crie um arquivo chamado `index.html`.

Copie e cole o conteúdo do seguinte link, ou o código fonte abaixo:

[https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-web3-injected/index.html](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-web3-injected/index.html)

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

Na pasta `Register`, crie o arquivo `index.js`.

Copie e cole o conteúdo do seguinte link, ou o código fonte abaixo:

[https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-web3-injected/index.js](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-web3-injected/index.js)


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

Esta parte faz a conexão com o nó RSK utilizando a wallet injetada, no nosso caso, Metamask:

```javascript
// web3 provider with fallback for old version
if (window.ethereum) {
  window.web3 = new Web3(window.ethereum)
  try {
      // ask user for permission
      ethereum.enable()
      // user approved permission
  } catch (error) {
      // user rejected permission
      console.log('user rejected permission')
  }
}
else if (window.web3) {
  window.web3 = new Web3(window.web3.currentProvider)
  // no need to ask for permission
}
else {
  window.alert('Non-Ethereum browser detected. You should consider trying MetaMask!')
}
```

# Atualize o index.js

Você lembra do endereço do smart contract que copiamos depois da publicação?

Ele será atualizado aqui:

```javascript
var contractAddress = '0xc864D0fef177A69aFa8E302A1b90e450910A4c3E';
```

# HTML server

No diretório `Register`, crie o arquivo `server.js`.

```javascript
var express = require('express');
var app = express();
app.use(express.static(__dirname));
app.listen('3300');
console.log('Running at\nhttp://localhost:3300');
```

Este arquivo configura o servidor HTML express.

## Executando

A última etapa é executar o servidor express. Digite o seguinte comando no terminal.

```shell
node server.js
```

![node server.js](/images/image-08.png)

Em seu browser, vá no endereço:

```shell
[http://localhost:3300](http://localhost:3300)
```

![register frontend](/images/image-09.png)

## Interagindo com o smart contract

### Get

Clique no botão get

Isto vai chamar a função `getInfo()` no smart contract register.

![getInfo](/images/image-10.png)

Não temos nenhuma informação armazenada porque não definimos nada quando fizemos a publicação.

### Set

Digite algo no campo info e clique no botão set.

Isto vai chamar a função `setInfo()` no smart contract register, passando como parâmetro o que você digitou no campo info.

Vou escrever "RSK".

![setInfo](/images/image-11.png)

Espere alguns segundos para a transação ser incluída em um bloco e minerada...

### getInfo (again)

Agora temos a informação "RSK" salva e podemos consultá-la.

Clique no botão get de novo

![getInfo again](/images/image-12.png)

E o retorno será a informação "RSK".

Muito bom! Agora temos uma informação armazenada no nosso smart contract, e podemos consultá-la!

### Observação

> Fizemos o deploy, em uma rede pública, de um smart contract com uma função pública, que faz alterações em uma váriavel, sem nenhuma restrição de quem pode chamá-la. Aqui isto é um exemplo didático.
> 
> Em um smart contract para o mundo real, é importante definir quem pode executar as funções que alteram o estado do smart contract.

# Parabéns!

Você construiu e publicou sua primeira aplicação descentralizada - decentralized application (DApp) com smart contracts na RSK! 

Você pode fazer download do código fonte deste tutorial aqui:

[https://github.com/solangegueiros/dapp-register-rsk/tree/master/register-rsk-web3-injected](https://github.com/solangegueiros/dapp-register-rsk/tree/master/register-rsk-web3-injected)

Tem algo a dizer sobre este tutorial? Agradeço seu feedback. Compartilhe o artigo caso tenha gostado :)

Se quiser ver outros videos, assine meu canal: 
<a href="https://www.youtube.com/user/solangegueiros" target="_blank"> youtube Solange Gueiros</a>
