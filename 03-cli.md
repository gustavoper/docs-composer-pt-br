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

O comando `install` lê o arquivo `composer.json`  do diretório corrente, trata as dependências e instala-as no diretório `vendor`.

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

Se você prefere não escolher os requisitos de forma interativa, basta passá-los através do comando:

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

If you want to see the details of a certain package, you can pass the package
name.

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

You can even pass the package version, which will tell you the details of that
specific version.

```sh
php composer.phar show monolog/monolog 1.0.2
```

### Options

* **--installed (-i):** List the pacotes that are installed.
* **--platform (-p):** List only platform pacotes (php & extensions).
* **--self (-s):** List the root package info.

## depends

The `depends` command tells you which other pacotes depend on a certain
package. You can specify which link types (`require`, `require-dev`)
should be included in the listing. By default both are used.

```sh
php composer.phar depends --link-type=require monolog/monolog

nrk/monolog-fluent
poc/poc
propel/propel
symfony/monolog-bridge
symfony/symfony
```

### Options

* **--link-type:** The link types to match on, can be specified multiple
  times.

## validate

You should always run the `validate` command before you commit your
`composer.json` file, and before you tag a release. It will check if your
`composer.json` is valid.

```sh
php composer.phar validate
```

### Options

* **--no-check-all:** Wether or not composer do a complete validation.

## status

If you often need to modify the code of your dependencies and they are
installed from source, the `status` command allows you to check if you have
local changes in any of them.

```sh
php composer.phar status
```

With the `--verbose` option you get some more information about what was
changed:

```sh
php composer.phar status -v

You have changes in the following dependencies:
vendor/seld/jsonlint:
    M README.mdown
```

## self-update

To update composer itself to the latest version, just run the `self-update`
command. It will replace your `composer.phar` with the latest version.

```sh
php composer.phar self-update
```

If you would like to instead update to a specific release simply specify it:

```sh
php composer.phar self-update 1.0.0-alpha7
```

