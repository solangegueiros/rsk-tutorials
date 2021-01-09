---
title: Crie seu primeiro token na rede RSK
date: "2020-04-26T10:00:00.000Z"
description: "Como criar um token ERC20 com menos de 10 linhas de código! Vamos utilizar Truffle e os smart contracts da Open Zeppelin, publicando na testnet da RSK.
"
tags: tutorial, rsk, token, openzeppelin, erc20, truffle
type: blog
---

![Title](/images/image-00.png)

Neste tutorial vou ensinar como criar um token ERC20 com menos de 10 linhas de código! Vamos utilizar Truffle e os smart contracts da Open Zeppelin, publicando na testnet da RSK.

# Overview

Aqui está um resumo das etapas que faremos para construir nosso token:

1. Inicializar um projeto utilizando Truffle;
2. Instalar os smart contracts da Open Zeppelin smart contracts no projeto;
3. Criar um mnemônico para uma carteira;
4. Configurar Truffle para conectar à RSK testnet;
5. Adquirir alguns tR-BTCs no faucet;
6. Criar o smart contract do token;
7. Criar um arquivo de publicação no Truffle;
9. Publicar o smart contract na RSK Testnet utilizando Truffle;
10. Interagir com o smart contract através do Truffle console.

## Video

Caso prefira assistir um video, eu ministrei um workshop online sobre este tutorial:
<a href="https://www.youtube.com/watch?v=9P0HtQ4s9wc" target="_blank"> clique aqui</a>

## Pré-requisitos

* Node.js e NPM (Node Package Manager)
* Visual Studio Code (VSCode) ou outro editor de sua escolha
* Truffle

## Node.js e NPM

Precisamos do Node.js e NPM, eles são instalados juntos.

Pra verificar se Node.js e NPM já estão instalados, verifique se os comandos abaixo funcionam no terminal:

```shell
node --version
npm --version
```

![node and npm version](/images/image-01.png)

