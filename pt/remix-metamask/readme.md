# Crie seu primeiro smart contract utilizando Remix e Metamask com a RSK testnet

Configure uma wallet utilizando Metamask, conecte a RSK testnet, crie, compile e publique um smart contract utilizando Remix diretamente na RSK tesnet.

![Title](/images/image-00.png)

A implementação da máquina virtual da RSK é compatível com o Ethereum EVM, então  podemos usar muitas das ferramentas para desenvolvedores Ethereum.

Neste tutorial, mostrarei passo a passo como usar o Remix e o Metamask, ferramentas criadas originalmente para o Ethereum, para criar, compilar e publicar um smart contract simples na Testnet da RSK.
## Overview

Executaremos estas etapas:

1. Configurar Metamask para se conectar a testnet RSK;
1. Adquirir alguns testnet R-BTCs no faucet;
1. Conectar Remix com a RSK Testnet;
1. Criar um smart contract no Remix;
1. Compilar;
1. Fazer a publicação na RSK Testnet utilizando Remix;
1. Conhecer o RSK explorer;
1. Interagir com o smart contract;
1. Verificar as transações no Metamask.

## Video

Caso prefira assistir um video, eu ministrei um workshop online sobre este tutorial:
<a href="https://www.youtube.com/watch?v=TTTUcgGO-Ko" target="_blank"> clique aqui</a>

## Pré-requisitos

- Metamask
- Remix

### Metamask

Metamask é um tipo de carteira web que facilita transações usando suas contas.
Também pode ser usado com redes RSK.
Possui versões para vários navegadores, como Chrome, Firefox, Opera e Brave.

