# Interface - Linha de Comando


Provavelmente você já aprendeu como utilizar a linha de comando para executar algumas tarefas.

Este capítulo relaciona todos os comandos disponíveis.

Para conseguir ajuda, basta digitar `composer` ou `composer list` para ver a lista de comandos. O parâmetro `--help` pode te dar informações mais relevantes.

## Opções globais

As seguintes opções estão disponiveis em todos os comandos: 

* **--verbose (-v):** Aumenta a "verbosidade" das mensagens (informações mais detalhadas)
* **--help (-h):** Exibe a ajuda
* **--quiet (-q):** Modo silencioso: nenhuma mensagem
* **--no-interaction (-n):** Não pergunta nada (sem interação)
* **--working-dir (-d):** Caso especificado, utiliza o diretório indicado como o diretório de trabalho.
* **--profile:** Exibe informações de profiling (Tempo de execução,uso de memória)
* **--ansi:** Força a saída no padrão ANSI.
* **--no-ansi:** Desabilita a saída no padrão ANSI.
* **--version (-V):** Exibe a versão instalada.

## Códigos de saída

* **0:** OK
* **1:** Erro genérico/desconhecido
* **2:** Erro de resolução de dependências

## init

No capítulo [Bibliotecas](02-libraries.md) nós aprendemos como criar o arquivo
`composer.json` manualmente. Existe também um comando chamado `init`  que faz com que essa tarefa fique mais fácil.

Quando você executa o comando, ele irá pedir para que você preencha alguns campos, enquanto lhe sugere algumas respostas mais espertas.

```sh
php composer.phar init
```

### Opções

* **--name:** Nome do Pacote
* **--description:** Descrição
* **--author:** Nome do Autor
* **--homepage:** Homepage do Autor
* **--require:** Pacote a exigir, com uma restrição de versão. Deve ser no padrão `foo/bar:1.0.0`.
* **--require-dev:** Requisitos de desenvolvimento  -  veja **--require**.
* **--stability (-s):** Valor para o campo `minimum-stability`.

## install

O comando `install` lê o arquivo `composer.json`  do diretório atual, trata as dependências e instala-as no diretório `vendor`.

```sh
php composer.phar install
```


Se há um arquivo `composer.lock` no diretório atual, serão usadas as informações de versões dos pacotes existentes neste arquivo, ao invês de 'resolve-las'. Isto garante que, todos que irão usar a biblioteca usarão as mesmas versões contidas no arquivo de lock.

Caso não exista, o composer irá criar um, de acordo com a versão mais recente de cada pacote baixado.

### Opções

* **--prefer-source:** Existem duas formas de se baixar um pacote: `source` e `dist`. Para versões estáveis, o Composer irá usar o parâmetro `dist` por padrão.
  O `source` é um controle de versão de repositório. se o  `--prefer-source` está 
  habilitado, composer irá instalar da `source` (origem), se houver. Isto é útil se você pensa em resolver um bugfix em um projeto e fazer um git clone da dependência, diretamente.
* **--prefer-dist:** É o inverso de `--prefer-source`: O Composer irá instalar do
   `dist`, se possível. Isto pode tornar as instalações mais rápidas em servidores de integração e em outros cenários onde você geralmente não roda os updates dos 'vendors'. Também é uma forma de contornar problemas com o git se você não possui um setup adequado.
* **--dry-run:** Se você quer apenas 'rodar' o instalador sem necessariamente instalar um pacote, você pode usar a opção `--dry-run`. Ela simula uma instalação e lhe mostrar o que aconteceria (caso a instalação fosse executada de fato).
* **--dev:** Instala os pacotes listados em  `require-dev` (comportamento padrão).
* **--no-dev:** Pula a instalação dos pacotes listados em `require-dev`.
* **--no-scripts:** Pula a execução dos scripts listados em  `composer.json`.
* **--no-plugins:** Desabilita os plugins.
* **--no-progress:** Remove o display de progresso (em alguns terminais/scripts, a barra de prograsso pode causar problemas, especialmente os que não lidam com o 'backspace')
* **--optimize-autoloader (-o):** Converte o autoloading PSR-0/4 para obter um autoloader mais rápido. Isto é recomendado especialmete no ambiente de produção, mas pode demandar um tempo maior de execução (esta opção não é executada, por padrão)

