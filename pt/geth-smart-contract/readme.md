# Publique um smart contract em um nó local RSK usando Geth e Remix

Como compilar um smart contract utilizando Remix e fazer a publicação em um nó local RSK usando Geth.

![Title](/images/image-00.png)

A implementação da máquina virtual da RSK é compatível com a máquina virtual Ethereum (Ethereum Virtual Machine - EVM), o que nos permite fazer uso de muitas ferramentas dos desenvolvedores Ethereum.

Neste tutorial mostrarei passo a passo como compilar um smart contract utilizando Remix e fazer a publicação em um nó local RSK usando Geth.

## Overview

Executaremos estas etapas:

1. Executar um nó local da RSK;
2. Conectar o Geth ao nó;
3. Criar um smart contract no Remix;
4. Compilar;
5. Criar um arquivo Javascript para fazer a publicação;
6. Publique o smart contract utilizando o console Geth;
7. Interagir com o smart contract.

## Video

Caso prefira assistir um video, eu ministrei um workshop online sobre este tutorial:
<a href="https://www.youtube.com/watch?v=TP6pxdwoHT0" target="_blank"> clique aqui</a>

## Requirements

- Java JDK
- Nó local RSK
- Geth
- Remix - ferramenta online

Importante fazer este tutorial antes de continuar: [Utilizando Geth para conectar em um nó local RSK](/2020/2020-04-05-Rsk-GethAttachLocalNode/).

## Executando um nó local da RSK

Para executar o nó:

```shell
java -cp <PATH-TO-THE-RSKJ-JAR> co.rsk.Start --regtest
```

(Altere <PATH-TO-THE-RSKJ-JAR> para o caminho do arquivo JAR).

Verifique o tutorial: [utilizando Geth para conectar em um nó local RSK](/tutorials/ethereum-devs/geth-attach-local-node/) para mais informações em como fazer isto.

## Conectar ao nó utilizando Geth attach

```shell
geth attach http://127.0.0.1:4444
```

O tutorial: [utilizando Geth para conectar em um nó local RSK](/tutorials/ethereum-devs/geth-attach-local-node/) ensina como fazer isto.

## Remix

