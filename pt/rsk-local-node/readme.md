# Como executar um nó local do Blockchain RSK

Como executar um nó local (regtest) do Blockchain RSK utilizando Java."

![Title](/images/image-00.png)

Para desenvolver smart contracts ganha-se muito tempo trabalhando em um Blockchain executado localmente. Esta é a melhor forma para desenvolver os projetos e executar testes. Nós vamos executar um nó local da rede RSK, também conhecido como regtest. 

Existem diversas maneiras para instalar / configurar um nó RSK. Aqui nós faremos o download de um arquivo JAR e executaremos com o Java SDK já instalado.

Não faz diferença se você está começando do zero ou já é um desenvolvedor, você não precisa ser um expert para fazer este tutorial. 

# Overview

Aqui está um resumo das etapas que faremos para criar nosso projeto:

1. Pré-requisitos;
2. Download;
3. Verificação de autenticidade;
4. Execução;
3. Testes da conexão;

## Video

Caso prefira assistir um video, eu ministrei um
<a href="https://www.youtube.com/watch?v=NZPEGV59vqk" target="_blank">webinar</a>
sobre este tutorial.

# Pré-requisitos

* POSIX compliant shell
* Curl
* Java

## Shell compatível POSIX

**Portable Operating System Interface (POSIX)** é uma família de padrões especificados pela IEEE Computer Society para manter a compatibilidade entre sistemas operacionais. POSIX define a interface de programação para a aplicação (Application Programming Interface - API) para terminais de comandos e interfaces de utilitários, de modo que exista compatibilidade entre diversas variantes de Unix e outros sistemas operacionais. Fonte: [Wikipidia](https://en.wikipedia.org/wiki/POSIX)

* Mac OSX e distribuições Linux: use o terminal standard
* Windows: Se você utilizar o terminal padrão (cmd) ou PowerShell, os comandos podem não funcionar.
  Considere instalar [Git para Windows](https://gitforwindows.org/), que inclui o terminal Git Bash. 
  Aqui está um [tutorial on installing and using Git Bash](https://www.atlassian.com/git/tutorials/git-bash) (em inglês).

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

# Download 

Vá em [releases page](https://github.com/rsksmart/rskj/releases) e clique na versão mais recente para fazer o download.

Você precisa selecionar o arquivo JAR, que está no final da publicação da última versão. 
Seu nome é algo parecido com `rskj-core-*.jar`:

![Download last RSK release](/images/image-03.png)

# Verificação de autenticidade

Quando instalamos e executamos um nó RSKj, é recomendável verificar se sua cópia é legítima.

No diretório onde foi salvo o arquivo JAR, vá em um terminal POSIX e execute este comando:

```shell
sha256sum rskj-core-2.0.1-PAPYRUS-all.jar
```

Para esta versão, o resultado será este:

```shell
43149abce0a737341a0b063f2016a1e73dae19b8af8f2e54657326ac8eedc8a0 *rskj-core-2.0.1-PAPYRUS-all.jar
```

![Verify authenticity](/images/image-04.png)

> Se você estiver utilizando o SO Windows, você precisa de um terminal POSIX.
> Veja instruções sobre [Git Bash](#shell-compatível-posix) acima. 

Para mais informações sobre como verificar se sua cópia é original, 
incluindo verificação de assinaturas, veja as 
[instruções em inglês](https://developers.rsk.co/rsk/node/security-chain/ "Verify authenticity of RskJ source code and its binary dependencies").

# Execução

Para executar o nó:

```shell
java -cp <PATH-TO-THE-RSKJ-JAR> -Drpc.providers.web.cors=* co.rsk.Start --regtest
```

(Altere <PATH-TO-THE-RSKJ-JAR> para a localização do seu arquivo JAR).

Eu estou utilizando o sistema operacional (SO) Windows e salvei o arquivo JAR em  `C:\RSK\node`,
portanto o caminho completo do meu arquivo é 
`C:\RSK\node\rskj-core-2.0.1-PAPYRUS-all.jar`.

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

![Run local node](/images/image-05.png)

**Importante:**

> Não feche o terminal / janela do console. 
> O nó está sendo executado nesta janela e, se você fechar, vai encerrar a execução.

# Testes da conexão

Verifique se o nó está funcionando utilizando cURL.

Abra uma nova janela de terminal.

Faça uma consulta ao servidor HTTP RPC do nó. Este é um exemplo usando cURL:

```shell
curl localhost:4444/1.1.0/ -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

![local node eth_blockNumber](/images/image-06.png)

A resposta será algo parecido com:

```json
{"jsonrpc":"2.0","id":1,"result":"0x2991b"}
```

A propriedade `result` está retornando o número do último bloco sincronizado. 
No nosso caso, que é um nó local isolado, é o último bloco minerado. 
Perceba que o valor `0x2991b` está em hexadecimal (base 16). No exemplo acima, o número do bloco é `170267` em decimal (base 10).

Para saber mais (em inglês):
[Setup RSKj with Java](https://developers.rsk.co/rsk/node/install/java/)

Se você tiver algum problema, verifique se o seu sistema atende os pré-requisitos [minimum requirements](https://developers.rsk.co/rsk/node/install/requirements/).

Existem outras formas de instalar um nó RSK, em outras plataformas: [installing RSKj](https://developers.rsk.co/rsk/node/install/)

# Próximos passos

Parabéns! Seu nó `regtest` está sendo executado localmente. 

Você está pronto para ir para os próximos tutoriais onde faremos projetos bem legais!

Escolha um destes para começar:

- [Aprender como conectar o Geth e interagir com o nó local](https://solange.dev/2020/2020-04-05-Rsk-GethAttachLocalNode/)
- [Conectar o Remix e publicar smart contracts](/2020/rsk-remix-local-node/)
- [Utilizar o Truffle para fazer testes localmente](https://solange.dev/2020/rsk-truffle-tests/)
