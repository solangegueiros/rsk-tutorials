---
title: Utilização da biblioteca rsk3.js no seu site com webpack
date: "2020-08-19T19:00:00.000Z"
description: "Aprenda como utilizar a biblioteca rsk3.js em un frontend para se comunicar com os smart contracts publicados na RSK, através do webpack."
tags: tutorial, rsk, smart-contracts, web3, rsk3
type: blog
---

![Title](/images/image-00.png)

Neste tutorial eu ensinarei passo-a-passo como utilizar a biblioteca rsk3.js em un frontend para se comunicar com os smart contracts publicados na RSK. Utilizaremos a conexão em um nó local da Blockchain RSK. A biblioteca será integrada ao seu projeto através do webpack.

Este tutorial é continuação do [Como conectar o Remix a um nó local da rede RSK](https://solange.dev/2020/rsk-remix-local-node/).

# Overview

Aqui está um resumo das etapas que faremos:

1. Instalar pré requisitos;
2. Publicar um smart contract;
3. Conhecer o endereço do smart contract;
3. Criar um projeto;
4. Instalar as dependências do projeto;
5. Criar o frontend;
6. Configurar e executar um HTML server;
7. Interagir com o nó e o smart contract através do frontend;

## Video

Caso prefira assistir um video, eu ministrei um
<a href="https://www.youtube.com/watch?v=txpJ7ybiqJw" target="_blank">webinar</a>
sobre este tutorial.

# Pré-requisitos

1. Execução de um nó RSK local
2. Remix
3. Editor: Visual Studio Code (VSCode) ou outro editor de sua escolha

## 1. Execução de um nó RSK local

Veja o tutorial [Como executar um nó local do Blockchain RSK](https://solange.dev/2020/rsk-local-node/)

## 2. Remix

Remix é uma ferramenta online. É um IDE (Integrated Development Environment - ambiente de desenvolvimento integrado) usado para escrever, compilar, publicar e depurar código fonte em Solidity. Pode ser conectado diretamente ao nó local e, com essa conexão, utilizado para publicar contratos inteligentes no Blockchain RSK localmente.

[remix.ethereum.org](http://remix.ethereum.org/)

> Atenção: utilizaremos a conexão http e não https!

## 3. Visual Studio Code (VSCode)

Neste tutorial estou utilizando [VS Code](https://code.visualstudio.com/) para criar os arquivos do projeto.

Para instalar, [faça o download aqui](https://code.visualstudio.com/download).

# Smart contract

Utilizaremos o [smart contract Register](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-rsk3-webpack/register.sol), já conhecido de tutoriais anteriores:

```javascript
pragma solidity 0.5.16;

contract Register {
    string private info;
    
    function getInfo() public view returns (string memory) {
        return info;
    }
    
    function setInfo(string memory _info) public {
        info = _info;
    }    
}
```

## Register.sol

Este smart contract tem:

* A variable `info`, privada, para armazenar uma string
* A function `getInfo()` para retornar a string armazenada na variável `info`
* A function `setInfo()` para alterar a string armazenada na variável `info`

## Publique um smart contract na rede local

Existem algumas maneiras de fazer isto. Uma delas é utilizando o Remix. 

O tutorial [Como conectar o Remix a um nó local da rede RSK](https://solange.dev/2020/rsk-remix-local-node/) já fez a publicação do smart contract `Register.sol` na rede local e aqui faremos um frontend para ele.

## Endereço do smart contract

Para interagir com um smart contract, precisamos saber 2 coisas:
1. ABI
2. Endereço

A ABI - Application Binary Interface declara todas as comunicações públicas com nosso smart contract, que podem ser as funções definidas, juntamente com seus parâmetros de entrada e de saída, e também as variáveis públicas.

E é necessário saber o endereço de um smart contract para interagir com ele. Vou utilizar o endereço do smart contract publicado no tutorial [Como conectar o Remix a um nó local da rede RSK](https://solange.dev/2020/rsk-remix-local-node/)

Na minha publicação, o endereço é `0xb5a9b7dA0ffcD59fDE4987Ff361eFB981Bb819F7`.

# Crie o projeto rsk3dapp

1. Crie uma nova pasta chamada `rsk3dapp` e vá para ela.
2. Inicialize um projeto npm;

São estes os comandos:

```shell
mkdir rsk3dapp
cd rsk3dapp
npm init -y
```

Por exemplo, eu vou criar a pasta neste local: `C:\RSK\`

Então meu projeto pode ser localizado no diretório: `C:\RSK\rsk3dapp`.

![Register project](/images/image-01.png)

# Instale as dependências do projeto

Nosso projeto utilizará estes pacotes npm:
1. rsk3.js;
2. express;
3. webpack

## Rsk3.js

A biblioteca [rsk3](https://developers.rsk.co/libraries/rsk3js/docs/) é utilizada para interagir com nós de blockchain RSK locais ou remotos. Ela é uma API Javascript que faz as conexões utilizando a especificação JSON-RPC. Esta é a forma de comunicação da RVM (RSK Virtual Machine), então é possível realizar a comunicação entre o front-end e um nó utilizando a a rsk3.

Quem já conhece a biblioteca Ethereum `web3`, vai se sentir familiarizado, dado que a maioria das chamadas de função tem uma sintaxe muito semelhante às do web3.js, porém focadas no blockchain RSK.

Para instalar o [pacote npm rsk3.js](https://www.npmjs.com/package/@rsksmart/rsk3), na pasta do seu projeto, digite o comando abaixo no terminal e pressione `enter`:

```shell
npm install @rsksmart/rsk3
```

## Express

[Express](http://expressjs.com/) é uma aplicação Node.js, um framework web que facilita o desenvolvimento de aplicações web. 
Ele é um servidor HTTP minimalista.

Para instalar o [pacote npm Express](https://www.npmjs.com/package/express), digite o comando abaixo no terminal e pressione a tecla `enter`, na pasta do seu projeto:

```shell
npm install express
```

## Webpack

[Webpack](https://webpack.js.org/) é um empacotador de módulos para aplicações javascript. Além do javascript, ele também pode incluir outros tipos de arquivos como css, font, image, HTML e etc. A biblioteca rsk3 será integrada ao seu projeto juntamente com as suas chamadas através do webpack.

No terminal, no diretório do projeto, vamos instalar os pacotes npm `webpack` e `webpack-cli`, com este comando:

```shell
npm i -D webpack webpack-cli 
```

A opção `-D` salva no arquivo de configuração npm as dependências apenas para o ambiente de desenvolvimento.

## Verifique o arquivo package.json

`package.json` é um arquivo de configurações que criamos na inicialização do projeto, com o comando `npm init -y`. 

Depois das instalações, na pasta do projeto, abra o arquivo `package.json` no VSCode, e verifique a parte das dependências (dependencies):

![package.json](/images/image-02.png)

Serão encontrados os pacotes instalados anteriormente:

```json
  "dependencies": {
    "@rsksmart/rsk3": "^0.3.4",
    "express": "^4.17.1"
  },
  "devDependencies": {
    "webpack": "^4.44.1",
    "webpack-cli": "^3.3.12"
  }
```

# Aplicação cliente - o frontend

Agora vamos construir o frontend que fará a interação com nosso smart contract.

Teremos apenas 3 arquivos:

1. index.js
2. index.html
3. webpack.config.js

## index.js

Na pasta do projeto, crie uma nova pasta chamada `src`.
Dentro dela, crie o arquivo `index.js`.

Copie e cole o conteúdo do seguinte link, ou o código fonte abaixo:

[index.js](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-rsk3-webpack/src/index.js)

```javascript
import Rsk3 from '@rsksmart/rsk3';

//connection with node
const rsk3 = new Rsk3(new Rsk3.providers.HttpProvider("http://localhost:4444"));

// contractAddress and abi are setted after contract deploy
var contractAddress = '0x8D547c1A322C4fE727B9947a97903d4622Fd2b88';
var abi = JSON.parse( '[{"constant":true,"inputs":[],"name":"getInfo","outputs":[{"internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"string","name":"_info","type":"string"}],"name":"setInfo","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"}]' );

//contract instance
let contract = new rsk3.Contract(abi, contractAddress);
console.log('Contract: ' + contract.address);

// Accounts
let account;
rsk3.getAccounts(function(err, accounts) {
  if (err == true) {
    console.log('Account Error: ' + err);
    alert("Error: get accounts.");
    return;
  }
  if (accounts.length == 0) {
    alert("No account found! Check if the node is configured properly.");
    return;
  }
  account = accounts[0];
  console.log('Account: ' + account);
  rsk3.defaultAccount = account;
});

//Smart contract functions
  
function registerGetInfo() {
  contract.methods.getInfo().call().then( function( info ) { 
    console.log("getInfo: ", info);
    document.getElementById('lastInfo').innerHTML = info;
  });    
}

function registerSetInfo() {  
  var element = document.getElementById('newInfo_input');
  //console.log("element: ", element);
  var info = element.value;
  console.log("newInfo: ", info);
  contract.methods.setInfo (info).send( {from: account}).then( function(tx) { 
    console.log("Transaction: ", tx); 
  });
  element.value = "";
}

//Blockchain function
async function fetchLatestBlockHeight() {
  const blockHeight = await rsk3.getBlockNumber();
  console.log("blockHeight: ", blockHeight);
  const element = document.getElementById('lastBlock');
  element.innerText = `Latest Block: ${blockHeight}`;
}

function main() {
  const blockHeightButton = document.getElementById('block_height_button');
  blockHeightButton.onclick = fetchLatestBlockHeight;

  const registerGetInfoButton = document.getElementById('registerGetInfo_button');
  registerGetInfoButton.onclick = registerGetInfo;

  const registerSetInfoButton = document.getElementById('registerSetInfo_button');
  registerSetInfoButton.onclick = registerSetInfo;
}

main();
```

Este arquivo faz a importação da biblioteca rsk3.js e a chamada das funções do smart contract, utilizando a biblioteca. Ele será utilizado pelo webpack para criar o arquivo final que será carregado no frontend.

## Atualize o index.js

Você lembra do endereço do smart contract que copiamos depois da publicação?

Ele será atualizado aqui:

```javascript
var contractAddress = '0xb5a9b7dA0ffcD59fDE4987Ff361eFB981Bb819F7';
```

![Register address](/images/image-03.png)

## index.html

No diretório do projeto, crie um arquivo chamado `index.html`.

Copie e cole o conteúdo do seguinte link, ou o código fonte abaixo:

[index.html](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-rsk3-webpack/index.html)

```html
<!DOCTYPE html>
<html >
  <head>
    <title>RSK3 js demo Register</title>
    <link href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css' rel='stylesheet' type='text/css'>
    <script src="https://code.jquery.com/jquery-3.1.1.slim.min.js"></script>
  </head>

  <body class="container">

    <h1 class="page-header">Register information at Blockchain - RSK3.js</h1>

    <div class="row">
      <button id="block_height_button" type="button" class="btn btn-primary">Latest block</button>
      <label id="lastBlock"></label>
      <br>
    </div>  

    <div class="row">
      <div>
        <h3 class="sub-header">Get last information saved</h3>        
        <div>
          <table>
            <tr>
              <td>
                <button id="registerGetInfo_button" type="button" class="btn btn-primary">Get</button>
              </td>              
              <td>Info:</td>
              <td>
                <label id="lastInfo">
              </td>
            </tr>
          </table>
        </div>
      </div>
    </div>    

    <div class="row">
      <div>
        <h3 class="sub-header">Set information</h3>
        <div>
          <table>
            <tr>
              <td><label for="newInfo">Info:</label> </td>
              <td>
                <input class="form-control" id="newInfo_input">
              </td>
              <td>
                <button id="registerSetInfo_button" type="button" class="btn btn-primary">Set</button>
              </td>
            </tr>
          </table>
        </div>
      </div>
    </div>    
    
  </body>

  <script src="./dist/main.js"></script>
</html>
```

No final do arquivo `index.html`, é carregado o arquivo `./dist/main.js`. Este arquivo será gerado pelo webpack, juntamente com a criação da pasta `dist`.

## webpack.config.js

Este é o arquivo de configuração do webpack. 

- `entry` - determina onde o webpack deve iniciar a construção do seu grafo de dependências. `./src/index.js` é o entry padrão do webpack 4.
- `output` - determina onde o webpack deve salvar os pacotes que cria e como os nomeia. `./dist/main.js` é o output padrão do webpack 4.

No diretório do projeto, crie um arquivo chamado `webpack.config.js`.

Copie e cole o conteúdo do seguinte link, ou o código fonte abaixo:

[webpack.config.js](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-rsk3-webpack/webpack.config.js)

```javascript
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'main.js',
        path: path.resolve(__dirname, 'dist'),
    },
};
```

## Criando o main.js

Vamos utilizar o webpack para criar o arquivo `main.js` na pasta `dist`, conforme foi definido no arquivo de configuração do passo anterior.

Na pasta do projeto, execute o comando:

```shell
npm webpack
```

Verifique que foi criado o diretório `dist` e o arquivo `main.js`

# HTML server

No diretório do projeto, crie o arquivo `server.js`.

Copie e cole o conteúdo do seguinte link, ou o código fonte abaixo:

[server.js](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-rsk3-webpack/server.js)

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

![node server.js](/images/image-04.png)

Em seu browser, vá no endereço:

```shell
[http://localhost:3300](http://localhost:3300)
```

![http://localhost:3300](/images/image-05.png)

# Interagindo com o nó

Clique no botão `Latest block`

Isto vai chamar a função `getBlockNumber()` que retorna o último bloco minerado.

![Latest block](/images/image-06.png)

# Interagindo com o smart contract

## Get

Clique no botão get

Isto vai chamar a função `getInfo()` no smart contract register.

Este tutorial é continuação de [como conectar o Remix a um nó local da rede RSK](https://solange.dev/2020/rsk-remix-local-node/). No final do tutorial anterior registramos a informação `RSK` no smart contract.

![getInfo](/images/image-07.png)

## Set

Digite algo no campo info e clique no botão set.

Isto vai chamar a função `setInfo()` no smart contract register, passando como parâmetro o que você digitou no campo info.

Vou escrever "Sol".

![setInfo](/images/image-08.png)

Espere alguns segundos para a transação ser validada e incluída em um bloco...

### getInfo (again)

Agora temos a informação "Sol" registrada e podemos consultá-la.

Clique no botão get de novo

![getInfo again](/images/image-09.png)

E o retorno será a informação "Sol".

Muito bom! Agora temos uma informação armazenada em nosso smart contract, e podemos consultá-la!

### Inspect - console

Dica: quando estamos desenvolvendo um frontend, ajuda bastante se abrirmos o console.

![browser right button](/images/image-10.png)

1. Clique com o botão direito na página
2. Escolha `Inspect`
3. Caso não esteja selecionada, encontre a tab `Console`

![browser console](/images/image-11.png)

Veja que já foram programados alguns retornos no console:

![console output](/images/image-12.png)

# Parabéns!

Você construiu e publicou um site que se comunica com seu smart contract na RSK! 

Você pode fazer download do código fonte deste tutorial aqui:

[register-rsk-rsk3-webpack](https://github.com/solangegueiros/dapp-register-rsk/tree/master/register-rsk-rsk3-webpack).

Em breve você aprenderá a fazer o mesmo para uma rede pública. Será publicado outro tutorial para a testnet RSK, aguarde :)

Tem algo a dizer sobre este tutorial? Agradeço seu feedback. Compartilhe o artigo caso tenha gostado :)

Se quiser ver outros videos, assine meu canal: 
<a href="https://www.youtube.com/user/solangegueiros" target="_blank"> youtube Solange Gueiros</a>
