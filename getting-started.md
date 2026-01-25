## Requisitos

O passo inicial é a configuração do nosso ambiente de desenvolvimento. Atualmente o RakStar possui suporte apenas para sistemas Linux, sendo necessário a instalação do [WSL2](https://learn.microsoft.com/pt-br/windows/wsl/install) se seu sistema for windows. Para utilizar o WSL2, você deve estar no Windows 10 ou superior.


### <h3 id="wsl"> Instalando e configurando o WSL (windows)</h3>

Abra o seu PowerShell como administrador e digite o comando abaixo:

```sh
wsl --install
```

Em seguida, abra a Microsoft Store e pesquise por "Ubuntu 22.04".
Instale a versão correspondente e reinicie sua máquina.

**(INSERIR INSTRUÇÕES)**

### Instalando dependências

Abra um terminal Ubuntu e instale o [SAMPCTL](https://github.com/Southclaws/sampctl/tree/master).

```sh
curl -fsSL https://raw.githubusercontent.com/Southclaws/sampctl/master/scripts/install-deb.sh | bash
```

Em seguida, verifique a instalação: 
```sh
sampctl 
```

**(INSERIR INSTRUÇÕES)**

## GameMode inicial

Crie um novo GameMode com SAMPCTL e escolha as configurações de sua preferência. Opte por SA-MP ao escolher o runtime (o suporte ao OpenMP ainda não é garantido).

```sh
mkdir tutorial-rakstar && cd tutorial-rakstar && sampctl init
```

Inicie um projeto Go na pasta do servidor e instale o Rakstar.

```sh
go mod init tutorial
go get github.com/alph4b3th/rakstar
```

Crie um arquivo chamado `main.go` que deverá conter o seguinte código.

```go
package main

func init() {}

func main() {} // -> não utilize essa função!
```

Temos duas funções: `main` e `init`. Não escreveremos na função `main` porque o RakStar é uma [biblioteca dinâmica](https://en.wikipedia.org/wiki/Dynamic_library) e a função não será executada. No entanto, a função deve ser mantida vazia para que o compilador compile o seu código. Escreveremos o código principal na função `init`, que será executada automaticamente quando o seu servidor SA-MP inicializar.


Vamos fazer um código de boas-vindas.

```go
import (
    "github.com/alph4b3th/rakstar/events"
    "github.com/alph4b3th/rakstar/init"
)

func init() {
    events.NewBuilder().
        SetName("onPlayerConnect").
        SetHandler(func (playerID int) {
            func(playerID int) {
                chat.
                    Builder().
                    Message(fmt.Sprintf("Bem vindo %s.", player.Builder().Select(playerID).Name)).
                    Tag("TUTORIAL").
                    Select(playerID).
                    Color("ffaaff").
                    Send()
            }
        }).
        Subscribe()
}

func main() {}
```

Perceba que, ao utilizarmos o módulo `events`, invocamos um construtor através da função `NewBuilder`. O Rakstar adota o [padrão builder](https://refactoring.guru/design-patterns/builder) para a maiora dos seus recursos; a escolha desse padrão favorece a legibilidade do framework. Embora algumas vezes possa ser verboso, a qualidade da leitura do código sobressai-se. 

O código de boas-vindas é tão simples quanto parece. Utilizamos o `SetName` para definir o nome do evento no qual queremos escutar, em `SetHandler` pomos a função que será executada quando o evento ocorrer e por fim executamos a função `Subscribe`, que construirá o evento e o registrará. 

Dentro do "handler" utilizamos o módulo `chat`. Esse é um módulo completo com todas as funções básicas de chat que utilizamos repetidamente. Invocamos o construtor `Builder`. Definimos a nossa mensagem com `Message` utilizando em complemento a função `Sprintf`, que formatará a string e a retornará. No nosso caso, utilizamos também o módulo `player` para obter o nome do jogador e pôr na mensagem. Colocamos um marcador na mensagem `Tag`. Selecionamos o jogador que ouvirá a mensagem `Select`. Por último, definimos a cor da mensagem com um hexadecimal `Color`. A função `Send` irá construir a mensagem e finalmente enviá-la ao jogador.  

Agora podemos compilar o código.

```sh
CGO_ENABLED=1 GOOS=linux GOARCH=386 go build -buildmode=c-shared -o  plugins/main.so main.go
```

Antes de testarmos o GameMode, é necessário configurar o SA-MP para carregar o plugin plugin que compilamos. Em RakStar, sua gamemode é um plugin SA-MP. 

Para isso, altere o seu arquivo `pawn.json` adicionando o array de plugins. O seu arquivo deverá ser similar com o arquivo abaixo, você deverá apenas adicionar ou modificar o campo `plugins`.

```json
{
    "plugins": ["main.so"],
	"user": "tutorial",
	"repo": "test",
	"tag": "0.0.1",
	"entry": "test.pwn",
	"output": "gamemodes/test.amx",
	"dependencies": [
		"pawn-lang/samp-stdlib"
	],
	"local": true,
	"runtime": {
		"version": "0.3.7"
	}
}
```

Ligue o servidor com `sampctl run` e você deverá ver a mensagem que o plugin `main.so` foi carregado.

```txt
[00:00:00] Server Plugins
[00:00:00] --------------
[00:00:00]  Loading plugin: main.so
[00:00:00]   Loaded.
[00:00:00]  Loaded 1 plugins.
```

Por fim, teste o GameMode no GTA e certifique-se de que a mensagem de boas-vindas esteja funcionando. Caso ocorra algum erro ou comportamento atípico, por favor, releia as instruções e refaça os passos. Se o problema persistir, solicitamos que abra uma issue no [repositório](https://github.com/alph4b3th/rakstar) do Rakstar.

