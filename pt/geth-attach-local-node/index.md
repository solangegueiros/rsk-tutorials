---
title: Utilizando Geth para conectar em um nó local RSK
date: "2020-04-05T22:00:00.000Z"
description: "Como utilizar o client Ethereum Geth, para conectar em um nó local da RSK(chamado regtest) e executar alguns comandos JSON-RPC.
"
tags: tutorial, rsk, geth, ethereum, rskj
type: blog
---

![Title](/images/image-00.png)

A implementação da máquina virtual da RSK é compatível com a máquina virtual Ethereum (Ethereum Virtual Machine - EVM), o que nos permite fazer uso de muitas ferramentas dos desenvolvedores Ethereum.

Neste tutorial mostrarei passo a passo como usar o client Ethereum Geth para conectar-se a um nó local da RSK (chamado regtest) e executar alguns comandos JSON-RPC.
Isso é muito útil para testar a execução de um nó local.

## Overview

Executaremos estas etapas:

1. Instalar os pré-requisitos: Java SDK, nó da RSK e Geth;
2. Conectar o Geth ao nó;
3. Verificar se o nó está sendo executado;
4. Conhecer algumas funções úteis;
5. Aprender como interagir com as contas: listar e criar;
6. Transferir fundos entre contas.

## Video

Caso prefira assistir um video, eu ministrei um workshop online sobre este tutorial:
<a href="https://www.youtube.com/watch?v=TP6pxdwoHT0" target="_blank"> clique aqui</a>

## Pré-requisitos

- Java JDK
- nó local RSK
- Geth

### Instalando Java JDK

Primeiro verifique se você já tem o Java runtime instalado:

```shell
java -version
```

Se retornar uma versão, isto significa que você já fez a instalação antes.

