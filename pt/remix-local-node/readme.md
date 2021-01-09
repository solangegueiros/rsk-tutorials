# Como conectar o Remix a um nó local da rede RSK

Crie um smart contract utilizando Remix, faça a conexão ao nó local da rede RSK e publique o smart contract.

![Title](/images/image-01.png)

A implementação da máquina virtual da RSK é compatível com o Ethereum EVM, então  podemos usar muitas das ferramentas para desenvolvedores Ethereum.

Neste tutorial, mostrarei passo a passo como usar o Remix, ferramenta do Ethereum, conectado a um nó local no Blockchain RSK. Vamos criar, compilar e publicar um smart contract simples.

## Overview

Executaremos estas etapas:

1. Pré-requisitos;
1. Conecte o Remix ao nó local RSK;
1. Crie um smart contract;
1. Compile o smart contract;
1. Publique localmente na RSK utilizando Remix;
1. Interagindo com o smart contract;

## Video

Caso prefira assistir um video, veja a primeira parte deste 
<a href="https://www.youtube.com/watch?v=txpJ7ybiqJw" target="_blank">webinar</a>.

# Pré-requisitos
1. Execução de um nó RSK local
2. Remix

## 1. Execução de um nó RSK local

Siga as instruções do tutorial [Como executar um nó local do Blockchain RSK](https://solange.dev/2020/rsk-local-node/)

## 2. Remix

Remix é uma ferramenta online desenvolvida para o Blockchain Ethereum. É um IDE (Integrated Development Environment - ambiente de desenvolvimento integrado) usado para escrever, compilar, publicar e depurar código fonte em Solidity. Dado que a RVM - RSK Virtual Machine é compatível com o Ethereum EVM, o Remix pode ser conectado diretamente ao nó local RSK e, com essa conexão, utilizado para publicar contratos inteligentes no Blockchain RSK localmente.

Vá para o site: 

[http://remix.ethereum.org/](http://remix.ethereum.org/)

> Atenção: utilizaremos a conexão http e não https!

Na página inicial - home / welcome page, escolha o ambiente / environment `Solidity`.

![Remix environment Solidity](/images/image-02.png)

### Terminal

No Remix, na parte abaixo, à direita, existe um terminal com algumas bibliotecas disponíveis.

Você pode enviar comandos e transações por aqui. Também apresenta o resultado das transações e / ou chamadas às funções de smart contracts.

> Esta área de retorno é muito importante para acompanhar os resultados!

![remix terminal](/images/image-03.png)

### Solidity compiler

Clique no 3o botão do lado esquerdo - Solidity compiler

![remix solidity compiler](/images/image-04.png)

Habilite o `auto-compile` para facilitar sua vida, compilando automaticamente os smart contracts durante a edição no Remix.

![auto-compile](/images/image-05.png)

# Conecte o Remix ao nó local RSK

No lado esquerdo do Remix, procure o botão `Deploy and run transactions`.
Atualmente é o 4o botão.

![Deploy and run transactions](/images/image-06.png)

Em Environment, escolha `Web3 Provider`

![Web3 Provider](/images/image-07.png)

Defina o `Web3 Provider Endpoint`:

```txt
http://127.0.0.1:4444
```

Web3 Provider conecta o Remix com as contas do nó local

![Web3 Provider Endpoint](/images/image-08.png)

ChainID 33 é o padrão para o nó local da RSK.

![ChainID 33](/images/image-09.png)

## Accounts

A lista de contas do nó local é apresentada no campo `ACCOUNT`. São diversos endereços / contas com R-BTCs fictícios que podemos escolher para publicar um smart contract ou interagir com ele.

Veja um exemplo de lista de contas:

![account](/images/image-10.png)

> Está sendo apresentada a unidade `ether` para os saldos, mas na realidade é `R-BTC`. Lembre-se que o Remix é uma ferramenta do Blockchain Ethereum. 

# Crie um smart contract

Clique no 2o botão do lado esquerdo - file explorer

![file explorer](/images/image-11.png)

Clique no símbolo `+` create a new file

![create a new file](/images/image-12.png)

Nome do arquivo / file name: `Register.sol`

![Register.sol](/images/image-13.png)

Copie e cole o smart contract deste link ou abaixo:

[register.sol](https://raw.githubusercontent.com/solangegueiros/dapp-register-rsk/master/register-rsk-rsk3-webpack/register.sol)


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

E cole aqui:
![Register.sol](/images/image-14.png)

## Register.sol

Este smart contract contém:

* A variable `info`, privada, para armazenar uma string
* A function `getInfo()` para retornar a string armazenada na variável `info`
* A function `setInfo()` para alterar a string armazenada na variável `info`

# Compile o smart contract

Se você habilitou o `auto-compile`, o smart contract já está compilado e aparecerá um sinal verde ao lado do 3o botão do lado esquerdo - Solidity compiler.

Caso não tenha habilitado: 
- Clique no 3o botão do lado esquerdo - Solidity compiler
- Clique no botão Compile Register.sol

Verifique o sinal verde no 3o botão com a mensagem `compilation successful`

![compilation successful](/images/image-15.png)

# Publique o smart contract localmente na RSK

No painel à esquerda, clique no 4o botão à esquerda, `Deploy and run transactions`.

Como nós só temos um smart contract, ele é automaticamente selecionado na lista.

![Register.sol](/images/image-16.png)

clique no botão `Deploy`.

![Deploy](/images/image-17.png)

No área de mensagens à direita, podemos verificar a transação de criação do smart contract `Register`:

![Deploy](/images/image-18.png)

# Interagindo com o smart contract

Quando fazemos a publicação de um smart contract utilizando Remix, podemos encontrá-lo em deploy and run transactions, na parte de baixo do painel à esquerda, `Deployed Contracts`:

![deployed contracts](/images/image-19.png)

Clique em `>` para visualizar os detalhes do Register:

![Register](/images/image-20.png)

Aparecerão as mesmas funções que criamos em nosso smart contract!

![Register](/images/image-21.png)

Os botões laranja são as funções que fazem alterações no blockchain, são chamadas alterações de estado. Este tipo de função gasta gas quando utilizada.

Os botões azuis são as funções somente leitura (read-only) e não fazem nenhuma alteração no blockchain. Não gastamos gas quando fazemos uma chamada para este tipo de função.

## GetInfo

Primeiro verificaremos a informação armazenada logo após a publicação.

Clique no botão `getInfo`

![getInfo](/images/image-22.png)

Não temos nenhuma informação armazenada porque não definimos nada quando fizemos a publicação.

No área de mensagens à direita, podemos verificar que foi chamada a função `Register.getInfo()`:

![transaction get](/images/image-23.png)

## SetInfo

Defina uma informação no campo do lado direito do botão `setInfo`. Por exemplo, vou armazenar `RSK`.

Depois clique no botão laranja `setInfo`.

![value to set](/images/image-24.png)

Não há nenhuma mensagem de confirmação porque o Remix está conectado diretamente ao nó, e desta forma acessa a chave privada da carteira selecionada, sem precisar de alguma ação de autorização.

No painel de mensagens embaixo, à direita, depois de alguns segundos podemos verificar a transação:

![transaction status](/images/image-25.png)

## GetInfo (novamente)

Agora que temos a informação `RSK` salva, podemos consultá-la.

Clique no botão `getInfo` novamente

![getInfo again](/images/image-26.png)

E a informação está correta!

# Endereço do smart contract

Clique no botão `copy` que está localizado do lado direito do smart contract para copiar o endereço do mesmo. Ele será utilizado no frontend ou outras formas de interação.

![address](/images/image-27.png)

Na minha publicação, o endereço é `0xb5a9b7dA0ffcD59fDE4987Ff361eFB981Bb819F7`.

# Considerações finais

Você imaginava que era tão fácil utilizar Remix para se conectar a um nó local no Blockchain RSK?
Eu mostrei como esta excelente ferramenta para desenvolvedores Ethereum pode ser utilizada para criar, compilar, publicar smart contracts e interagir com eles na rede RSK.

Nosso objetivo é unir forças e dar opções para as pessoas que acreditam poder dos smart contracts baseados em Ethereum e também na segurança do Bitcoin, fazer isso na rede RSK.

## Próximos passos

Aprenda a criar um frontend utilizando a biblioteca `rsk3.js`:

- [Utilização da biblioteca rsk3.js no seu site com webpack](https://solange.dev/2020/rsk-rsk3js-webpack/)

Espero que esse tutorial tenha sido útil e agradeço caso tenha algum feedback. 
Compartilhe o artigo caso tenha gostado :)

Se quiser ver outros videos, assine meu canal: 
<a href="https://www.youtube.com/user/solangegueiros" target="_blank"> youtube Solange Gueiros</a>
