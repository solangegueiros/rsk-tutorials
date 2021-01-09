# Como criar um projeto utilizando Truffle e OpenZeppelin conectado à rede RSK

Como criar um projeto utilizando Truffle framework e a biblioteca de smart contracts da Open Zeppelin conectado tanto a um nó local quanto a testnet da RSK.

![Title](/images/image-00.png)

Neste tutorial eu mostrarei passo-a-passo como criar um novo projeto blockchain, utilizando Truffle framework e a biblioteca de smart contracts da Open Zeppelin, conectado tanto a um nó local quanto a testnet da RSK.

Não faz diferença se você está começando do zero ou já é um desenvolvedor, você não precisa ser um expert para fazer este tutorial. 

# Overview

Aqui está um resumo das etapas que faremos para criar nosso projeto:

1. Instalar pré requisitos;
2. Executar um nó local da RSK (regtest);
3. Inicializar um projeto utilizando Truffle;
4. Instalar os smart contracts da Open Zeppelin smart contracts no projeto;
5. Instalar HD wallet provider;
6. Criar um mnemônico para uma carteira;
7. Configurar Truffle para conectar às redes RSK;
10. Usar o Truffle console;
11. Adquirir alguns tR-BTCs no faucet;

# Pré-requisitos

* Git
* POSIX compliant shell
* Curl
* Java
* Node.js e NPM (Node Package Manager)
* Editor: Visual Studio Code (VSCode) ou outro editor de sua escolha
* Extensão VSCode para a linguagem Solidity
* Truffle

## Git

Git é um sistema open-source de controle de versão. Alguns pacotes que serão instalados posteriormente utilizam o Git internamente para fazer download de suas versões corretas.
Além disto, no sistema operacional (SO) Windows, é instalado com ele um terminal POSIX, chamado `Git Bash`, que é o próximo pré-requisito. 