Caso precise instalar, vá em [Java Download](https://www.java.com/en/download/)

![Java Download](/images/image-01.png)

### Instalando o nó local RSK

Existem diversas maneiras para instalar / configurar um nó RSK. Aqui nós faremos o download de um arquivo JAR e executaremos com o Java SDK já instalado.

#### Download

Vá em [releases page](https://github.com/rsksmart/rskj/releases) e clique na versão mais recente para fazer o download.

Você precisa selecionar o arquivo JAR, que está no final da publicação da última versão. 
Seu nome é algo parecido com `rskj-core-*.jar`:

![Download last RSK release](/images/image-02.png)

#### Execução

Para executar o nó:

```shell
java -cp <PATH-TO-THE-RSKJ-JAR> co.rsk.Start --regtest
```

(Altere <PATH-TO-THE-RSKJ-JAR> para a localização do seu arquivo JAR).

Eu estou utilizando o sistema operacional (SO) Windows e salvei o arquivo JAR em  `C:\RSK\node`,
portanto o caminho completo do meu arquivo é 
`C:\RSK\node\rskj-core-1.3.0-WASABI-all.jar`.

Então, para executar o nó RSK: 

#### Windows

```shell
java -cp C:\RSK\node\rskj-core-1.3.0-WASABI-all.jar co.rsk.Start --regtest
```

#### Linux ou Mac

```shell
java -cp C/RSK/node/rskj-core-1.3.0-WASABI-all.jar co.rsk.Start --regtest
```

Se não apareceu nada depois que você executou o comando, normalmente isso quer dizer que o nó está funcionando perfeitamente. Nós confirmaremos isso na próxima etapa.

![Run local node](/images/image-03.png)

**Importante:**

> Não feche o terminal / janela do console. O nó está sendo executado nesta janela e, se você fechar, vai encerrar a execução.

### Verifique se o nó está funcionando utilizando cURL

Abra uma nova janela de terminal.

Faça uma consulta ao servidor HTTP RPC do nó. Este é um exemplo usando cURL:

```shell
curl localhost:4444/1.1.0/ -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

A resposta será algo parecido com:

```shell
{"jsonrpc":"2.0","id":1,"result":"0xfc0"}
```

A propriedade `result` está retornando o número do último bloco sincronizado. 
No nosso caso, que é um nó local isolado, é o último bloco minerado. 
Perceba que o valor está em hexadecimal. No exemplo acima, o número do bloco é 4032.

Para saber mais:
[Setup RSKj with Java](https://developers.rsk.co/rsk/node/install/java/)

Se você tiver algum problema, verifique se o seu sistema atende os pré-requisitos [minimum requirements](https://developers.rsk.co/rsk/node/install/requirements/).

Existem outras formas de instalar um nó RSK, em outras plataformas: [installing RSKj](https://developers.rsk.co/rsk/node/install/)

### Instalando Geth

[Download](https://geth.ethereum.org/downloads/) e instale [Geth](https://geth.ethereum.org/).

![Geth download page](/images/image-04.png)

Execute a instalação com as opções padrão.

> Você não precisa instalar as Developer tools.

![Geth install](/images/image-05.png)

> Este tutorial foi escrito utilizando a versão 1.9.12, Eu recomendo utilizar esta versão.

Para ter certeza que a instalação está ok, no terminal, execute este comando para verificar a versão, se ele retornar uma versão do Geth, está ok:

```shell
geth version
```

![geth version](/images/image-06.png)

Neste link, você tem mais informações para instalar o Geth:
[https://geth.ethereum.org/docs/install-and-build/installing-geth](https://geth.ethereum.org/docs/install-and-build/installing-geth)

## Geth attach

Este procedimento é apenas para um nó local, em execução na sua máquina ou em uma rede que você tem permissão. O Geth attach fornece controle total da instância remota; portanto, não espere que alguém lhe dê um acesso deste tipo.

```shell
geth attach http://127.0.0.1:4444
```

![image alt text](/images/image-07.png)

Estamos executando geth para fazer um attach (conectar) a um nó local RSK. O endereço  `http://127.0.0.1` conecta com seu próprio computador (localhost). O nó local, por padrão, tem o servidor HTTP-RPC habilitado e escutando na porta `4444`, então estamos conectando nesta porta.

### Métodos RPC suportados

Aqui está uma lista com todos os métodos RPC ativados nos nós RSK:
[RSK JSON-RPC](/rsk/node/architecture/json-rpc/)

Para mais informações sobre métodos RPC, consulte o site Ethereum, mas lembre-se de que nem todos estão implementados nos nós RSK:

- [geth.ethereum.org/docs/rpc/server](https://geth.ethereum.org/docs/rpc/server)
- [github.com/ethereum/wiki/wiki/JSON-RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC)

Verifique se o nó está funcionando utilizando cURL

### Verifique o número do bloco

```js
eth.blockNumber
```

![eth.blockNumber](/images/image-08.png)

Toda vez que você executa, obtém um número maior, dado que o nó está minerando novos blocos.

## Dicas especiais

### Copiar e colar nos terminais SO Windows

No console Geth console, para colar alguma coisa que você copiou de outro lugar, utilize:

- Botão direito
- Seta para direita

Não pressione as duas teclas ao mesmo tempo, mas sim em sequencia: primeiro `Botão direito`, depois `Seta para direita`.

### Listar comandos

Uma dica para listar os comando disponíveis. Tecle 2 espaços e a tecla TAB duas vezes.
Este é o resultado:

![Geth list commands](/images/image-09.png)

## Funcões úteis

Demonstrarei alguns métodos RPC que são úteis para entender como tudo as coisas funcionam.

## Funcões úteis eth

### eth.blockNumber

Shows the last block number

```js
eth.blockNumber
```

![eth.blockNumber](/images/image-10.png)

### gasPrice

No nó local da RSK, o gas price é 0.

```js
eth.gasPrice
```

![eth.gasPrice](/images/image-11.png)

## Funcões úteis net

### net.version

```js
net.version
```

Vai retornar o ID da rede.

![net.version](/images/image-12.png)

### net.peerCount

```js
net.peerCount
```

![net.peerCount](/images/image-13.png)

Você está sozinho nesta rede.

## Contas

### Personal

Lista tudo relacionado às contas no seu nó local.

```js
personal
```

O nó RSK vem configurado com algumas contas:

![Personal](/images/image-14.png)

### Listar contas

Este comando lista apenas as contas:

```js
personal.listAccounts
```

![personal.listAccounts](/images/image-15.png)

Este outro comando faz o mesmo:

```js
eth.accounts
```

![eth.accounts](/images/image-16.png)

### Criando uma conta

É possível criar contas novas:

```js
personal.newAccount("minhasenha")
```

É importante salvar ou guardar sua senha, ela é utilizada para criptografar sua chave privada no computador.

No exemplo acima, a senha é `minhasenha`.

![personal.newAccount](/images/image-17.png)

Este é o endereço, ou chave pública, da minha nova conta
`0xf6e443fd1c869c6a25d18a9866f3a6c7f8dfb703`

### Saldos

Para saber o saldo de uma conta, por exemplo, da `account[1]`:

```js
eth.getBalance(eth.accounts[1])
```

![eth.getBalance](/images/image-18.png)

O retorno é um número muito grande porque está em wei, que é a menor unidade do Ether. Podemos convertê-lo para Ether:

```js
web3.fromWei(eth.getBalance(eth.accounts[1]),"ether")
```

![balance in ethers](/images/image-19.png)

Em um nó local, as contas pré-configuradas tem muito “dinheiro”! Na rede da RSK, é chamado `R-BTC`. Portanto eu tenho 1.000.000.000.000 = Um trilhão de R-BTC!

### Saldo de uma conta específica

Eu quero saber o saldo da conta (`0xf6e443fd1c869c6a25d18a9866f3a6c7f8dfb703`) que eu criei anteriormente:

```js
web3.fromWei(eth.getBalance("0xf6e443fd1c869c6a25d18a9866f3a6c7f8dfb703"),"ether")
```

![Balance of a specific account](/images/image-20.png)

Eu não tenho nada na minha conta, então o próximo passo é transferir alguns R-BTC para esta conta.

## Transferindo R-BTC

Eu tenho um trilhão de R-BTC na conta 1 e nada na conta nova. Eu quero transferir 300 bilhões da conta 1 para a nova conta:

```js
eth.sendTransaction({from:eth.accounts[1], to:"0xf6e443fd1c869c6a25d18a9866f3a6c7f8dfb703", value: web3.toWei(300000000000, "ether")})
```

![Transfer](/images/image-21.png)

Agora eu tenho um hash de transação. Isto significa que minha transação foi enviada a Blockchain e será incluída em um bloco em alguns segundos.

Agora verificarei os saldos, tanto da conta 1 quanto da nova conta:

```js
web3.fromWei(eth.getBalance(eth.accounts[1]),"ether")

web3.fromWei(eth.getBalance("0xf6e443fd1c869c6a25d18a9866f3a6c7f8dfb703"),"ether")
```

Este é o resultado:

![Balances after transfer](/images/image-22.png)

Maravilha! A nova conta tem 300 bilhões de R-BTC e a conta 1 tem 700 bilhões de R-BTC.

## Saindo do Geth

Para sair do console geth:

```js
exit
```

![exit](/images/image-23.png)

## Considerações finais

Você achou que seria tão fácil usar o Geth, que é um cliente Ethereum, para interagir com um nó local da RSK?

Podemos fazer mais coisas usando Geth, como [a publicação de um smart contract no nó local RSK usando Geth e Remix] (/ tutoriais / ethereum-devs / geth-attach-deploy-smart-contract /).

Nosso objetivo é unir forças e, através da RSK, oferecer opções  para pessoas que acreditam em contratos inteligentes baseados no Ethereum e também acreditam no poder do Bitcoin.

Espero que esse tutorial tenha sido útil e agradeço caso tenha algum feedback para mim. Compartilhe o artigo caso tenha gostado :)

Se quiser ver outros videos, assine meu canal: 
<a href="https://www.youtube.com/user/solangegueiros" target="_blank"> youtube Solange Gueiros</a>
