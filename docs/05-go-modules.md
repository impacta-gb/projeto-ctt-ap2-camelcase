# Go Modules

## O que são Go Modules

É o sistema oficial de gerenciamento de dependências do Go. Ele funciona como o "gerente" do seu projeto, sendo responsável por definir o nome do módulo, controlar as versões das bibliotecas externas que você utiliza e garantir que seu código seja fácil de compartilhar e rodar em qualquer computador.

## Exemplos de Go Modules

Acontece principalmente através de comandos no terminal que geram e organizam arquivos de configuração (go.mod e go.sum).

Bash
 1. Iniciando o módulo (Certidão de nascimento do projeto)
 Substitua "meu-projeto" pelo nome que desejar
go mod init meu-projeto

 2. Adicionando uma dependência externa
 Exemplo: baixando um pacote para dar cores ao terminal
go get github.com/fatih/color

 3. Limpando e organizando as dependências
 Remove o que não é usado e baixa o que falta
go mod tidy
Exemplo de como o código utiliza um módulo externo:

```go
package main

import (
    "github.com/fatih/color" // Importando a dependência instalada
)

func main() {
    // Usando uma função do módulo externo
    color.Cyan("🚀 Projeto configurado com Go Modules!")
}
```

### O que o código faz?

O comando go mod init cria um arquivo chamado go.mod, que registra o nome do seu projeto e a versão do Go. Quando você baixa uma biblioteca com go get, o Go Modules anota a versão exata dela para que, no futuro, seu código não pare de funcionar caso a biblioteca seja atualizada de forma incompatível.

### Por que usar?

Portabilidade: Permite que qualquer pessoa baixe seu projeto e instale todas as dependências necessárias com um único comando, sem que você precise enviar pastas pesadas de bibliotecas para o GitHub.

Gestão de Versões: Você tem controle total sobre quais versões das bibliotecas seu projeto usa, evitando bugs causados por atualizações automáticas de terceiros.

Liberdade de Pastas: Antes do Go Modules, você era obrigado a criar projetos em pastas específicas do sistema. Com ele, você pode criar seu projeto em qualquer lugar do seu computador.

!!! tip "Dica 💡"

    Sempre que você terminar de escrever seu código ou antes de fazer um commit, rode o comando go mod tidy. Ele faz uma "limpeza geral": remove bibliotecas que você parou de usar e baixa as que você acabou de importar, deixando seu arquivo go.mod sempre impecável.

!!! warning "Alerta ⚠️"
    Arquivo go.sum: Nunca apague o arquivo go.sum. Ele guarda as assinaturas de segurança das bibliotecas. Se ele for alterado ou excluído, o Go pode impedir o download das dependências por segurança.

    Nome do Módulo: Se você pretende subir seu código para o GitHub, o ideal é iniciar o módulo com o caminho do repositório, por exemplo: go mod init github.com/seu-usuario/nome-do-repo. Isso facilita a importação por outras pessoas.

## Resumo
O Go Modules é o "RG" e a "Lista de Compras" do seu projeto. Ele diz quem o seu projeto é e de quais ferramentas externas ele precisa para funcionar. É o que transforma uma simples pasta de arquivos em um projeto profissional, seguro e pronto para ser compartilhado com o mundo.