Vá em [Node.js](https://nodejs.org/en/) caso precise instalar.

## Visual Studio Code (VSCode)

Neste tutorial estou utilizando VSCode para criar os arquivos do projeto.

Para instalar, [faça o download aqui](https://code.visualstudio.com/download).

Verifique se a instalação do VS code está ok consultando sua versão no terminal:

```shell
code -v
```

![visual code version](/images/image-02.png)

## Truffle

Truffle é um conhecido framework para desenvolvimento de smart contract, que facilita a vida do desenvolvedor.
Entre suas características, podemos citar o gerenciamento da "vida" de um smart contract (voce pode fazer várias publicações e saber qual foi a última), desenvolvimento de scripts para deploy, testes automatizados e gerenciamento de rede simplificado.

Da mesma forma também facilita o desenvolvedor RSK porque podemos configurar as redes RSK no Truffle.

Para instalar Truffle, no terminal, digite o comando abaixo no terminal e pressione a tecla `enter`:

```shell
npm install truffle -g
```

![truffle install](/images/image-03.png)

Quando a instalação finalizar, feche a janela do terminal e abra novamente para verificar a versão do Truffle:

```shell
truffle version
```

![truffle version](/images/image-04.png)

Mais informações:

[trufflesuite.com/truffle](https://www.trufflesuite.com/truffle)

# Inicialize um projeto Truffle

Crie uma nova pasta chamada `token`:

```shell
mkdir token
cd token
```

Inicialize um projeto Truffle nesta pasta executando o comando abaixo no terminal:

```shell
truffle init
```

Por exemplo, eu vou criar a pasta neste local: `C:\RSK\`

Então meu projeto pode ser localizado no diretório: `C:\RSK\token`.

![truffle init](/images/image-05.png)

Abra a pasta no VSCode. 
Você verá uma estrutura de diretórios como esta:

![truffle file structure](/images/image-06.png)

* `./contracts`: Todos os smart contracts serão salvos nesta pasta.
* `./migrations`: Os scripts para publicação ficarão armazenados aqui.
* `./test`: Aqui serão salvos os scripts para testes.
* `./truffle-config.js`: Este é o arquivo de configuração do Truffle. Aqui vamos configurar as redes, incluindo a da RSK.

Veja que os seguintes arquivos também foram criados:

* `Migrations.sol`: Smart contract que registra todos as publicações realizadas em uma rede.
* `1_initial_migration.js`: Publicação do `Migrations.sol`.

## Inicialize um projeto npm

Para inicializar um projeto npm na pasta `token`,  execute o comando abaixo no terminal:

```shell
npm init -y
```

![npm init](/images/image-07.png)

## Instale Open Zeppelin

OpenZeppelin Contracts é um conjunto de bibliotecas para smart contracts Ethereum, desenvolvidos em Solidity. Eles também funcionam em outros blockchains, como a **RSK**. 

Serão instaladas não apenas as bibliotecas com padrões de tokens, mas também outras para propriedade e controle de perfis, matemática e outros utilitários.

Vale ressaltar que essas bibliotecas foram revisadas e auditadas visando altos padrões de segurança, para que os contratos que dependam delas sejam menos suscetíveis a hackers quando usados corretamente.

No terminal, na pasta `token`, instale as biblitecas OpenZeppelin com este comando:

```shell
npm install -E @openzeppelin/contracts@2.5.0
```

A opção `-E` é para salvar no arquivo de configuração npm as dependências na versão definida na instalação, e não com a versão default.
Os smart contracts podem ser alterados de uma versão para outra, então é importante fixar a versão porque nosso tutorial foi escrito utilizando esta versão.

![openzeppelin install](/images/image-08.png)

Mais informações:

[openzeppelin.com/contracts](https://openzeppelin.com/contracts/)

## Instale o HD wallet provider

Para conectar a rede RSK network, utilizaremos um pacote provedor que possibilita a conexão a qualquer rede desbloqueando uma conta localmente. 
Utilizaremos `@truffle/hdwallet-provider`. 
Pode ser utilizado para assinar transações de endereços gerados a partir de um mnemônico com 12 ou 24 palavras.

> Precisa ter instalado o Node >= 7.6.

No terminal, na pasta `token`, instale com este comando:

```shell
npm install -E @truffle/hdwallet-provider@1.0.34
```

![hd wallet provider install](/images/image-09.png)

Este é um pacote grande, com muitas dependências. Demora um pouco até aparecer a mensagem `successful installation`.

![hd wallet provider successful installation](/images/image-10.png)


## Verifique o arquivo package.json

`package.json` é um arquivo de configurações que criamos na inicialização do projeto, com o comando `npm init -y`. 

Depois das instalações, na pasta `Token`, abra o arquivo `package.json` no VSCode, e verifique a parte das dependências (dependencies):

![package.json](/images/image-11.png)

# Verifique sua conexão a RSK Testnet

Vamos verificar o status da RSK Testnet, com o comando abaixo. 
Se você estiver utilizando um computador com sistema operacional Windows, não funciona no terminal, minha sugestão é utilizar o Git Bash. Para instalar, vá ao [Git site](https://git-scm.com/).

```shell
curl https://public-node.testnet.rsk.co/1.3.0/ \
  -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

Este comando verifica qual o último bloco da rede.

Você vai receber um retorno parecido com este:

```json
{"jsonrpc":"2.0","id":1,"result":"0xc1266"}
```

![eth_blockNumber jsonrpc result](/images/image-42.png)

O resultado é apresentado em hexadecimal. `0xc3f9b` é o bloco número `802.715`. 
Consultando o site [explorer.testnet.rsk.co](https://explorer.testnet.rsk.co/) e é possível verificar que é o mesmo número de bloco.

![explorer.testnet.rsk.co blockNumber](/images/image-43.png)

# Criar um mnemônico

O próximo passo é criar um mnemônico para gerar endereços de contas a partir dele.

Vamos utilizar este site: 

[iancoleman.io/bip39](https://iancoleman.io/bip39/)

> Isto não é recomendado para uma carteira com fundos de verdade, que valem dinheiro real, porque não é tão seguro gerar um mnemônico e chaves privadas em um website.
> Para fins educacionais podemos utilizar aqui, dado que vamos nos conectar à testnet.

No campo `Generate a random mnemonic` , selecione `12 words` and clique no botão `generate`.

![Generate a random mnemonic](/images/image-12.png)

O resultado estará no campo `BIP39 Mnemonic`. 
São 12 palavras aleatórias, como da figura abaixo:

![BIP39 Mnemonic](/images/image-13.png)

Meu mnemônico é: 

```
access card stove drama pizza elite argue tuition plate kiwi junior sponsor
```

Copie estas 12 palavras para utilizá-las daqui há pouco.

## Arquivo .secret

Dentro da pasta token, crie um arquivo chamado `.secret`.

Cole seu mnemônico neste arquivo e salve.

![dot secret](/images/image-14.png)

# Configure Truffle para conectar à RSK testnet

Abra o arquivo `truffle-config.js` no diretório do seu projeto Truffle e sobreescreva com o seguinte código:

```javascript
const HDWalletProvider = require('@truffle/hdwallet-provider');

const fs = require('fs');
const mnemonic = fs.readFileSync(".secret").toString().trim();

module.exports = {
  networks: {
    testnet: {
      provider: () => new HDWalletProvider(mnemonic, 'https://public-node.testnet.rsk.co/1.3.0/'),
      network_id: 31,
      gasPrice: 0x387EE40,
      networkCheckTimeout: 1000000000
    }
  },  
  compilers: {
    solc: {
      version: "0.5.2",
    }
  }
}
```

# Truffle Console

Truffle tem seu próprio console para executar comandos.

## Verifique a conexão à testnet RSK

Vamos abrir o Truffle console conectando à testnet RSK.

No terminal, no diretório token, execute este comando:

```shell
truffle console --network testnet
```

Aparecerá um novo console:

![truffle console](/images/image-15.png)

O que estamos fazendo é dizer ao Truffle para se conectar ao nó público da RSK testnet, e deixar o console ter controle sobre as contas criadas com o mnemônico.

## Descobrindo um endereço

No truffle console, digite o comando abaixo para guardar na variável `account` o primeiro endereço gerado a partir do nosso mnemônico:

```javascript
var account = Object.keys(web3.currentProvider.wallets)[0]
```

O resultado é a sua conta. Digite isto para mostrá-la:
```
account
```

No meu exemplo, a conta é `0x9682725a85f85f097ab368555a286618dc982c99`. Copie este endereço.

![account address](/images/image-16.png)

## Verifique o saldo

Agora, vamos verificar o saldo de nossa conta. Execute este comando no Truffle console:

```javascript
(await web3.eth.getBalance(account)).toString()
```

![getBalance(account) 0](/images/image-44.png)

O saldo é 0 e precisamos de tR-BTC. Vamos obtê-los no próximo passo.

# TestNet Faucet

Você pode receber alguns Testnet R-BTC em 

[faucet.testnet.rsk.co](https://faucet.testnet.rsk.co/)

![faucet.testnet.rsk.co](/images/image-17.png)

Cole seu endereço (que foi copiado no passo anterior) e faça a a verificação do CAPTCHA.

![Wait a few seconds](/images/image-18.png)

Espere alguns segundos...

![Received some R-BTCs](/images/image-19.png)

Você pode ver o hash da transação, por examplo, eu fiz esta:

[0x16bedc1339a8fe59e270b0c6d5175851010bb93d0cf6c4974f1705b9ead7ee6e](https://explorer.testnet.rsk.co/tx/0x16bedc1339a8fe59e270b0c6d5175851010bb93d0cf6c4974f1705b9ead7ee6e)

E agora eu tenho 0.05 tR-BTC!

## Consulte o saldo (de novo)

Verifique o saldo de nossa conta novamente. Execute este comando no Truffle console:

```javascript
(await web3.eth.getBalance(account)).toString()
```

![getBalance](/images/image-20.png)

Agora eu tenho 50000000000000000, o que significa 0.05 com 18 casas decimais de precisão.

# Crie o smart contract do token

Na pasta `contracts`, crie um novo arquivo chamado`Token.sol`.

![create Token.sol](/images/image-21.png)

## Token.sol tem apenas 7 linhas!

Este smart contract é um `mintable ERC20 token`. 
Isto significa que, além da especificação padrão ERC20, ele tem uma função para emitir novos tokens.

> Para saber mais, vá em [EIP 20: ERC-20 Token Standard](https://eips.ethereum.org/EIPS/eip-20)

Copie e cole o seguinte código fonte:

```javascript
pragma solidity 0.5.2;
import '@openzeppelin/contracts/token/ERC20/ERC20Mintable.sol';
contract Token is ERC20Mintable {
       string public name = "My RSK token";
       string public symbol = "MRT";
       uint8 public decimals = 2;
}
```

Vamos entender o código acima.

Para criar nosso  ERC20 Token, importaremos `ERC20Mintable` do Open Zeppelin. 
Esta biblioteca importa outras como `SafeMath.sol`, o padrão ERC20 e a capacidade de emitir tokens.

Também definimos algumas informações básicas do token: `name` (nome), `symbol` (símbolo), e `decimals` (número de casas decimais).

Para herdar os atributos e funções da biblioteca, apenas definimos nosso contrato como `ERC20Mintable` utilizando a instrução `is`.

![Token.sol](/images/image-22.png)

# Compilando o smart contract

No Truffle console, execute este comando:

```
compile
```

![truffle compile](/images/image-23.png)

# Publicando o smart contract

Primeiro precisamos criar um novo arquivo com instruções para publicação. Ao encontrá-lo, Truffle vai processá-lo no momento do deploy.

## Crie o arquivo `2_deploy_contracts.js`

No diretório `migrations` contém arquivos JavaScript para a publicação dos contratos na rede. 
Estes arquivos são responsáveis por preparar suas tarefas de publicação e são escritos pensado que sua publicação pode ser alterada com o tempo.
O histórico das migrações executadas anteriormente é gravado no Blockchain a partir do smart contract Migrations.(fonte: [truffle: running-migrations](https://www.trufflesuite.com/docs/truffle/getting-started/running-migrations))

Na pasta `migrations`, crie o arquivo `2_deploy_contracts.js`

![create 2_deploy_contracts.js](/images/image-24.png)

Copie e cole:

```javascript
var Token = artifacts.require("Token");

module.exports = function(deployer) {
  deployer.deploy(Token);
};
```

![2_deploy_contracts.js](/images/image-25.png)

## Migrate

No console Truffle, execute este comando:

```
migrate
```

Espere alguns minutos enquanto as transações da publicação do smart contract são gravadas no Blockchain.

Se for necessário, o comando `migrate` vai compilar os smart contracts novamente.

![truffle migrate](/images/image-26.png)

Primeiro é executada a publicação do smart contract `Migrations.sol`, que foi gerado pelo Truffle:

![deploy Migrations.sol](/images/image-27.png)

No meu exemplo, esta é a transação:

[0xd29d03fc2b904545005ab6ed205f970575aef184ebecf14c9f0f6b6f45ec1bb3](https://explorer.testnet.rsk.co/tx/0xd29d03fc2b904545005ab6ed205f970575aef184ebecf14c9f0f6b6f45ec1bb3)

E depois faz publicação do nosso smart contract `Token.sol`:

![deploy Token.sol](/images/image-28.png)

Transação:

[0xbfff7cf431bb4af9e1b059dbd6eea935d7d20e52a770c467f38b97b479ba414a](https://explorer.testnet.rsk.co/tx/0xbfff7cf431bb4af9e1b059dbd6eea935d7d20e52a770c467f38b97b479ba414a)

YYUUPPYY!!!

`My RSK Token` está publicado na Testnet RSK.

Guarde o endereço do contrato do token, ele será ser utilizado depois:

![token address](/images/image-29.png)

No meu caso:

```javascript
tokenAddress = "0x095156af46597754926874dA15DB40e10113fb4d" 
```

# Interagindo com o token no console

Vamos interagir com nosso token através do Truffle console.

## Suas contas / endereços

No Truffle console:

```javascript
const accounts = await web3.eth.getAccounts()
```

Para olhar cada conta:

```javascript
accounts[0]
accounts[1]
```

![accounts](/images/image-30.png)

## Faça a conexão com seu token

```javascript
const token = await Token.deployed()
```

![token instance](/images/image-31.png)

Verifique se a instância está OK.

Escreva o nome da variável:  `token`, tecle `.` e depois aperte a tecla TAB duas vezes para acionar o recurso autocompletar. 
Será apresentado o endereço e hash da transação na publicação, além de outras coisas, incluindo todas as váriaveis e métodos públicos disponíveis. 

```javascript
token. [TAB] [TAB]
```

![token tab tab](/images/image-32.png)

## Consulte o total de tokens

Chame a função `totalSupply` para verificar a quantidade de tokens já emitidos:

```javascript
(await token.totalSupply()).toString()
```

![totalSupply 0](/images/image-33.png)

O valor retornado é 0, o que é esperado, dado que nós não fizemos uma emissão inicial ao publicar o token.

## Consulte o saldo de tokens

Chame a função `balanceOf` para saber o saldo de uma conta, por exemplo, da conta 0:

```javascript
(await token.balanceOf(accounts[0])).toString()
```

![balanceOf 0](/images/image-34.png)

O valor retornado é 0, o que também é esperado, como não foi realizada uma emissão inicial ao publicar o token, o saldo de todas as contas será 0.

## Emitindo tokens

Execute este comando:

```javascript
token.mint(accounts[0], 10000)
```

Está sendo enviada uma transação para a emissão de 100,00 tokens para a conta 0. 

![token.mint account 0](/images/image-45.png)

É possível verificar a transação no explorer:

[0x2162617b34ffcd55cf719cb998e69a33cf115c5d4d58b7ee639c1060fae81355](https://explorer.testnet.rsk.co/tx/0x2162617b34ffcd55cf719cb998e69a33cf115c5d4d58b7ee639c1060fae81355)

Você pode emitir para outras contas, por exemplo, account 1:

```javascript
token.mint(accounts[1], 10000)
```

Para cada conta, o resultado será como este:

![token.mint account 1](/images/image-35.png)

Também é possível emitir para um endereço específico, por exemplo: `0xa52515946DAABe072f446Cc014a4eaA93fb9Fd79`

```javascript
token.mint("0xa52515946DAABe072f446Cc014a4eaA93fb9Fd79", 10000)
```

![token.mint address](/images/image-36.png)

Transação:

[0x1534230dea0ba07b876dd0ad22fdcb693359de42cb12e5af5e55e17543828a85](https://explorer.testnet.rsk.co/tx/0x1534230dea0ba07b876dd0ad22fdcb693359de42cb12e5af5e55e17543828a85)

## Consultando o saldo de tokens (de novo)

Verifique o saldo da conta 0 novamente:

```javascript
(await token.balanceOf(accounts[0])).toString()
```

![balanceOf account 100](/images/image-37.png)

O valor retornado é 10000, o que significa 100 com 2 casas decimais de precisão.
Isto é exatamente o que esperávamos, dado que emitimos 100 tokens

Você também pode consultar o saldo de um endereço específico, por exemplo,  `0xa52515946DAABe072f446Cc014a4eaA93fb9Fd79`:

```javascript
(await token.balanceOf("0xa52515946DAABe072f446Cc014a4eaA93fb9Fd79")).toString()
```

![balanceOf address 100](/images/image-38.png)

## Consultando o total supply (de novo)

Verifique o total de tokens novamente:

```javascript
(await token.totalSupply()).toString()
```

![totalSupply 300](/images/image-39.png)

O valor retornado é 30000, que são 300 tokens com 2 casas decimais de precisão.
Depois de emitir 100 tokens para cada uma das 3 contas, está correto!

## Transfira tokens

Eu gostaria de transferir 40,00 tokens da conta 0 para a conta 2. Farei isto chamando a função `transfer`.

```javascript
token.transfer(accounts[2], 4000, {from: accounts[0]})
```

![token.transfer](/images/image-40.png)

Transação:

[0x529dbbe27e21770c21f4af34dbbbe23733af9be5c8c09b7dd4314fef743275a2](https://explorer.testnet.rsk.co/tx/0x529dbbe27e21770c21f4af34dbbbe23733af9be5c8c09b7dd4314fef743275a2)

A conta 2 não tinha nenhum token antes da transferência, então agora deveria ter 40,00. 
Vamos verificar o saldo da conta 2:

```javascript
(await token.balanceOf(accounts[2])).toString()
```

![balanceOf account 2](/images/image-41.png)

Maravilha! O saldo da conta 2 está certo.

# Considerações finais

Você imaginava que era tão fácil utilizar o framework Truffle conectado com a rede RSK, e que era possível criar um token com menos de 10 linhas de código?

Eu mostrei como fazer a conexão do Truffle com a rede RSK e como publicar um token com apenas 7 linhas de código! 
Também espero que você tenha percebido como é simples utilizar as bibliotecas do Open Zeppelin, e que elas funcionam na rede RSK.

Nosso objetivo é unir forças e dar opções para as pessoas que acreditam nos smart contracts baseados em Ethereum e também na força do Bitcoin, fazer isso na rede RSK.

Espero que esse tutorial tenha sido útil e agradeço caso tenha algum feedback para mim. Compartilhe o artigo caso tenha gostado :)

Se quiser ver outros videos, assine meu canal: 
<a href="https://www.youtube.com/user/solangegueiros" target="_blank"> youtube Solange Gueiros</a>