If you have installed composer for your entire system (see [global installation](00-intro.md#globally)),
you may have to run the command with `root` privileges

```sh
sudo composer self-update
```

### Options

* **--rollback (-r):** Rollback to the last version you had installed.
* **--clean-backups:** Delete old backups during an update. This makes the current version of composer the only backup available after the update.

## config

The `config` command allows you to edit some basic composer settings in either
the local composer.json file or the global config.json file.

```sh
php composer.phar config --list
```

### Usage

`config [options] [setting-key] [setting-value1] ... [setting-valueN]`

`setting-key` is a configuration option name and `setting-value1` is a
configuration value.  For settings that can take an array of values (like
`github-protocols`), more than one setting-value arguments are allowed.

See the [config schema section](04-schema.md#config) for valid configuration
options.

### Options

* **--global (-g):** Operate on the global config file located at
`$COMPOSER_HOME/config.json` by default.  Without this option, this command
affects the local composer.json file or a file specified by `--file`.
* **--editor (-e):** Open the local composer.json file using in a text editor as
defined by the `EDITOR` env variable.  With the `--global` option, this opens
the global config file.
* **--unset:** Remove the configuration element named by `setting-key`.
* **--list (-l):** Show the list of current config variables.  With the `--global`
 option this lists the global configuration only.
* **--file="..." (-f):** Operate on a specific file instead of composer.json. Note
 that this cannot be used in conjunction with the `--global` option.

### Modifying Repositories

In addition to modifying the config section, the `config` command also supports making
changes to the repositories section by using it the following way:

```sh
php composer.phar config repositories.foo vcs http://github.com/foo/bar
```

## create-project

You can use Composer to create new projects from an existing package. This is
the equivalent of doing a git clone/svn checkout followed by a composer install
of the vendors.

There are several applications for this:

1. You can deploy application pacotes.
2. You can check out any package and start developing on patches for example.
3. Projects with multiple developers can use this feature to bootstrap the
   initial application for development.

To create a new project using composer you can use the "create-project" command.
Pass it a package name, and the directory to create the project in. You can also
provide a version as third argument, otherwise the latest version is used.

If the directory does not currently exist, it will be created during installation.

```sh
php composer.phar create-project doctrine/orm path 2.2.*
```

It is also possible to run the command without params in a directory with an
existing `composer.json` file to bootstrap a project.

By default the command checks for the pacotes on packagist.org.

### Options

* **--repository-url:** Provide a custom repository to search for the package,
  which will be used instead of packagist. Can be either an HTTP URL pointing
  to a `composer` repository, or a path to a local `pacotes.json` file.
* **--stability (-s):** Minimum stability of package. Defaults to `stable`.
* **--prefer-source:** Install pacotes from `source` when available.
* **--prefer-dist:** Install pacotes from `dist` when available.
* **--dev:** Install pacotes listed in `require-dev`.
* **--no-install:** Disables installation of the vendors.
* **--no-plugins:** Disables plugins.
* **--no-scripts:** Disables the execution of the scripts defined in the root
  package.
* **--no-progress:** Removes the progress display that can mess with some
  terminals or scripts which don't handle backspace characters.
* **--keep-vcs:** Skip the deletion of the VCS metadata for the created
  project. This is mostly useful if you run the command in non-interactive
  mode.

## dump-autoload

If you need to update the autoloader because of new classes in a classmap
package for example, you can use "dump-autoload" to do that without having to
go through an install or update.

Additionally, it can dump an optimized autoloader that converts PSR-0/4 pacotes
into classmap ones for performance reasons. In large applications with many
classes, the autoloader can take up a substantial portion of every request's
time. Using classmaps for everything is less convenient in development, but
using this option you can still use PSR-0/4 for convenience and classmaps for
performance.

### Options

* **--optimize (-o):** Convert PSR-0/4 autoloading to classmap to get a faster
  autoloader. This is recommended especially for production, but can take
  a bit of time to run so it is currently not done by default.
* **--no-dev:** Disables autoload-dev rules.

## licenses

Lists the name, version and license of every package installed. Use
`--format=json` to get machine readable output.

## run-script

To run [scripts](articles/scripts.md) manually you can use this command,
just give it the script name and optionally --no-dev to disable the dev mode.

## diagnose

If you think you found a bug, or something is behaving strangely, you might
want to run the `diagnose` command to perform automated checks for many common
problems.

```sh
php composer.phar diagnose
```

## archive

This command is used to generate a zip/tar archive for a given package in a
given version. It can also be used to archive your entire project without
excluded/ignored files.

```sh
php composer.phar archive vendor/package 2.0.21 --format=zip
```

### Options

* **--format (-f):** Format of the resulting archive: tar or zip (default:
  "tar")
* **--dir:** Write the archive to this directory (default: ".")

## help

To get more information about a certain command, just use `help`.

```sh
php composer.phar help install
```

## Environment variables

You can set a number of environment variables that override certain settings.
Whenever possible it is recommended to specify these settings in the `config`
section of `composer.json` instead. It is worth noting that the env vars will
always take precedence over the values specified in `composer.json`.

### COMPOSER

By setting the `COMPOSER` env variable it is possible to set the filename of
`composer.json` to something else.

For example:

```sh
COMPOSER=composer-other.json php composer.phar install
```

### COMPOSER_ROOT_VERSION

By setting this var you can specify the version of the root package, if it can
not be guessed from VCS info and is not present in `composer.json`.

### COMPOSER_VENDOR_DIR

By setting this var you can make composer install the dependencies into a
directory other than `vendor`.

### COMPOSER_BIN_DIR

By setting this option you can change the `bin` ([Vendor Binaries](articles/vendor-binaries.md))
directory to something other than `vendor/bin`.

### http_proxy or HTTP_PROXY

If you are using composer from behind an HTTP proxy, you can use the standard
`http_proxy` or `HTTP_PROXY` env vars. Simply set it to the URL of your proxy.
Many operating systems already set this variable for you.

Using `http_proxy` (lowercased) or even defining both might be preferable since
some tools like git or curl will only use the lower-cased `http_proxy` version.
Alternatively you can also define the git proxy using
`git config --global http.proxy <proxy url>`.

### no_proxy

If you are behind a proxy and would like to disable it for certain domains, you
can use the `no_proxy` env var. Simply set it to a comma separated list of
domains the proxy should *not* be used for.

The env var accepts domains, IP addresses, and IP address blocks in CIDR
notation. You can restrict the filter to a particular port (e.g. `:80`). You
can also set it to `*` to ignore the proxy for all HTTP requests.

### HTTP_PROXY_REQUEST_FULLURI

If you use a proxy but it does not support the request_fulluri flag, then you
should set this env var to `false` or `0` to prevent composer from setting the
request_fulluri option.

### HTTPS_PROXY_REQUEST_FULLURI

If you use a proxy but it does not support the request_fulluri flag for HTTPS
requests, then you should set this env var to `false` or `0` to prevent composer
from setting the request_fulluri option.

### COMPOSER_HOME

The `COMPOSER_HOME` var allows you to change the composer home directory. This
is a hidden, global (per-user on the machine) directory that is shared between
all projects.

By default it points to `/home/<user>/.composer` on \*nix,
`/Users/<user>/.composer` on OSX and
`C:\Users\<user>\AppData\Roaming\Composer` on Windows.

#### COMPOSER_HOME/config.json

You may put a `config.json` file into the location which `COMPOSER_HOME` points
to. Composer will merge this configuration with your project's `composer.json`
when you run the `install` and `update` commands.

This file allows you to set [configuration](04-schema.md#config) and
[repositories](05-repositories.md) for the user's projects.

In case global configuration matches _local_ configuration, the _local_
configuration in the project's `composer.json` always wins.

### COMPOSER_CACHE_DIR

The `COMPOSER_CACHE_DIR` var allows you to change the composer cache directory,
which is also configurable via the [`cache-dir`](04-schema.md#config) option.

By default it points to $COMPOSER_HOME/cache on \*nix and OSX, and
`C:\Users\<user>\AppData\Local\Composer` (or `%LOCALAPPDATA%/Composer`) on Windows.

### COMPOSER_PROCESS_TIMEOUT

This env var controls the time composer waits for commands (such as git
commands) to finish executing. The default value is 300 seconds (5 minutes).

### COMPOSER_DISCARD_CHANGES

This env var controls the discard-changes [config option](04-schema.md#config).

### COMPOSER_NO_INTERACTION

If set to 1, this env var will make composer behave as if you passed the
`--no-interaction` flag to every command. This can be set on build boxes/CI.

&larr; [Libraries](02-libraries.md)  |  [Schema](04-schema.md) &rarr;