## update

Para obter as últimas versões das dependências e atualizar o arquivo `composer.lock`, use o comando `update`:


```sh
php composer.phar update
```

Ele irá resolver todas as dependências do projeto e gravar as versões exatas no `composer.lock`.

Se você quer apenas atualizar alguns pacotes (mas não todos), você pode listá-los:

```sh
php composer.phar update vendor/package vendor/package2
```

Você pode utilizar *wildcards* (caracteres-coringa) para atualizar alguns pacotes, de uma vez só:

```sh
php composer.phar update vendor/*
```

### Opções

* **--prefer-source:** Instala pacotes de `source` quando disponíveis.
* **--prefer-dist:** Instala pacotes de `dist` quando disponíveis.
* **--dry-run:** Executa o comando mas não faz nada (simula).
* **--dev:** Instala pacotes listados em `require-dev` (comportamento padrão).
* **--no-dev:** Pula a instalação de pacotes listados em `require-dev`.
* **--no-scripts:** Pula a execução de scripts listados no `composer.json`.
* **--no-plugins:** Desabilita os plugins.
* **--no-progress:** Remove o diplay de progresso 
* **--optimize-autoloader (-o):** Converte o autoloading PSR-0/4 para obter um autoloader mais rápido. Isto é recomendado especialmete no ambiente de produção, mas pode demandar um tempo maior de execução (esta opção não é executada, por padrão)
* **--lock:** Apenas atualiza o hash do arquivo de lock para suprimir alertas sobre a validade (out-of-date) do mesmo
* **--with-dependencies** Adiciona também todas as dependências dos pacotes para a whitelist (desde que sejam marcados como "whitelisted"). Desta forma, a atualização das mesmas é feita de forma recursiva.

## require

O comando `require` adiciona novos pacotes ao arquivo `composer.json` do diretório atual.

```sh
php composer.phar require
```

Após adicionar/alterar os requisitos, os mesmos que foram modificados serão instalados ou alterados.

Se você não quer escolher os requisitos de forma interativa, basta passá-los através do comando:

```sh
php composer.phar require vendor/package:2.* vendor/package2:dev-master
```

### Opções

* **--prefer-source:** Instala pacotes de `source` quando disponíveis.
* **--prefer-dist:** Instala pacotes de `dist` quando disponíveis.
* **--dev:** Instala pacotes listados em `require-dev` (comportamento padrão).
* **--no-update:** Desabilita o update automático das dependências
* **--no-progress:** Remove o diplay de progresso 
* **--update-with-dependencies** Adiciona também as dependências dos novos pacotes requeridos.



## global


