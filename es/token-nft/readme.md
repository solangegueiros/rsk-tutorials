# Crea tu NFT: token coleccionable en la red RSK

Cómo crear tokens coleccionables en la red RSK, utilizando el framework Truffle y los contratos inteligentes Open Zeppelin. 
Además, crearemos una interfaz de React para interactuar con el token ERC721 publicado en RSK testnet a través de Metamask.

![Title](/images/image-00.png)

En este tutorial aprenderá a programar Blockchain desde cero, creando una aplicación descentralizada completa - dApp, paso a paso.
Aprenderá a crear tokens coleccionables en RSK Blockchain usando framework Truffle, los contratos inteligentes de Zeppelin y la creación de una interfaz React usando `create-react-app`.

Crearemos una dApp inspirada en [Cryptokitties](https://www.cryptokitties.co/), un juego de Blockchain pupular donde puedes recolectar y crear gatos digitales.
En este tutorial, en lugar de coleccionar felinos, en nuestra aplicación recolectaremos tokens de colores exclusivos.

### Fungible vs.infungible

Un token fungible representa un activo que se puede canjear por cualquier otro de igual valor en su clase.

El dinero es un ejemplo de token fungible.
Un billete de $ 100 es lo mismo que cualquier otro billete de $ 100,
puedes cambiarlos libremente porque tienen el mismo valor,
independientemente del número de serie de un billete específico de $ 100.
Son notas fungibles.

Por otro lado, un token infungible (Non-Fungible Token - NFT) es un token único, por lo tanto, los artículos coleccionables son activos infungibles y pueden estar representados por NFT.

### ERC-721

ERC-721 fue el primer estándar en representar tokens infungibles,
y hasta el día de hoy es el más popular.

Las características más importantes de este tipo de activos es tener una forma de verificar quién es el propietario y una forma de transferir los activos.

Es fácil crear un contrato inteligente en el estándar ERC-721 importando contratos de la biblioteca de OpenZeppelin y lo haremos en este tutorial.

La interfaz para el estándar ERC-721 tiene dos métodos:

* `ownerOf`: para averiguar quién es el propietario del token
* `transferFrom`: transferir la propiedad de un token

¡Y esto es suficiente para representar un NFT!

### Colors collection

En este tutorial, crearemos un token infungible para representar nuestros colores coleccionables.

Podrá crear nuevos tokens de color y reclamarlos para que puedan almacenarse en una billetera blockchain.

# Resumen

A continuación, se muestra un resumen de los pasos que seguiremos para crear nuestro token:

1. Configurar los requisitos previos;
2. Inicializar el proyecto con Truffle y Open Zeppelin;
3. Configurar Truffle para conectarse a la red RSK;
4. Crear una billetera
5. Solicitar testnet R-BTCs en faucet;
6. Inicialice la interfaz;
7. Instalar RSK Truffle Token Box;
8. Agregue más configuraciones a Truffle;
9.  Crea el contrato inteligente del token y compile;
10. Prepárese para hacer deploy usando Truffle;
11. Hacer la publicación en la red de prueba RSK con Truffle;
12. Personaliza el frontend;
13. Interactuar con el token;

## Passos 1 a 4

Os passos 1 a 4 são explicados detalhadamente no tutorial: 

* [Como criar um projeto utilizando Truffle e OpenZeppelin conectado à rede RSK](pt/setup-truffle-open-zeppelin/readme.md)

# Pré-requisitos

1. Node.js e NPM (Node Package Manager)
2. Visual Studio Code (VSCode) ou outro editor à sua escolha
3. Truffle
4. Metamask - extensão para o google chrome

Os pré-requisitos 1 a 3 também são explicados detalhadamente no tutorial:

* [Como criar um projeto utilizando Truffle e OpenZeppelin conectado à rede RSK](pt/setup-truffle-open-zeppelin/readme.md)

O pré-requisito 4, instalar Metamask, conectar a testnet RSK e ganhar alguns tR-BTCs, está explicado passo a passo no tutorial: 

* [Remix and Metamask with RSK testnet](pt/remix-metamask/readme.md)

# Configure o projeto

Crie uma nova pasta chamada `colors`:

```shell
mkdir colors
cd colors
```

Na pasta `colors`, execute as etapas abaixo, seguindo as instruções do tutorial já citado anteriormente:
[Como criar um projeto utilizando Truffle e OpenZeppelin conectado à rede RSK](pt/setup-truffle-open-zeppelin/readme.md)

1. Inicialize um novo projeto Truffle;
2. Inicialize um projeto npm;
3. Instale Open Zeppelin;
4. Instale HD wallet provider;
5. Crie um mnemonic para uma carteira;
6. Crie o arquivo .secret;
7. Verifique o gas price na testnet;
8. Configure Truffle para se conectar à testnet RSK;
9. Use o console Truffle;
10. Teste a conexão a testnet RSK;
11. Saiba os endereços da sua wallet;
12. Consulte o saldo;
13. Ganhe alguns testnet R-BTCs no faucet;

# Initialize a aplicação client

Precisamos de 3 itens para construir o frontend:

1. Create React App
2. Web3.js
3. Bootstrap

## Create React App

Este é o template oficial para criar uma aplicação React de página única (single-page React application). Com ele faremos uma instalação sem precisar configurar mais nada. 

Para saber mais: [create react app](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app)

No diretório do projeto, no terminal, execute:

```
npx create-react-app app --use-npm
```

![create-react-app](/images/image-01.png)

A opção `--use-npm` é para escolher o npm como gerenciador de pacotes.

[npx](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b) está incluído com o npm versão 5.2+ ou superior, se precisar, veja [instruções para versões antigas de npm](https://gist.github.com/gaearon/4064d3c23a77c74a3614c498a8bb1c5f).

Este é um grande pacote e demora um pouco para apresentar a mensagem de instalação realizada com sucesso:

![create-react-app successful installation](/images/image-02.png)

Em nosso projeto, agora existe uma nova pasta chamada `app`. Nós customizaremos o frontend mais tarde, neste diretório.

![create-react-app file structure](/images/image-03.png)

## Web3.js

Web3.js auxilia o desenvolvimento de websites ou clients que interagem com o blockchain, com instruções para ler e escrever dados no Blockchain através dos smart contracts. 

A biblioteca web3.js é uma API Javascript Ethereum que faz as conexões utilizando a especificação JSON-RPC. Como a máquina virtual da RSK é compatível com a Ethereum Virtual Machine (EVM), é possível utilizar web3.js para realizar a comunicação entre o front-end e um nó.

Para instalar web3.js, na pasta `app`, digite o comando abaixo no terminal e pressione `enter`:

```shell
cd app
npm install -E web3@1.2.7
```

A opção `-E` é para salvar as dependências na versão especificada ao invés da versão padrão definida no npm.

![web3 install](/images/image-04.png)

![web3 successful installation](/images/image-05.png)

Mais informações: 

[web3.js](https://web3js.readthedocs.io/)

## Bootstrap

Também na pasta `app`, digite o comando abaixo no terminal e pressione `enter`:

```shell
cd app
npm install -E bootstrap@4.4.1
```

![web3 install](/images/image-06.png)

Como já dito anteriormente, a opção `-E` é para salvar as dependências na versão especificada ao invés da versão padrão definida no npm.

![web3 successful installation](/images/image-07.png)

# Configurando Truffle

Volte para o diretório do projeto, `colors`, abra o arquivo `truffle-config.js` no VS Code e sobrescreva como o seguinte código:

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

const path = require("path");

module.exports = {
  networks: {
    testnet: {
      provider: () => new HDWalletProvider(mnemonic, 'https://public-node.testnet.rsk.co', 0, 10, true, "m/44'/37310'/0'/0/"),
      network_id: 31,
      gasPrice: Math.floor(minimumGasPriceTestnet * 1.3),
      networkCheckTimeout: 1e6,
      timeoutBlocks: 100,
      deploymentPollingInterval: 15e3,
    },
  },

  contracts_build_directory: path.join(__dirname, "app/src/contracts"),

  compilers: {
    solc: {
      version: "0.5.7",
    }
  }
}
```

Ficará deste jeito (o códido está mais atualizado do que a imagem):

![truffle-config](/images/image-08.png)

## Sobre contracts_build_directory

Nós adicionamos a bibliteca `path` para utilizá-la com um novo parâmetro chamado `contracts_build_directory`, que define o local onde as informações dos contratos (contracts artifacts) são armazenadas, como a abi e os endereços de publicação nas diversas redes. 

Em nosso projeto, estará localizada em uma pasta diferente: `app/src/contracts`.

# Arquitetura dos smart contracts

Criaremos o smart contract `Color.sol` que vai herdar as definições do ERC721 que estão na biblioteca OZ.

Além das bibliotecas com padrões do token ERC721, Open Zeppellin importa outras para propriedade e controle de perfis, matemática e outros utilitários.

## Crie o smart contract Color.sol

No diretório `Contracts`, crie um arquivo chamado `Color.sol`.

![Create Color.sol](/images/image-09.png)

## arquivo Color.sol

Copie e cole o smart contract deste link, ou do código abaixo:

[Color.sol](https://raw.githubusercontent.com/solangegueiros/tokens-rsk/master/colors-create-react-app/contracts/Color.sol)

```javascript
pragma solidity 0.5.7;

import '@openzeppelin/contracts/token/ERC721/ERC721Full.sol';

contract Color is ERC721Full {

  bytes3[] public colors;
  mapping(bytes3 => bool) private _colorExists;

  constructor() ERC721Full("Color", "COLOR") public {
  }

  // E.G. color = "#FFFFFF"
  function mint(bytes3 _color) public {
    require(!_colorExists[_color], "color exists");
    uint _id = colors.push(_color);
    _mint(msg.sender, _id);
    _colorExists[_color] = true;
  }
}
```

Ficará assim:

![Color.sol](/images/image-10.png)

## Entendendo o smart contract

Para criar nosso token ERC721, importaremos o `ERC721Full` do Open Zeppelin. 
Esta biblioteca importa algumas outras, como `SafeMath.sol`, 
o padrão para o ERC721 e algumas características extras, como metadata e a listagem de tokens. 

Com o metadata podemos customizar nosso token definindo um nome e símbolo para ele no construtor. 

A função constructor é executada apenas uma vez, no momento em que o smart contract é publicado no blockchain.

No construtor estamos chamando a função constructor do smart contract `ERC721Full` e passando como argumentos o nome `Color` e símbolo `COLOR`.

O gerenciamento de cores é executado com a variável `colors`, que é um array de cores, além da váriável `_colorExists`, que é um índice para saber se uma cor já foi emitida.

Também temos uma função para criar novos tokens de cores. 
Está função recebe um argumento do tipo bytes3, que é um número hexadecimal que corresponde a cor do token que será criado. 

Por exemplo, para criar um token verde, 
passe como argumento "#00FF00" quando chamar a função.
Ou se quiser criar um token vermelho, use "#FF0000".

# Compile o smart contract

No terminal, execute este comando:

```
truffle compile
```

![truffle compile](/images/image-11.png)

# Publique o smart contract na testnet

Primeiro é necessário criar um arquivo na estrutura do Truffle com instruções para publicar o smart contract.

## Crie o arquivo 2_deploy_contracts.js

O diretório `migrations` contém arquivos JavaScript que auxiliam a publicação de contratos no blockchain. 
Estes arquivos são responsáveis pelo preparo das tarefas de implantação e são escritos com base no pressuposto de que suas necessidades de implantação podem mudar com o tempo. 
I histórico das publicações executadas anteriormente é salvo no blockchain através de um contrato especial criado automaticamente pelo Truffle, chamado Migrations. 
(source: [running migrations](https://www.trufflesuite.com/docs/truffle/getting-started/running-migrations))

Na pasta `migrations`, crie o arquivo `2_deploy_contracts.js`

![create 2_deploy_contracts](/images/image-12.png)

Copie e cole este código:

```javascript
const Color = artifacts.require("Color");

module.exports = function(deployer) {
  deployer.deploy(Color);
};
```

![2_deploy_contracts](/images/image-13.png)

## Migrate

No terminal, execute este comando:

```
truffle migrate --network testnet
```

Aguarde alguns minutos para que as transações da publicação do smart contract sejam incluídas no blockchain.

O comando migrate pode compilar o smart contract de novo, se for necessário.

![truffle migrate compile](/images/image-14.png)

Primeiro é publicado o smart contract `Migrations.sol`, arquivo gerado pelo Truffle:

![truffle migrate Migrations.sol](/images/image-15.png)

Está é a transação da minha publicação na RSK testnet:

[0x3de61b8983dc3db2ca21a9d10106a19c445885fcb7040774bd6937daf94a4702](https://explorer.testnet.rsk.co/tx/0x3de61b8983dc3db2ca21a9d10106a19c445885fcb7040774bd6937daf94a4702)

Em seguida, é publicado nosso smart contract `Color.sol`:

![truffle migrate Color.sol](/images/image-16.png)

Aqui está a transação na RSK testnet:

[0x2c2d2932a7d637fbba100b5c482c1fa1899c4fe24bd1a458976a93cee6c5ba85](https://explorer.testnet.rsk.co/tx/0x2c2d2932a7d637fbba100b5c482c1fa1899c4fe24bd1a458976a93cee6c5ba85)

> Uma dica: se acontecer algum problema de comunicação com a testnet entre a publicação do Migrations.sol e do Color.sol, apenas execute o comando migrate novamente, que ele executa o deploy apenas do que estiver faltando.

**Parabéns!**

Nosso NFT Color está publicado na RSK Testnet.

Salve o endereço do contrato do token, ele poderá ser útil depois: 

```javascript
tokenAddress = "0x5505a54a8F3e63D37095c37d9f8AcF0f4900B61F"
```

# Aplicação client

Agora vamos construir o frontend que vai interagir com o smart contract. 
Com ele será possível criar novos tokens de cores e listar todos os tokens que existem na sua carteira.

Na pasta `app`, vamos customizar alguns arquivos.

## index.html

Abra o arquivo `index.html` na pasta `app\public`. 

Na seção `head`, atualize o `title`:

```html
<title>NFT Colors</title>
```

## index.js

Na pasta `app\src`, abra o arquivo `index.js` e adicione uma linha para utilizar bootstrap em nosso projeto

```javascript
import 'bootstrap/dist/css/bootstrap.css';
```

Também apague está linha:

```javascript
import './index.css';
```

O arquivo `index.js` ficará como este. Se quiser, pode copiar deste link ou do código abaixo:

[index.js](https://raw.githubusercontent.com/solangegueiros/tokens-rsk/master/colors-create-react-app/app/src/index.js)

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import 'bootstrap/dist/css/bootstrap.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```

Agora seu `index.js` ficará desta forma:

![index.js](/images/image-17.png)

## App.css

Abra o arquivo `App.css` e sobrescreva com o código abaixo:

[App.css](https://raw.githubusercontent.com/solangegueiros/tokens-rsk/master/colors-create-react-app/app/src/App.css)

```javascript
.token {
  height: 150px;
  width: 150px;
  border-radius: 50%;
  display: inline-block;
}
```

Este arquivo customiza a aparência dos tokens.

Este é o resultado:

![App.css](/images/image-18.png)

## App.js

Abra o arquivo `App.js` e sobrescreva com o código abaixo:

[App.js](https://raw.githubusercontent.com/solangegueiros/tokens-rsk/master/colors-create-react-app/app/src/App.js)

```javascript
import React, { Component } from 'react';
import Web3 from 'web3';
import './App.css';
import Color from './contracts/Color.json';

function colorHexToString(hexStr) {
  return '#' + hexStr.substring(2);
}

function colorStringToBytes(str) {
  if (str.length !== 7 || str.charAt(0) !== '#') {
    throw new Error('invalid color string');
  }
  const hexStr = '0x' + str.substring(1);
  return Web3.utils.hexToBytes(hexStr);
}

class App extends Component {

  constructor(props) {
    super(props);
    this.state = {
      account: '',
      contract: null,
      totalSupply: 0,
      colors: [],
    };
  }

  async componentWillMount() {
    await this.loadWeb3();
    await this.loadBlockchainData();
  }

  async loadWeb3() {
    if (window.ethereum) {
      // current web3 providers
      window.web3 = new Web3(window.ethereum);
      await window.ethereum.enable();
    }
    else if (window.web3) {
      // fallback for older web3 providers
      window.web3 = new Web3(window.web3.currentProvider);
    }
    else {
      // no web3 provider, user needs to install one in their browser
      window.alert(
        'No injected web3 provider detected');
    }
    console.log(window.web3.currentProvider);
  }

  async loadBlockchainData() {
    const web3 = window.web3;
    // Load account
    const accounts = await web3.eth.getAccounts();
    console.log ('account: ', accounts[0]);
    this.setState({ account: accounts[0] });
    const networkId = await web3.eth.net.getId();
    const networkData = Color.networks[networkId];

    if (!networkData) {
      window.alert('Smart contract not deployed to detected network.');
      return;
    }

    const abi = Color.abi;
    const address = networkData.address;
    const contract = new web3.eth.Contract(abi, address);
    this.setState({ contract });
    const totalSupply = await contract
      .methods.totalSupply().call();
    this.setState({ totalSupply });

    // Load Colors
    for (var i = 1; i <= totalSupply; i++) {
      const colorBytes = await contract
        .methods.colors(i - 1).call();
      const colorStr = colorHexToString(colorBytes);
      this.setState({
        colors: [...this.state.colors, colorStr],
      });
    }
  }

  mint = (colorStr) => {
    const colorBytes = colorStringToBytes(colorStr);
    this.state.contract.methods
      .mint(colorBytes)
      .send({ from: this.state.account })
      .once('receipt', (receipt) => {
        console.log ('transaction receipt: ', receipt)
        this.setState({
          colors: [...this.state.colors, colorStr],
        });
      });
  }

  render() {
    return (
      <div>
        <nav className="navbar navbar-dark fixed-top bg-dark flex-md-nowrap p-0 shadow">
          <span className="navbar-brand col-sm-3 col-md-2 mr-0">
            Color Tokens
          </span>
          <ul className="navbar-nav px-3">
            <li className="nav-item text-nowrap d-none d-sm-none d-sm-block">
              <small className="text-white"><span id="account">{this.state.account}</span></small>
            </li>
          </ul>
        </nav>
        <div className="container-fluid mt-5">
          <div className="row">
            <main role="main" className="col-lg-12 d-flex text-center">
              <div className="content mr-auto ml-auto">
                <h1>Issue Token</h1>
                <form onSubmit={(event) => {
                  event.preventDefault();
                  const colorStr = this.color.value;
                  this.mint(colorStr);
                }}>
                  <input
                    type='text'
                    className='form-control mb-1'
                    placeholder='e.g. #FF00FF'
                    ref={(input) => { this.color = input }}
                  />
                  <input
                    type='submit'
                    className='btn btn-block btn-primary'
                    value='MINT'
                  />
                </form>
              </div>
            </main>
          </div>
          <hr/>
          <div className="row text-center">
            { this.state.colors.map((colorStr, key) => {
              return (
                <div key={key} className="col-md-3 mb-3">
                  <div className="token" style={{ backgroundColor: colorStr }}></div>
                  <div>{colorStr}</div>
                </div>
              );
            })}
          </div>
        </div>
      </div>
    );
  }
}

export default App;
```

## Entendendo App.js

Depois de importar web3 aqui:

```javascript
import Web3 from 'web3'
```

Esta parte faz a conexão com a RSK testnet utilizando a carteira injetada (web3 injected), no nosso caso, Metamask:

```javascript
  async loadWeb3() {
    if (window.ethereum) {
      // current web3 providers
      window.web3 = new Web3(window.ethereum);
      await window.ethereum.enable();
    }
    else if (window.web3) {
      // fallback for older web3 providers
      window.web3 = new Web3(window.web3.currentProvider);
    }
    else {
      // no web3 provider, user needs to install one in their browser
      window.alert(
        'No injected web3 provider detected');
    }
    console.log(window.web3.currentProvider);
  }
```
Para carregar a instância do smart contract Color que já está publicado, precisamos carregar algumas informações do arquivo de artifatos do Truffle:

```javascript
import Color from './contracts/Color.json';
```

E depois da conexão com sucesso com o web3, a função `loadBlockchainData` carrega as contas, informações da rede e o smart contract Color.

```javascript
  async loadBlockchainData() {
    const web3 = window.web3;
    // Load account
    const accounts = await web3.eth.getAccounts();
    console.log ('account: ', accounts[0]);
    this.setState({ account: accounts[0] });
    const networkId = await web3.eth.net.getId();
    const networkData = Color.networks[networkId];

    if (!networkData) {
      window.alert('Smart contract not deployed to detected network.');
      return;
    }

    const abi = Color.abi;
    const address = networkData.address;
    const contract = new web3.eth.Contract(abi, address);
    this.setState({ contract });
    const totalSupply = await contract
      .methods.totalSupply().call();
    this.setState({ totalSupply });

    // Load Colors
    for (var i = 1; i <= totalSupply; i++) {
      const colorBytes = await contract
        .methods.colors(i - 1).call();
      const colorStr = colorHexToString(colorBytes);
      this.setState({
        colors: [...this.state.colors, colorStr],
      });
    }
  }
```

Também temos a função mint que envia uma transação para o blockchain chamando o método mint no smart contract.

```javascript
  mint = (colorStr) => {
    const colorBytes = colorStringToBytes(colorStr);
    this.state.contract.methods
      .mint(colorBytes)
      .send({ from: this.state.account })
      .once('receipt', (receipt) => {
        console.log ('transaction receipt: ', receipt)
        this.setState({
          colors: [...this.state.colors, colorStr],
        });
      });
  }
```

E, finalmente, a função `render()` é responsável pelo código HTML da aplicação. São basicamente 3 partes:

* Apresentar o endereço ativo (selecionado no Metamask) na barra de navegação
* Um formulário para a emissão de novos tokens
* Listar todos os tokens emitidos para a conta ativa, em forma de tabela, mostrando a cor e o código de cada token.

## Running

No diretório `app`, no terminal, executar:

```
npm start
```

![npm start](/images/image-19.png)

Será aberto o browser default no endereço [http://localhost:3000/](http://localhost:3000/)

Se não abrir, você pode manualmente copiar e colar a url no browser.

![localhost:3000](/images/image-20.png)

Metamask automaticamente detectará que nossa aplicação quer se conectar com a carteira, então temos que autorizar clicando no botão Connect.

![authorize Metamask](/images/image-21.png)

E aqui está nosso frontend!

![frontend](/images/image-22.png)

# Interagindo com o smart contract

As cores são salvas utilizando a representação hexadecimal de cada uma.

Para saber mais sobre cores e seus códigos hexa:

* [https://htmlcolorcodes.com/](https://htmlcolorcodes.com/)
* [https://www.color-hex.com/color-names.html](https://www.color-hex.com/color-names.html)
* [https://www.rapidtables.com/web/color/RGB_Color.html](https://www.rapidtables.com/web/color/RGB_Color.html)

Alguns códigos de cores:

<table>
  <tr>
    <td>Red</td>
    <td>#FF0000</td>
  </tr>
  <tr>
    <td>Green</td>
    <td>#00FF00</td>
  </tr>
  <tr>
    <td>Blue</td>
    <td>#0000FF</td>
  </tr>
  <tr>
    <td>Yellow</td>
    <td>#FFFF00</td>
  </tr>
</table>

## Mint - emitindo tokens

Escolha uma cor, preencha seu valor hexadecimal no campo de texto e clique no botão `MINT`.

Será chamada a função `mint()` no smart contract Color, com o parâmetro da cor que você definiu.

Vamos criar a cor vermelha, `#FF0000`.

> Não esqueça do símbolo `#`

![#FF0000](/images/image-23.png)

![#FF0000 Metamask confirm](/images/image-24.png)

Clique no botão `confirm`.

Aguarde alguns segundos até que a transação seja incluída em um bloco…

Ótimo! Você já tem seu primeiro token colecionável de cores: 

![Red minted](/images/image-25.png)

Vamos criar a cor azul: `#0000FF`

![#0000FF](/images/image-26.png)

Aguarde alguns segundos até que a transação seja minerada…

E agora temos duas cores na coleção!

![Blue minted](/images/image-27.png)

A coleção está crescendo!

![color collection](/images/image-28.png)

# Parabéns!

Você imaginava que era tão fácil criar um NFT? 

Eu mostrei como conectar o framework Truffle com a rede RSK e publicar seu próprio token colecionável utilizando a biblioteca Open Zeppelin. 

Este tutorial foi inspirado em outro criado por Gregory McCubbin, da dApp University. Confira o [original](https://www.dappuniversity.com/articles/blockchain-programming).

Nosso objetivo é unir forças e dar opções para as pessoas que acreditam nos smart contracts baseados em Ethereum e também na força do Bitcoin, fazer isso na rede RSK.

Espero que esse tutorial tenha sido útil e agradeço caso tenha algum feedback para mim. Compartilhe o artigo caso tenha gostado :)

Se quiser ver outros videos, assine meu canal: 
<a href="https://www.youtube.com/user/solangegueiros" target="_blank"> youtube Solange Gueiros</a>