Instale o [Git para Windows](https://gitforwindows.org/), que inclui o terminal Git Bash. 

Este [tutorial on installing and using Git Bash](https://www.atlassian.com/git/tutorials/git-bash) (em inglês) também pode ajudar.

## Shell compatível POSIX

**Portable Operating System Interface (POSIX)** é uma família de padrões especificados pela IEEE Computer Society para manter a compatibilidade entre sistemas operacionais. POSIX define a interface de programação para a aplicação (Application Programming Interface - API) para terminais de comandos e interfaces de utilitários, de modo que exista compatibilidade entre diversas variantes de Unix e outros sistemas operacionais. Fonte: [Wikipidia](https://en.wikipedia.org/wiki/POSIX)

* Mac OSX e distribuições Linux: use o terminal standard
* Windows: Utilize o terminal `Git Bash`, instalado juntamente com o `Git` no passo anterior. Se você utilizar o terminal padrão (cmd) ou PowerShell, os comandos podem não funcionar. 
  
## cURL

Este é um sistema de comandos geralmente instalado no seu sistema operacional.

Se o comando `curl --version` apresentar um erro, [download curl](https://curl.haxx.se/download.html).

## Java

É necessário ter Java para executar o RSKj. 

Verifique se você já tem o Java runtime instalado:

```shell
java -version
```

Se retornar uma versão, isto significa que você já fez a instalação antes.

![java -version](/images/image-01.png)

Caso precise instalar, vá em [Java Download](https://www.java.com/en/download/)

![Java Download](/images/image-02.png)

### Para Mac OSX e Linux 

Existem diversas formas de fazer isto. SDKman é uma delas, e possibilita a instalação e seleção de diferentes versões:

```shell
curl -s "https://get.sdkman.io/" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
# to get a filtered list of available java versions
sdk list java  | grep "8\." # copy a selection for use below

# install the version of java copied above
# (replace accordingly)
sdk install java 8.0.242.j9-adpt

# show installed versions, and switch to the selected one
# (replace accordingly)
sdk list java | grep installed
sdk use java 8.0.242.j9-adpt
java -version
```

## Node.js e NPM

Outra dependência é NPM, que é instalado com Node.js.

Pra verificar se Node.js e NPM já estão instalados, verifique se os comandos abaixo funcionam no terminal:

```shell
node --version
npm --version
```

![node and npm version](/images/image-03.png)

Vá em [Node.js](https://nodejs.org/en/) caso precise instalá-los.

Caso queira ter mais de uma versão do node instalada, utilize o gerenciador de versões para o node, chamado [nvm](https://github.com/nvm-sh/nvm).

## Editor para código-fonte

Precisamos de algum editor de código, de preferência um que destaque as linguagens Solidity e Javascript.
Neste tutorial estou utilizando VSCode para criar os arquivos do projeto.

[VS Code](https://code.visualstudio.com/) é uma boa escolha.

### Visual Studio Code (VS Code)

Neste tutorial estou utilizando VSCode para criar os arquivos do projeto.

Para instalar, [faça o download aqui](https://code.visualstudio.com/download).

Verifique se a instalação do VS code está ok consultando sua versão no terminal:

```shell
code -v
```

![vscode version](/images/image-04.png)

## Extensão VSCode para a linguagem Solidity

Se utilizar o VSCode, vá em Extensions (Menu View -> Extensions).

1. Digite `solidity` no campo de pesquisa.
2. Selecione a extensão `solidity` do Juan Blanco.
3. Clique em `install`.

![vscode version](/images/image-42.png)

## Truffle

[Truffle](https://www.trufflesuite.com/truffle) é um conhecido framework para desenvolvimento de smart contracts, que facilita a vida do desenvolvedor.
Entre suas características, podemos citar o gerenciamento da "vida" de um smart contract (você pode fazer várias publicações e saber qual foi a última), desenvolvimento de scripts para deploy, testes automatizados e gerenciamento de rede simplificado.

Da mesma forma também facilita o desenvolvedor RSK porque podemos configurar as redes RSK no Truffle.

Para instalar Truffle, no terminal, digite o comando abaixo e pressione a tecla `enter`:

```shell
npm install -g truffle
```

![truffle install](/images/image-05.png)

Quando a instalação finalizar, feche a janela do terminal e abra novamente para verificar a versão do Truffle:

```shell
truffle version
```

![truffle version](/images/image-06.png)

# Nó local RSK - regtest

Quando desenvolvemos um projeto utilizando o framework Truffle, precisamos de um blockchain executando localmente. Esta é a melhor forma para desenvolver os projetos e executar testes. Nós vamos executar um nó local da rede RSK, também conhecido como regtest. 

Existem diversas maneiras para instalar / configurar um nó RSK. Aqui nós faremos o download de um arquivo JAR e executaremos com o Java SDK já instalado.

### Download 

Vá em [releases page](https://github.com/rsksmart/rskj/releases) e clique na versão mais recente para fazer o download.

Você precisa selecionar o arquivo JAR, que está no final da publicação da última versão. 
Seu nome é algo parecido com `rskj-core-*.jar`:

![Download last RSK release](/images/image-07.png)

### Verificar autenticidade

Quando instalamos e executamos um nó RSKj, é recomendável verificar se sua cópia é legítima.

No diretório onde foi salvo o arquivo JAR, vá em um terminal POSIX e execute este comando:

```shell
sha256sum rskj-core-2.0.1-PAPYRUS-all.jar
```

Para esta versão, o resultado será este:

```shell
43149abce0a737341a0b063f2016a1e73dae19b8af8f2e54657326ac8eedc8a0 *rskj-core-2.0.1-PAPYRUS-all.jar
```

![Verify authenticity](/images/image-08.png)

> Se você estiver utilizando o SO Windows, você precisa de um terminal POSIX.
> Veja instruções sobre [Git Bash](#shell-compatível-posix) acima. 

Para mais informações sobre como verificar se sua cópia é original, 
incluindo verificação de assinaturas, veja as 
[instruções em inglês](https://developers.rsk.co/rsk/node/security-chain/ "Verify authenticity of RskJ source code and its binary dependencies").

### Execução

Para executar o nó:

```shell
java -cp <PATH-TO-THE-RSKJ-JAR> -Drpc.providers.web.cors=* co.rsk.Start --regtest
```

(Altere <PATH-TO-THE-RSKJ-JAR> para a localização do seu arquivo JAR).

Eu estou utilizando o sistema operacional (SO) Windows e salvei o arquivo JAR em  `C:\RSK\node`,
portanto o caminho completo do meu arquivo é 
`C:\RSK\node\rskj-core-1.3.0-WASABI-all.jar`.

Então, para executar o nó RSK: 

#### Windows

```shell
java -cp C:\RSK\node\rskj-core-2.0.1-PAPYRUS-all.jar -Drpc.providers.web.cors=* co.rsk.Start --regtest
```

#### Usando Git Bash

```shell
java -cp C:/RSK/node/rskj-core-2.0.1-PAPYRUS-all.jar -Drpc.providers.web.cors=* co.rsk.Start --regtest
```

#### Linux ou Mac

```shell
java -cp ~/RSK/node/rskj-core-2.0.1-PAPYRUS-all.jar -Drpc.providers.web.cors=* co.rsk.Start --regtest
```

Se não apareceu nada depois que você executou o comando, normalmente isso quer dizer que o nó está funcionando perfeitamente. O resultado está sendo salvo no arquivo de log.

> Veja o parâmetro utilizado acima: `-Drpc.providers.web.cors=*`
> Ele desabilita a proteção de compartilhamento para origens diferentes,
> fazendo com que qualquer página possa acessar o nó.
> Como nós utilizaremos consultas JSON-RPC vindas de um browser,
> como uma DApp, precisamos ativar este flag.

Este é o resultado no terminal Windows:

![Run local node](/images/image-09.png)

**Importante:**

> Não feche o terminal / janela do console. 
> O nó está sendo executado nesta janela e, se você fechar, vai encerrar a execução.

### Verifique se o nó está funcionando utilizando cURL

Abra uma nova janela de terminal.

Faça uma consulta ao servidor HTTP RPC do nó. Este é um exemplo usando cURL:

```shell
curl localhost:4444/1.1.0/ -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

![local node eth_blockNumber](/images/image-10.png)

A resposta será algo parecido com:

```json
{"jsonrpc":"2.0","id":1,"result":"0x2991b"}
```

A propriedade `result` está retornando o número do último bloco sincronizado. 
No nosso caso, que é um nó local isolado, é o último bloco minerado. 
Perceba que o valor `0x2991b` está em hexadecimal (base 16). No exemplo acima, o número do bloco é `170267` em decimal (base 10).

Para saber mais:
[Setup RSKj with Java](https://developers.rsk.co/rsk/node/install/java/)

Se você tiver algum problema, verifique se o seu sistema atende os pré-requisitos [minimum requirements](https://developers.rsk.co/rsk/node/install/requirements/).

Existem outras formas de instalar um nó RSK, em outras plataformas: [installing RSKj](https://developers.rsk.co/rsk/node/install/)

# RSK Testnet - verificando a conexão 

Além do nó local, também vamos publicar smart contracts na testnet RSK. Antes disto, vamos verificar se a conexão está ok.

Este é um exemplo utilizando cURL. Execute o comando abaixo no terminal. 

Se você estiver utilizando um computador com sistema operacional Windows, não funciona no terminal, minha sugestão é utilizar o Git Bash. Para instalar, vá ao [Git site](https://git-scm.com/).

```shell
curl https://public-node.testnet.rsk.co/2.0.1/ -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

Esta consulta retorna qual o número do último bloco minerado.

A resposta será algo parecido com:

```json
{"jsonrpc":"2.0","id":1,"result":"0xc3f9b"}
```

![testnet eth_blockNumber](/images/image-11.png)

O campo `result` está apresentado em hexadecimal. `0xc3f9b` é o número do bloco, e seu equivalente em decimal é  `802715`.  

Você pode consultar o [testnet explorer](https://explorer.testnet.rsk.co/) e validar que é o mesmo número do bloco:

![explorer testnet block number](/images/image-12.png)

# Inicialize um projeto Truffle 

Há duas opções para inicializar um projeto:

1. Um modelo de projeto vazio
2. Um projeto baseado em um Truffle Box

## 1 - Inicialize um novo projeto Truffle 

Crie um novo diretório, `myproject`, por exemplo, e vá para a pasta no terminal.

```shell
mkdir myproject
cd myproject
```

Por exemplo, eu vou criar em `C:\RSK\` (Estou utilizando Windows).

Meu projeto estará localizado no diretório `C:\RSK\myproject`.

![myproject folder](/images/image-13.png)

Na pasta do seu projeto, inicialize um projeto Truffle:

```shell
truffle init
```

![truffle init](/images/image-14.png)

Abra a pasta no VSCode. 
Você verá uma estrutura de diretórios como esta:

![truffle file structure](/images/image-15.png)

* `./contracts`: Todos os smart contracts serão salvos nesta pasta.
* `./migrations`: Os scripts para publicação ficarão armazenados aqui.
* `./test`: Aqui serão salvos os scripts para testes.
* `./truffle-config.js`: Este é o arquivo de configuração do Truffle. Aqui vamos configurar as redes, incluindo RSK.

Veja que os seguintes arquivos também foram criados:

* `Migrations.sol`: Smart contract que registra todos as publicações realizadas em uma rede.
* `1_initial_migration.js`: Publicação do `Migrations.sol`.

### Inicialize um projeto npm

Quando inicializamos um projeto Truffle a partir do template, também precisamos inicializar um projeto npm.

Para inicializar um projeto npm na pasta `myproject`,  execute o comando abaixo no terminal:

```shell
npm init -y
```

![npm init](/images/image-16.png)

## 2 - Inicialize um projeto baseado em um Truffle Box

> Você só precisa fazer esta parte se não escolheu a opção 1.

Truffle Boxes são modelos. 
Além dos arquivos do Truffle, 
Truffle Boxes podem conter outros módulos úteis, como smart contracts Solidity, bibliotecas, páginas front-end e mais.

Na opção 1, quando usamos `truffle init`, estamos utilizando um tipo especial de Truffle box. 
Conheça outros [boxes] (https://www.trufflesuite.com/boxes).

Também temos alguns configurados para a RSK: [RSK truffle boxes](https://developers.rsk.co/tutorials/truffle-boxes/) (em inglês).

## Instale Open Zeppelin

[OpenZeppelin Contracts](https://openzeppelin.com/contracts/) é um conjunto de bibliotecas para smart contracts desenvolvidos em Solidity. Eles também funcionam em outros blockchains, como a **RSK**. 

Vale ressaltar que essas bibliotecas foram revisadas e auditadas visando altos padrões de segurança, para que os contratos que dependam delas sejam menos suscetíveis a hackers quando usados corretamente.

No terminal, no diretório `myproject`, instale as biblitecas OpenZeppelin com este comando:

```shell
npm install -E @openzeppelin/contracts@2.5.0
```

A opção `-E` é para salvar no arquivo de configuração npm as dependencias na versão definida na instalação, e não com a versão default.

![openzeppelin install](/images/image-17.png)

> Os smart contracts podem ser alterados de uma versão para outra, então é importante fixar a versão porque nosso tutorial foi escrito utilizando esta versão.

# Instale o HD wallet provider

Para conectar a rede RSK network, utilizaremos um pacote provedor que possibilita a conexão a qualquer rede desbloqueando uma conta localmente. 
Utilizaremos [@truffle/hdwallet-provider](https://www.npmjs.com/package/@truffle/hdwallet-provider). 
Este provedor possibilita assinar transações de endereços gerados a partir de um mnemônico com 12 ou 24 palavras.

> Precisa ter instalado Node >= 7.6.

No terminal, na pasta `myproject`, instale com este comando:

```shell
npm install -E @truffle/hdwallet-provider@1.0.34
```

![hd wallet provider install](/images/image-18.png)

Este `truffle package` tem muitas dependências.
Pode demorar um pouco até aparecer a mensagem `successful installation`.

![hd wallet provider successful installation](/images/image-19.png)

# Verifique o arquivo package.json

`package.json` é um arquivo de configurações que criamos na inicialização do projeto, com o comando `npm init -y`. 

Depois das instalações, na pasta `Token`, abra o arquivo `package.json` no VSCode, e verifique a parte das dependências (dependencies):

![package.json](/images/image-20.png)

# Crie uma wallet

Para utilizar a testnet, precisamos de tR-BTC e um endereço / wallet para armazená-los. 
A melhor forma de criar uma wallet é a partir de um mnemônico, utilizando o padrão definido em [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)

Existem algumas formas de fazer isto.

Uma delas é criar com uma wallet web, como [Metamask](https://metamask.io/) ou [Nifty](https://www.poa.network/for-users/nifty-wallet). 
Estas carteiras geram o mnemônico para você.
Se quiser criar utilizando Metamask, veja as instruções aqui:

- [Remix and Metamask with RSK testnet](https://solange.dev/2020/2020-03-27-Rsk-RemixMetamask/)

## iancoleman.io/bip39

Outra maneira é com este web app: 

[iancoleman.io/bip39](https://iancoleman.io/bip39/)

> Neste tutorial, o método utilizado para guardar o mnemônico não é recomendado para uma carteira com fundos de verdade, que valem dinheiro real, porque não é tão seguro gerar um mnemônico e chaves privadas em um website.
> Para fins educacionais podemos utilizar aqui, dado que vamos nos conectar à testnet.

No campo `Generate a random mnemonic` , selecione `12 words` e clique no botão `generate`.

![Generate a random mnemonic](/images/image-21.png)

O resultado estará no campo `BIP39 Mnemonic`. 
São 12 palavras aleatórias, como da figura abaixo:

![BIP39 Mnemonic](/images/image-22.png)

Meu mnemônico é: 

```
energy knife ice mouse merge track cram brown decorate atom rule virus
```

Copie estas 12 palavras para utilizá-las daqui há pouco.

## Ferramenta mnemonics

Outra alternativa é utilizar o pacote [mnemonics](https://github.com/itinance/mnemonics), 
que é um simples utilitário para gerar [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) mnemônicos.

Para instalar `mnemonics` globalmente, digite o comando abaixo no terminal e pressione a tecla `enter` na pasta do seu projeto:

```shell
npm install -g mnemonics@1.1.3
```

Use este comando para criar um mnemônico BIP39 de 12 palavras:

```shell
mnemonics > .secret
```

O novo mnemônico está salvo no arquivo `.secret`, o que é a próxima etapa.

# Crie o arquivo .secret

No terminal, no diretório `myproject`, crie um arquivo chamado `.secret`.

Lembra do seu mnemônico? 
Cole seu mnemônico neste arquivo e salve.

![dot secret](/images/image-23.png)

# Configure Truffle para se conectar à redes RSK

Abra o arquivo `truffle-config.js` no seu projeto Truffle e sobrescreva com estas instruções:

```javascript
const HDWalletProvider = require('@truffle/hdwallet-provider');

const fs = require('fs');
const mnemonic = fs.readFileSync(".secret").toString().trim();
if (!mnemonic || mnemonic.split(' ').length !== 12) {
  throw new Error('unable to retrieve mnemonic from .secret');
}

module.exports = {
  networks: {
  },
  compilers: {
    solc: {
    }
  }
}
```

O `hdwallet-provider` nos habilita a conectar com qualquer rede, desbloqueando uma conta localmente, a partir do mnemônico. Isto inclui as redes da RSK. 

Também estamos fazendo a leitura do mnemônico armazenado no arquivo `.secret`, e salvando na variável mnemonic.

## Configure Truffle para se conectar à RSK regtest (local node)

No arquivo `truffle-config.js`, adicione esta configuração na seção `network`:

```javascript
    development: {
      host: "127.0.0.1",
      port: 4444,
      network_id: "*"
    },  
```

Este é o resultado:

![network development](/images/image-24.png)

## Verifique o gas price na testnet

Consulte o valor atual do gas price para a testnet network, e salve no arquivo `.gas-price-testnet.json`. 

No diretório do projeto, execute este comando cURL:

```shell
curl https://public-node.testnet.rsk.co/2.0.1/ -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":1}' > .gas-price-testnet.json
```

![gas price result](/images/image-25.png)

Você vai receber um retorno parecido com este no arquivo:

```json
{"jsonrpc":"2.0","id":1,"result":"0x3938700"}
```

![gas-price-testnet.json](/images/image-26.png)

O resultado está apresentado em hexadecimal.

Altere o arquivo `truffle-config` novamente para incluir a atualização do gas price. 
Depois da parte mnemônico, inclua estas instruções:

```javascript
const gasPriceTestnetRaw = fs.readFileSync(".gas-price-testnet.json").toString().trim();
const gasPriceTestnet = parseInt(JSON.parse(gasPriceTestnetRaw).result, 16);
if (typeof gasPriceTestnet !== 'number' || isNaN(gasPriceTestnet)) {
  throw new Error('unable to retrieve network gas price from .gas-price-testnet.json');
}
console.log("Gas price Testnet: " + gasPriceTestnet);
```

## Configure Truffle para se conectar à RSK testnet

No arquivo `truffle-config.js`, inclua esta configuração na seção `network`:

```javascript
    testnet: {
      provider: () => new HDWalletProvider(mnemonic, 'https://public-node.testnet.rsk.co/2.0.1/'),
      network_id: 31,
      gasPrice: Math.floor(gasPriceTestnet * 1.1),
      networkCheckTimeout: 1e9
    },
```

## Truffle configurado para RSK testnet e regtest (nó local)

Este é a versão final do arquivo `truffle-config.js` com as configurações para as duas networks:

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
 
module.exports = {
  networks: {
    development: {
      host: "127.0.0.1",
      port: 4444,
      network_id: "*"
    },
    testnet: {
      provider: () => new HDWalletProvider(mnemonic, 'https://public-node.testnet.rsk.co/2.0.1/'),
      network_id: 31,
      gasPrice: Math.floor(gasPriceTestnet * 1.1),
      networkCheckTimeout: 1e9
    },        
  },
  compilers: {
    solc: {
    }
  }
}
```

Confira na imagem do VS Code:

![truffle-config](/images/image-27.png)

# Truffle Console conectado à RSK network

Truffle tem seu próprio console para executar comandos e pode ser conectado a qualquer rede previamente configurada no arquivo `truffle-config.js`.

## Conecte ao nó local RSK (regtest)

Vamos abrir o console Truffle para conectar ao nó local.

No terminal, na pasta `myproject`, execute este comando:

```shell
truffle console
```

![Truffle console development](/images/image-28.png)

> Qualquer rede definida com o nome `development` será considerada a rede padrão. 

## Conecte à testnet RSK 

Até agora, nos conectamos a um blockchain que funciona com apenas 1 nó e que roda em seu próprio computador.
Vamos interagir com um blockchain "real", que está sendo executado em vários nós distribuídos em vários computadores!

Para conectar o Truffle console em outra rede, você precisa especificar a rede:

No terminal, na pasta `myproject`, execute este comando:

```shell
truffle console --network testnet
```

Demora um pouco mais para estabelecer a conexão, se compararmos com o nó local.
Vai abrir um novo console:

![truffle console network testnet](/images/image-29.png)

## Teste a conexão com a rede RSK

Em qualquer uma das redes, execute este comando no Truffle console:

### Block number

Apresenta o número do último bloco minerado.

```javascript
(await web3.eth.getBlockNumber()).toString()
```

![getBlockNumber](/images/image-30.png)

### Network ID

Para saber o ID da rede, execute este comando:

```javascript
(await web3.eth.net.getId()).toString()
```

Para o nó local, o ID da rede é `33`.

![getId local](/images/image-31.png)

E para a testnet, é `31`.

![getId testnet](/images/image-32.png)

## Sair do Truffle console

No console Truffle, execute este comando para sair do terminal:

```shell
.exit
```

![exit Truffle console](/images/image-33.png)

# Saiba os endereços da sua wallet

Vamos executar um comando no Truffle console para recuperar os 10 primeiros endereços da nossa HD (hierarchical deterministic) wallet para a RSK Testnet, que é gerado a partir do nosso mnemônico.

No terminal, na pasta `myproject`, vá para o Truffle console conectado a testnet:

```shell
truffle console --network testnet
```

E execute este comando para salvar os endereços na variável `accounts`:

```javascript
const accounts = Object.keys(web3.currentProvider.wallets)
```

Veja os endereços com este comando:

```javascript
accounts
```

![list accounts](/images/image-34.png)

Vamos salvar os endereços em um arquivo chamado `.accounts`

```javascript
await require('fs').promises.writeFile('.accounts', accounts.join('\n'))
```

![create file .accounts](/images/image-35.png)

Agora podemos ver o arquivo:

![file .accounts](/images/image-36.png)

## Consulte o saldo

Para consultar o saldo de uma conta, por exemplo, da primeira conta de nossa lista (`account[0]`), execute este comando no Truffle console:


```javascript
(await web3.eth.getBalance(accounts[0])).toString()
```

![getBalance accounts 0](/images/image-37.png)

O saldo é 0 e precisamos de tR-BTC para pagar o gas utilizado para publicar smart contracts e interagir com eles. Vamos obtê-los no próximo passo.

# TestNet Faucet

Você pode receber alguns Testnet R-BTC em 

[RSK Testnet faucet](https://faucet.testnet.rsk.co/).

Copie o primeiro endereço do arquivo `.accounts`. No meu caso, é:

```
0xe16f6abdd5815f3d24b4e5c29138f863933b000a
```

Cole seu endereço (que foi copiado no passo anterior) e faça a a verificação do CAPTCHA.

![faucet.testnet.rsk.co](/images/image-38.png)

Espere alguns segundos...

![Wait a few seconds](/images/image-39.png)

![Received some R-BTCs](/images/image-40.png)

Você pode ver o hash da transação, por exemplo, eu fiz esta:

[0x4a2bf1f65c525219020c3a1215a29453c20f4ced90575d9a7d13f8fe666d05b4](https://explorer.testnet.rsk.co/tx/0x4a2bf1f65c525219020c3a1215a29453c20f4ced90575d9a7d13f8fe666d05b4)

E agora eu tenho 0.05 tR-BTC!

## Consulte o saldo de novo

Verifique o saldo de nossa conta novamente. Execute este comando no Truffle console:

```javascript
(await web3.eth.getBalance(accounts[0])).toString()
```

![getBalance accounts 0 again](/images/image-41.png)

Agora eu tenho 50000000000000000, o que significa 0.05 com 18 casas decimais de precisão.

# Próximos passos

Neste ponto, nós temos instalados todos os pré-requisitos e criamos um template de projeto vazio, utilizando o framework Truffle e a biblioteca de smart contracts da Open Zeppelin, conectado tanto a um nó local (Regtest) quanto a testnet da RSK.

Nós não construímos nada ainda, mas você está pronto para ir para os próximos tutoriais onde faremos projetos bem legais!

Escolha um destes para começar:

- [Crie seu primeiro token](https://solange.dev/2020/2020-04-26-Rsk-CreateToken/)
- [Crie seu token colecionável na rede RSK](https://solange.dev/2020/2020-05-11-Rsk-CreateTokenNFT/)