Este comando permite rodar outros comandos (`install`, `require`
or `update`) como se você estivesse executando-os do diretório definido por [COMPOSER_HOME](#composer-home)

Pode ser usado para instalar utilitários CLI de forma global  se você adicionar o parâmetro 
`$COMPOSER_HOME/vendor/bin` a sua variável de ambiente`$PATH`. Exemplo:

```sh
php composer.phar global require fabpot/php-cs-fixer:dev-master
```

Agora o binário `php-cs-fixer` está disponível de forma global (assumindo que você ajustou a variável PATH). Se você deseja atualizar o binário mais tarde, basta  executar um update global:

```sh
php composer.phar global update
```

## search

O comando search permite que você faça uma busca através dos repositórios do pacote do projeto atual. Geralmente é apenas o packagist. 
Basta simplesmente passar os termos de busca:

```sh
php composer.phar search monolog
```

Você pode também buscar por mais de um termo, passando vários argumentos.

### Opções

* **--only-name (-N):** Busca apenas pelo nome.

## show

Para exibir a lista de todos os pacotes disponíveis, você pode usar o comando `show`.

```sh
php composer.phar show
```

Se você quer ver detalhes de um pacote específico, você pode informar o nome do mesmo:

```sh
php composer.phar show monolog/monolog

name     : monolog/monolog
versions : master-dev, 1.0.2, 1.0.1, 1.0.0, 1.0.0-RC1
type     : library
names    : monolog/monolog
source   : [git] http://github.com/Seldaek/monolog.git 3d4e60d0cbc4b888fe5ad223d77964428b1978da
dist     : [zip] http://github.com/Seldaek/monolog/zipball/3d4e60d0cbc4b888fe5ad223d77964428b1978da 3d4e60d0cbc4b888fe5ad223d77964428b1978da
license  : MIT

autoload
psr-0
Monolog : src/

requires
php >=5.3.0
```

Inclusive, você pode informar a versão do pacote e serão exibidos os detalhes dessa versão específica:

```sh
php composer.phar show monolog/monolog 1.0.2
```

### Options

* **--installed (-i):** Lista pacotes instalados
* **--platform (-p):**  Apenas os pacotes de plataforma (php & extensões).
* **--self (-s):**  Lista a informação de pacotes raiz.

## depends

O comando 


O comando `depends` indica quais outros pacotes dependem de um certo pacote. Você pode dizer quais tipos de links (`require`, `require-dev`) devem ser incluidos. Por padrão, ambos são utilizados.

```sh
php composer.phar depends --link-type=require monolog/monolog

nrk/monolog-fluent
poc/poc
propel/propel
symfony/monolog-bridge
symfony/symfony
```

### Opções

* **--link-type:** Tipos de links correspondentes. Podem ser especificados diversas vezes.

## validate

Você deve sempre rodar o comando `validate` antes de enviar o seu arquivo `composer.json` e antes de marcar um release. Este comando irá verificar se seu `composer.json` é válido.

```sh
php composer.phar validate
```

### Opções

* **--no-check-all:** Faz ou não uma validação completa.

## status

Se você não precisa modificar o código-fonte de suas dependências e elas são instaladas da fonte, o comando `status` permite a você checar se houve alterações em alguma delas (localmente).

```sh
php composer.phar status
```

Com a opção `--verbose` são exibidas mais informações do que foi alterado.

```sh
php composer.phar status -v

You have changes in the following dependencies:
vendor/seld/jsonlint:
    M README.mdown
```

## self-update

Para atualizar o composer para a versão mais atual, basta executar o comando `self-update`. Ele irá substituir o seu `composer.phar` para a versão mais atual.

```sh
php composer.phar self-update
```

Se, ao invés disso, você quiser atualizar para um release específico:

```sh
php composer.phar self-update 1.0.0-alpha7
```

Se você instalou o composer de forma global, você deve rodar o comando com privilégios de `root`:

```sh
sudo composer self-update
```

### Opções

* **--rollback (-r):** Rollback para a versão anterior
* **--clean-backups:** Exclui os backups antigos durante um update. Desta forma, a versão atual é o último backup disponível.

## config

O comando `config` permite você editar algumas configs básicas do composer tanto no `composer.json` (local) quanto no `config.json` (global).

```sh
php composer.phar config --list
```

### Como utilizar

`config [options] [setting-key] [setting-value1] ... [setting-valueN]`

Onde `setting-key`  é um nome de opção de config  `setting-value1` é o respectivo valor. Para opções que necesistam de um array de valores (
`github-protocols`), é permitido mais de um argumento do tipo "setting-value".

Dê uma olhada em [config schema section](04-schema.md#config) para maiores informações (e configs válidas).

### Opções

* **--global (-g):**  Trabalha com o arquivo de config global localizado em `$COMPOSER_HOME/config.json`  por padrão. Sem esta opção,  este comando  afeta o `composer.json` local ou outro, especificado por `--file`.
* **--editor (-e):** Abre o `composer.json` local usando um editor de texto definido pela variável de ambiente `EDITOR` . Com a opção `--global` o arquivo global será exibido.
* **--unset:** Remove o parâmetro de configuração definido por `setting-key`.
* **--list (-l):** Exibe a lista de variáveis atuais. Através da opção `--global`, será listado apenas o arquivo global.
* **--file="..." (-f):**  Trabalha num arquivo em específcado no composer.json` Esta opção não pode ser utilizada em conjunto com a opção `--global`.

### Modificando repositórios

Além de alterar a parte de configurações, através do  `config` você pode também fazer alterações na parte de repositórios, da seguinte forma:

```sh
php composer.phar config repositories.foo vcs http://github.com/foo/bar
```

## create-project

Você pode usar o Composer para criar novos projetos a partir de um pacote já existente. Isto equivale a fazer um *git clone/svn checkout* seguido de um `composer install`.

Esta opção pode ser útil de diversas formas:

1.  Você pode fazer o deploy de pacotes de aplicativos;
2.  Você pode baixar qualquer pacote e desenvolver patches para ele, por exemplo;
3.  Projetos com vários desenvolvedores podem usar esta feature para inicializar a aplicação no inicio do desenvolvimento.

Para criar um novo projeto utilizando o composer, você pode usar o comando "create-project". Especifique um nome de pacote e o diretório ao qual o projeto será criado.
Você pode também especificar um número de versão como terceiro argumento, do contrário, será utilizado o da última versão.
Se o diretório não existe, ele será criado durante a instalação.

```sh
php composer.phar create-project doctrine/orm path 2.2.*
```

Também é possível rodar o comando sem parâmetros num diretório que tenha o arquivo `composer.json` para inicializar o projeto.

Por padrão, o comando checa os pacotes no packagist.org.

### Opções

* **--repository-url:** Fornece um repositório personalizado para procurar pelo pacote, que será usado ao invés do packagist. Pode ser uma URL apontando para um diretório remoto do `composer`, ou um caminho para o arquivo `packages.json`
* **--stability (-s):** Versão mínima estável.
* **--prefer-source:**  Instala pacotes da fonte (source) quando disponível.
* **--prefer-dist:** Instala pacotes de `dist` quando disponíveis.
* **--dev:** Instala pacotes listados em `require-dev`.
* **--no-install:** Desabilita a instalação dos distribuidores (vendors).
* **--no-plugins:** Desabilita os plugins.
* **--no-scripts:** Desabilita a execução de scripts definidos no pacote raiz.
* **--no-progress:** Retira o indicador de progresso.
* **--keep-vcs:** Pula a exclusão de metadata do VCS para o projeto criado. Útil se você estiver executando o comando no modo não-interativo.

## dump-autoload

Se você precisa atualizar o autoloader por conta de novas classes no pacote de classmap, por exemplo, você pode usar o *dump-autoload* para fazer isto sem precisar passar por uma instalação ou atualização.

Este comando também permite criar um dump do autoloader, de forma automática, que converte os pacotes no padrão PSR-0/4 em *classmaps* por questões de performance. Em aplicações maiores com muitas classes, o autoloader pode tomar uma porção substancial de tempo para cada requisição. Usando *classmaps* para tudo é menos conveniente no desenvolvimento, mas utilizando esse recurso, você pode continuar utilizando o padrão PSR-0/4 por conveniência e *classmaps* para melhorar a perfomance. 

### Options

* **--optimize (-o):** Converte um autoloader no padrão PSR-0/4 para um *classmap* para obter um autoloader mais ráṕido. Recomendamos especialmente para deploy em produção, mas esta opção costuma consumir maior recurso de processamento e portanto, não é habilitada por padrão.
* **--no-dev:** Desabilita regras do  autoload-dev.

## licenses

Lista o nome, versão e licença de cada pacote instalado. Você pode definir o formato de saída (json) através do parâmetro `--format=json`.

## run-script

Para rodar os [scripts](articles/scripts.md) de forma manual, você pode utilizar este comando. Basta dar o nome do script e definir a opção --no-dev (opcional) para desabilitar o modo dev.

## diagnose

Se você encontrou um bug, ou alguma coisa não está funcionando corretamente, você pode rodar o comando `diagnose`para fazer checks automatizados, para tentar resolver alguns problemas comuns.

```sh
php composer.phar diagnose
```

## archive

Este comando é usado para gerar um zip/tar de um dado pacote em uma certa versão. Pode ser também utilizado para comprimir/armazenar todo o seu projeto, sem os arquivos excluídos/ignorados. 


```sh
php composer.phar archive vendor/package 2.0.21 --format=zip
```

### Options

* **--format (-f):** Formato do arquivo de saída: tar or zip (default:
  "tar")
* **--dir:** Gravar o arquivo neste diretório (default: ".")

## help

Para obter maior informação sobre um comando, basta usar o `help`.

```sh
php composer.phar help install
``

## Environment variables

Você pode definir certas variáveis de ambientes que irão sobrescrever algumas definições padrão. Assim que possível, recomendamos definir essas configs na seção `config` do arquivo `composer.json`. Vale a pena notar que estas variáveis tem precedência sobre as variáveis definidar no `composer.json`. 


### COMPOSER

Definindo a variável de ambiente `COMPOSER` é possível definir o nome do arquivo `composer.json para um outro qualquer.

Exemplo:

```sh
COMPOSER=composer-other.json php composer.phar install
```

### COMPOSER_ROOT_VERSION

Definindo essa variável você pode definir a versão do pacote raíz, se ela não puder ser definida atraves de informações do VCS e não estiver presente no arquivo `composer.json`.

### COMPOSER_VENDOR_DIR

Definindo essa variável você pode fazer com que o composer instale as dependências dentro de um diretório diferente do padrão (`vendor`).

### COMPOSER_BIN_DIR

Definindo essa opção, você pode alterar o diretório `bin` ([Vendor Binaries](articles/vendor-binaries.md))
para algo diferente de `vendor/bin`.

### http_proxy or HTTP_PROXY

Se você utiliza o composer por um proxy HTTP, você pode utilizar a variável `http_proxy` ou a varíavel de ambiemnte `HTTP_PROXY`. Simplesmente defina-a para a URL do seu proxy.
Diversos sistemas operacionais já definem esta variável para você.


Usando `http_proxy` (em minusculas) ou mesmo ambos, pode ser melhor pois algumas ferramentas (git, curl) irão apenas usar a versão *lower-case* do `http_proxy`. Você pode também definir o proxy para o git usando: 
`git config --global http.proxy <proxy url>`.

### no_proxy

Se você está em um proxy e deseja desabilitá-lo para alguns domínios, você pode usar a variável de ambiente `no_proxy`. Basta defini-la numa lista de valores separados por vírgula de domínios que o proxy não deve utilizar.

Esta variável de ambiente aceita domínios, endereços IP, e blocos de IP na notação CIDR. Você pode restringir a uma porta em particular (e.x. `:80`). Você pode também usar o `\* para ignorar o proxy para todas as requisições HTTP.

### HTTPS_PROXY_REQUEST_FULLURI

Se você utiliza um proxy mas ele não suporta a flag request_fulluri  para requisições HTTPS
requests, então você deve definir esta varíavel para `false` ou `0` para não deixar que o composer defina-a para `true`, por padrão.

### COMPOSER_HOME

A varíavel `COMPOSER_HOME` permite mudar o diretório padrão do Composer. Este diretório é oculto, global (por usuário na máquina) e compartilhado entre todos os projetos.

Por padrão, ele aponta para `/home/<user>/.composer` no \*nix,
`/Users/<user>/.composer` no OSX e 
`C:\Users\<user>\AppData\Roaming\Composer` no Windows.

#### COMPOSER_HOME/config.json

Você pode inserir um arquivo `config.json` no caminho apontado por `COMPOSER_HOME`. O Composer irá fazer um "merge" deste arquivo com o `composer.json` do projeto, quando você executar um comando `update` e/ou `install`.

Este arquivo permite definir algumas [configurações](04-schema.md#config) e
[repositórios](05-repositories.md)  para o projeto do usuário.

Caso o arquivo de configuração _global_ conflitar com o arquivo de configuração _local_, o Composer dará preferência ao segundo.

### COMPOSER_CACHE_DIR

A variável `COMPOSER_CACHE_DIR`  permite alterar o diretório de cache do composer, também configurável via [`cache-dir`](04-schema.md#config).

Por padrão,  ela é apontada para $COMPOSER_HOME/cache no \*nix / OSX, e
`C:\Users\<user>\AppData\Local\Composer` (or `%LOCALAPPDATA%/Composer`) no Windows.

### COMPOSER_PROCESS_TIMEOUT

Esta variável de ambiente controla o tempo de timeout do composer, ou seja, o tempo que o composer espera por cada comando para finalizar/prosseguir a execução. O valor padrão é de 300 segundos (05 minutos).


### COMPOSER_DISCARD_CHANGES

Esta variável controla a opção [discard-changes] (04-schema.md#config).

### COMPOSER_NO_INTERACTION

Caso denifido como 1, esta variável fará o composer se comportar como se você tivesse definido, para todo o comando, a flag 
`--no-interaction` . Ela pode ser definido em build boxes/CI.

&larr; [Libraries](02-libraries.md)  |  [Schema](04-schema.md) &rarr;
