# Migrando dApps do Ethereum para RSK

Conheça as possibilidades para migrar seus smart contracts e aplicações Ethereum para a rede RSK.

![Title](/images/image-00.png)

# Overview

Smart contracts na RSK são escritos em Solidity, que é a mesma linguagem dos Smart Contracts Ethereum. Desta forma não é necessário fazer nenhuma alteração para migrar um smart contract do Ethereum para RSK. Neste tutorial eu mostrarei algumas formas para realizar esta migração.

Para saber mais sobre a linguagem Solidity, consulte sua [documentação](https://solidity.readthedocs.io/).

Arquivos Solidity possuem a extensão `.sol`. Você pode criar ou alterar este tipo de arquivo em qualquer editor de texto, mas é mais produtivo utilizar editores específicos para linguagens de programação porque eles contém formatação, cores e verificação de sintaxe que facilitam a vida.

Eu utilizo o [Visual Studio Code (VS Code)](https://code.visualstudio.com/).

# Como publicar um smart contract na rede RSK

Já falamos anteriormente que nada muda do smart contract Ethereum para o smart contract RSK. 

Então... o que muda? 

> Muda apenas a configuração de rede!

Neste tutorial serão apresentadas instruções para a configuração de rede em 2 ferramentas diferentes:

1. Remix
2. Truffle

# Remix

Remix é uma ferramenta online. É um IDE (Integrated Development Environment - ambiente de desenvolvimento integrado) utilizado para escrever, compilar, publicar e depurar código fonte em Solidity. Pode ser conectado a uma carteira web, e em consequência disto, qualquer rede configurada nesta carteira.

Pode ser acessado em [remix.ethereum.org](https://remix.ethereum.org/)

Para utilizar o Remix, é preciso configurar a rede RSK em uma carteira web, por exemplo, [Metamask](https://metamask.io/).


## Conectando o Remix a RSK Testnet através do Metamask

Com a RSK network selecionada no Metamask...

No painel à esquerda, clique no botão `Deploy and run transactions`. 
Atualmente é o 4o botão.

Em Environment, escolha `Injected Web3`

Injected Web3 conecta Remix com a conta ativa / selecionada no Metamask.

Tudo pronto! Agora é possível fazer a publicação de qualquer smart contract na testnet RSK!

O tutorial [Crie seu primeiro smart contract utilizando Remix e Metamask com a RSK testnet](https://solange.dev/2020/2020-03-27-Rsk-RemixMetamask/) contem mais detalhes e um smart contract básico para publicar. Confira como:

1. Configurar Metamask para se conectar a testnet RSK;
1. Adquirir alguns testnet R-BTCs no faucet;
1. Conectar Remix com a RSK Testnet;
1. Criar um smart contract no Remix;
1. Compilar;
1. Fazer a publicação na RSK Testnet utilizando Remix;
1. Interagir com o smart contract;

# Truffle

[Truffle](https://www.trufflesuite.com/truffle) é um conhecido framework para desenvolvimento de smart contracts, que facilita a vida do desenvolvedor.

Para conectá-lo à rede RSK network, utilizaremos um pacote provedor que possibilita a conexão a qualquer rede desbloqueando uma conta localmente:  
[@truffle/hdwallet-provider](https://www.npmjs.com/package/@truffle/hdwallet-provider). 
Desta forma é possível desbloquear uma conta localmente, a partir de um mnemônico. Isto inclui as redes da RSK. 

Na parte de redes do Truffle config, é definido um provider utilizando o mnemônico gerador da carteira e apontando para o nó público da testnet RSK, através do `HD wallet provider`.

`HD wallet provider` também é utilizado nas redes Ethereum, geralmente em conjunto com [Infura](https://infura.io/) para fazer a conexão à rede. `Infura` disponibiliza uma estrutura de nós para a rede Ethereum, seja a MainNet ou as diversas TestNets.

Um ponto a destacar na configuração de uma rede é verificar o `gas price`. Sem esta verificação, a publicação de um smart contract pode ser demorada ou até mesmo apresentar erros e não acontecer. A rede Ethereum, por exemplo, neste momento (julho/2020), está com um gas price elevado nestes últimos meses devido ao grande número de transações relacionadas a DeFi - Decentralized Finance. Também é importante verificar o `gas price` na rede RSK.

O tutorial [Como criar um projeto utilizando Truffle e OpenZeppelin conectado à rede RSK](https://solange.dev/2020/2020-05-10-Rsk-SetupTruffleOZ/) explica detalhadamente como:

1. Instalar Truffle framework;
2. Inicializar um projeto utilizando Truffle;
3. Instalar HD wallet provider;
4. Criar um mnemônico para uma carteira;
5. Adquirir alguns tR-BTCs no faucet;
6. Testar a conexão à testnet RSK;
7. Verificar o gas price na rede;
8. Configurar Truffle para conectar à rede RSK;

Depois de tudo configurado, minha sugestão é começar publicando através do Truffle o mesmo smart contract utilizado na publicação com o Remix :)

# Próximos passos

Aqui estão alguns tutoriais onde faremos projetos bem legais!

Escolha um destes para começar:

- [Crie seu primeiro token](https://solange.dev/2020/2020-04-26-Rsk-CreateToken/)
- [Crie seu token colecionável na rede RSK](https://solange.dev/2020/2020-05-11-Rsk-CreateTokenNFT/)

Você pode publicá-los na RSK ou Ethereum :)

# Frontend

O que acontece com o frontend de uma dApp que está configurada para a rede Ethereum? Muda alguma coisa?

Não muda nada! Se a sua dApp está configurada para utilizar `Injected Web3`, ela vai continuar utilizando a conexão à carteira. Se a rede RSK está selecionada na carteira, está tudo pronto!

Caso o frontend esteja acessando um nó local, este tutorial que eu escrevi apresenta a configuração:

* [Create a frontend for smart contracts using web3 connected to a local node](https://developers.rsk.co/tutorials/frontend/frontend-web3-local/)

Está em inglês, mas se você gostaria de ler a tradução dele, é só avisar :)

# Considerações finais

Neste tutorial eu mostrei opções e configurações para publicar smart contracts Ethereum na rede RSK.

Nosso objetivo é unir forças e dar opções para as pessoas que acreditam poder dos smart contracts baseados em Ethereum e também na segurança do Bitcoin, fazer isso na rede RSK.

Espero que esse tutorial tenha sido útil e agradeço caso tenha algum feedback para mim. Compartilhe o artigo caso tenha gostado :)

Se quiser aprender com videos, assine meu canal: 
<a href="https://www.youtube.com/user/solangegueiros" target="_blank"> youtube Solange Gueiros</a>