Vá em
[Remix](http://remix.ethereum.org/)

Na página inicial - home / welcome page, escolha o ambiente / environment `Solidity`.

![environment Solidity](/images/image-01.png)

## Crie um smart contract

Crie um novo arquivo

Clique no 2o botão do lado esquerdo - file explorer

![Remix file explorer](/images/image-02.png)

Clique no símbolo `+` create a new file

![Remix create a new file](/images/image-03.png)

Nome do arquivo / file name: `Register.sol`

![Register.sol](/images/image-04.png)

Copie o smart contract do gist ou abaixo:

[`Register.sol` gist](https://gist.github.com/solangegueiros/6f30100662f8583ea39a49a5fa198b89)


```shell
pragma solidity >=0.4.0 <0.7.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

Cole no Remix:

![Register.sol at Remix](/images/image-05.png)

### Register.sol

Este smart contract tem:

* Uma variável `storedData` para armazenar um número
* Uma função `get()` para retornar o número armazenado na variável `storedData`
* Uma função `set()` para alterar o número armazenado na variável `storedData`

## Compilar o smart contract

Clique no 3o botão do lado esquerdo - Solidity compiler

![Solidity compiler](/images/image-06.png)

Costumo deixar habilitado o auto-compile:

![enable auto-compile](/images/image-07.png)

![enable auto-compile](/images/image-08.png)

Clique no botão Compile Register.sol

![Compile Register.sol](/images/image-09.png)

Verifique o sinal verde no 3o botão com a mensagem `compilation successful`:

![Compilation successful](/images/image-10.png)

## Crie um arquivo Javascript para publicação

Remix gera um script para publicação de um smart contract e pode ser utilizado no console geth para o deploy no nó local RSK.

No editor que você quiser, crie o arquivo `register.js`.

Eu uso o Visual Studio Code.
Se quiser utilizá-lo também, pode fazer o [download aqui](https://code.visualstudio.com/download).

Eu também criei uma pasta chamada `Register` para colocar o arquivo.

Este é o caminho completo do meu arquivo:

On Windows:

```shell
C:\RSK\Register\register.js
```

Linux, Mac

```shell
/RSK/Register/register.js
```

![register.js](/images/image-11.png)

Volte para o Remix.

na tela `solidity compiler` tem um botão no final da parte esquerda, chamado `Compilation Details`:

![button Compilation Details](/images/image-12.png)

Abrirá uma nova janela. Role a tela para baixo até encontrar `web3Deploy`.

![web3Deploy](/images/image-13.png)

Clique no botão copiar, que fica ao lado direito da palavra `web3Deploy`:

![copy web3Deploy](/images/image-14.png)

![copy web3Deploy](/images/image-15.png)

Cole no arquivo `register.js` e salve o arquivo.

![paste register.js](/images/image-16.png)

Remix cria um script para fazer a publicação de um smart contract e isto pode ser utilizado no console geth para fazer o deploy no nó local RSK.

## Publique o smart contract no console Geth

É muito simples, é só carregar o script utilizando este comando:

```js
loadScript("C:/RSK/Register/register.js");
```

Perceba que, mesmo se você está utilizando SO Windows, o camilho do arquivo utiliza `/` ao invés de `\`.

![loadScript](/images/image-17.png)

Após algumas mensagens, quando a transação do smart contract for incluída em um bloco, você receberá a mensagem "Contract mined!" - contrato minerado.

Normalmente, o prompt de comando da janela desaparecerá após esta mensagem. É só apertar qualquer tecla que ele aparece de novo.

## Interagindo com o smart contract

A primeira coisa a fazer é verificar se a instância do smart contract está OK.

Coloque o nome da instância (register), tecle `.`, e depois pressione a tecla `TAB` duas vezes para acionar o auto-completar. Isto vai apresentar o endereço da publicação, o hash da transação e mais algumas coisas, incluindo todos os métodos disponíveis.

```js
register . [TAB] [TAB]
```

![register . TAB TAB](/images/image-18.png)

### getInfo

Retorna a string armazenada na variável info.

Primeiro verificaremos se existe algum valor armazenado logo após a publicação:

```js
register.getInfo()
```

![register.getInfo](/images/image-19.png)

Nós não tempos nenhuma informação armazenada, porque não definimos nada no construtor, que é a função chamada na publicação do smart contract.

### setInfo

Esta função altera a string armazenada na variável info.

Vamos salvar alguma informação no smart contract executando a função:

```js
register.setInfo("RSK", {from:eth.accounts[1]})
```

![register.setInfo](/images/image-20.png)

Nós recebemos um hash de transação porque enviamos uma transação para alterar o estado do smart contract, dado que alteramos o valor da variável info.

### getInfo (again)

Agora que temos a string "RSK" salva, podemos conformar isto.

Execute a função `getInfo()` de novo:

```js
register.getInfo()
```

![register.getInfo](/images/image-21.png)

E retornará a info `RSK`.

QUe ótimo! Agora temos uma informação salva em nosso smart contract, e podemos fazer a leitura da mesma!

## Considerações finais

Você imaginava que era tão fácil compilar um smart contract e criar um script de publicação utilizando Remix, uma ferramenta muito útil de Ethereum, fazer o deploy no seu nó local RSK utilizando Geth e interagir com seu smart contract de forma tão simples?

Eu mostrei como algumas ferramentas de desenvolvedores Ethereum podem ser utilizadas na rede RSK, e fico muito feliz em utilizá-las!

Nosso objetivo é unir forças e dar opções para as pessoas que acreditam nos smart contracts baseados em Ethereum e também na força do Bitcoin, fazer isso na rede RSK.

Espero que esse tutorial tenha sido útil e agradeço caso tenha algum feedback para mim. Compartilhe o artigo caso tenha gostado :)

Se quiser ver outros videos, assine meu canal: 
<a href="https://www.youtube.com/user/solangegueiros" target="_blank"> youtube Solange Gueiros</a>