Vá em [metamask.io](https://metamask.io/) e faça a instalação.

Crie uma conta / carteira.

Salve seu mnemônico, ou frase de backup, ou seed (tudo isso significa a mesma coisa), com 12 palavras. Estas palavras serão a única forma de recuperar sua conta, se você esquecer ou perder sua senha.

O mnemônico é a parte mais importante em uma conta / carteira!

### Remix

Remix é uma ferramenta online. É um IDE (Integrated Development Environment - ambiente de desenvolvimento integrado) usado para escrever, compilar, publicar e depurar código fonte em Solidity. Pode ser conectado ao Metamask e, com essa conexão, utilizado para publicar contratos inteligentes na RSK Testnet e na RSK Mainnet.

Pode ser acessado em [remix.ethereum.org](https://remix.ethereum.org/)

## Conectando MetaMask a RSK testnet

Vou colocar alguns termos em inglês também: 

- Vá em networks / redes
- Custom RPC

![networks - custom RPC](/images/image-01.png)

- Network Name / nome da rede

    RSK Testnet

- New RPC URL

    [https://public-node.testnet.rsk.co](https://public-node.testnet.rsk.co)

- ChainID (opcional)

    31

- Symbol / símbolo (opcional)

    tRBTC

- Block Explorer URL (opcional)

    https://explorer.testnet.rsk.co


![RSK Testnet configuration](/images/image-02.png)

Depois de configurar, feche a janela de configuração e selecione RSK Tesnet.

## TestNet Faucet

Você pode receber alguns Testnet RBTC em [faucet.testnet.rsk.co](https://faucet.testnet.rsk.co/).

Copie seu endereço da conta do Metamask

![Copy address from Metamask](/images/image-03.png)

Cole seu endereço e faça a a verificação do CAPTCHA.

Espere alguns segundos...

![Wait a few seconds](/images/image-04.png)

![Received some R-BTCs](/images/image-05.png)

Você pode ver o hash da transação, por examplo, eu fiz esta: [`0xf63c45dabd52e0b44f4cf15825985e9ddfe790b4323a88a3531f762a417f9011`](https://explorer.testnet.rsk.co/tx/0xf63c45dabd52e0b44f4cf15825985e9ddfe790b4323a88a3531f762a417f9011).

Agora eu tenho 0.05 RBTC!

![R-BTCs at Metamask wallet](/images/image-06.png)

## Remix

Vá em

[remix.ethereum.org](http://remix.ethereum.org/)

Na página inicial - home / welcome page, escolha o ambiente / environment `Solidity`.

![Remix environment Solidity](/images/image-08.png)

## Conectando o Remix a RSK Testnet através do Metamask

Com a RSK network selecionada no Metamask...

No Remix, do lado esquerdo, procure o botão `Deploy and run transactions`.
Atualmente é o 4o botão

![Deploy and run transactions](/images/image-09.png)

Em Environment, escolha `Injected Web3`

![Injected Web3](/images/image-10.png)

Injected Web3 conecta Remix com a conta ativa / selecionada no Metamask

![Injected Web3 - ChainID 31](/images/image-11.png)

ChainID 31 foi definida na configuração da RSK Testnet no Metamask.

## Crie um smart contract

Clique no 2o botão do lado esquerdo - file explorer

![file explorer](/images/image-12.png)

Clique no símbolo `+` create a new file

![create a new file](/images/image-13.png)

Nome do arquivo / file name: `SimpleStorage.sol`

![filename SimpleStorage.sol](/images/image-14.png)

Copie esse exemplo:

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

E cole aqui:
![SimpleStorage.sol](/images/image-15.png)

### SimpleStorage.sol

Este smart contract tem:

* Uma variável `storedData` para armazenar um número
* Uma função `get()` para retornar o número armazenado na variável `storedData`
* Uma função `set()` para alterar o número armazenado na variável `storedData`

## Compilar o smart contract

Clique no 3o botão do lado esquerdo - Solidity compiler

![Compile smart contract](/images/image-16.png)

Costumo deixar habilitado o auto-compile

![enable auto-compile](/images/image-17.png)

![enable auto-compile](/images/image-18.png)

Clique no botão Compile SimpleStorage.sol

![Compile SimpleStorage.sol](/images/image-19.png)

Verifique o sinal verde no 3o botão com a mensagem `compilation successful`

![compilation successful](/images/image-20.png)

## Publique o smart contract na RSK testnet

No painel à esquerda, clique no botão `Deploy and run transactions`. Atualmente é o 4o botão.

![Deploy and run transactions](/images/image-21.png)

Como nós só temos um smart contract, ele é automaticamente selecionado na lista.

![SimpleStorage.sol](/images/image-22.png)

clique no botão `Deploy`.

Metamask abrirá uma janela do tipo "popup" para confirmar a transação configurada pelo Remix para fazer a publicação do smart contract SimpleStorage.sol

![Deploy](/images/image-23.png)

Clique em confirme - confirm.

Na parte de baixo, à direita, aparecerá a mensagem: `creation of SimpleStorage pending...`

![creation of SimpleStorage pending](/images/image-24.png)


![transaction confirmed](/images/image-25.png)

Quando for confirmada, podemos verificá-la:

![check the transaction](/images/image-26.png)

Na linha da transação, clique no símbolo ao lado do botão debug (do lado direito do botão), para ver os detalhes:

![transaction details](/images/image-27.png)

Copie o hash da transação para verificá-la no blockchain explorer

![transaction hash](/images/image-28.png)

![transaction hash copied](/images/image-29.png)

Neste examplo, o hash da transação é:

0x419c4b17ec0bf59568d9b5f5c7f0e4678039f52b9c644c2914ccd0bd2bb331da

## RSK Explorer

O RSK explorer é o blockchain explorer para as transações da RSK. Estamos utilizando o explorer da Testnet:

[explorer.testnet.rsk.co](https://explorer.testnet.rsk.co/)

![explorer](/images/image-30.png)

Cole o hash da transação no campo de pesquisa, no alto da tela

![search](/images/image-31.png)

![search transaction](/images/image-32.png)

Este é o resultado:

![explorer transaction result](/images/image-33.png)

Você pode verificar meu exemplo aqui: [`0x419c4b17ec0bf59568d9b5f5c7f0e4678039f52b9c644c2914ccd0bd2bb331da`](https://explorer.testnet.rsk.co/tx/0x419c4b17ec0bf59568d9b5f5c7f0e4678039f52b9c644c2914ccd0bd2bb331da)

## Interagindo com o smart contract

Quando fazemos a publicação de um smart contract utilizando Remix, podemos encontrá-lo no painel a esquerda, no item deploy and run transactions:

![deployed contracts](/images/image-34.png)

Clique em `>` para enxergar os detalhes do SimpleStorage:

![Deployed SimpleStorage](/images/image-35.png)

Aparecerão as mesmas funções que criamos em nosso smart contract!

Os botões laranja são as funções que fazem alterações no blockchain, são chamadas alterações de estado. Este tipo de função gasta gas quando utilizada.

Os botões azuis são as funções somente leitura (read-only) e não fazem nenhuma alteração no blockchain. Não gastamos gas quando fazemos uma chamada para este tipo de função.

### Get

Primeiro vamos verificar o valor armazenado logo após a publicação.

Clique no botão get

![get](/images/image-36.png)

Não temos nenhum valor armazenado porque não definimos nada quando fizemos a publicação.

No área de mensagens à direita, podemos verificar que foi chamada a função `SimpleStorage.get()`:

![transaction get](/images/image-37.png)

### Set

Defina um valor no campo do lado direito do botão set, e depois clique no botão.

![value to set](/images/image-38.png)

Abrirá uma janela popup do Metamask, para confirmar a transação para armazenar o valor no blockchain.

![confirm transaction](/images/image-39.png)

Clique em confirme - confirm

No painel de mensagens embaixo, à direita, podemos verificar que a transação está pendente, esperando a confirmação no blockchain:

![transaction status](/images/image-40.png)

Depois de alguns segundos, Metamask mostrará quando a transação for confirmada!

![transaction mined](/images/image-41.png)

No painel de mensagens embaixo, à direita, temos os detalhes da transação:

![transaction details](/images/image-42.png)

Você pode copiar o hash da transação e também verificar no RSK explorer: [`0xb9f4d73e7555d2b3cdf516f2d3044daa58669f7324cb957f2b83da21a6c89b4b`](https://explorer.testnet.rsk.co/tx/0xb9f4d73e7555d2b3cdf516f2d3044daa58669f7324cb957f2b83da21a6c89b4b)

![explorer](/images/image-43.png)

### Get (novamente)

Agora que temos o valor 2020 salvo, podemos consultar

Clique no botão get

![get](/images/image-44.png)

E o valor está correto!

## Transações no Metamask

É possível verificar todas as transações no Metamask

![Metamask transactions](/images/image-45.png)

![Metamask transactions](/images/image-46.png)

## Considerações finais

Você imaginava que era tão fácil utilizar Remix e Metamask para criar um smart contract que pode ser utilizado em ambas as redes: Ethereum e RSK?

Eu mostrei como algumas ferramentas de desenvolvedores Ethereum podem ser utilizadas na rede RSK.

Nosso objetivo é unir forças e dar opções para as pessoas que acreditam nos smart contracts baseados em Ethereum e também na força do Bitcoin, fazer isso na rede RSK.

Espero que esse tutorial tenha sido útil e agradeço caso tenha algum feedback para mim. Compartilhe o artigo caso tenha gostado :)

Se quiser ver outros videos, assine meu canal: 
<a href="https://www.youtube.com/user/solangegueiros" target="_blank"> youtube Solange Gueiros</a>
