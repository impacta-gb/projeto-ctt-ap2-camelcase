# Tratamento de Erros em Go - Error Handling

## O que é Error Handling

O Error Handling (Tratamento de Erros) em Go é feito de forma explícita, tratando erros como valores comuns. Diferente de outras linguagens, Go não utiliza o mecanismo de **try/catch** ou  **exceções**, forçando o desenvolvedor a verificar e gerenciar as falhas logo após a execução de uma função. Na prática, se uma função pode falhar, ela é projetada para retornar o resultado esperado e um erro (geralmente como o último valor do retorno).

## Exemplo de Error Handling

## 1. O Padrão do Dia a Dia (Consumindo Erros)

No cotidiano em Go, a maior parte do tempo é gasta tratando erros gerados por funções da própria biblioteca padrão do Go ou de pacotes externos.

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    // Tentamos abrir um arquivo chamado "dados.txt"
    arquivo, err := os.Open("dados.txt")
    
    // Verificação explícita: se 'err' não for nulo (nil), algo deu errado
    if err != nil {
        fmt.Println("Erro ao abrir o arquivo:", err)
        return // Interrompe a execução para evitar o uso de um arquivo inexistente
    }

    // Se o erro for 'nil', a operação foi um sucesso
    fmt.Println("Arquivo aberto com sucesso!", arquivo.Name())
    arquivo.Close() 
}
```
### O que o código faz?

A função **os.Open** tenta abrir um arquivo e retorna duas variáveis: o ponteiro do **arquivo** e a variável **err**. O bloco **is err != nil** checa se o erro carrega alguma informação. Se o arquivo não existir, o programa entra no **if**, exibe o error e o **return** encerra a função.

### Por que usar?

Evita travamentos fatais (panics) no sistema. Se você tentasse ler a variável **arquivo** estando ela vazia devido a uma falha silenciosa, o programa simplesmente cairia em execução.

## 2. Criando seus próprios Erros (errors.New)

Quando você escreve suas próprias funções e precisa sinalizar que algo deu errado de forma simples, utiliza-se o pacote **errors**.

```go
package main

import (
    "errors"
    "fmt"
)

func dividir(a, b float64) (float64, error) {
    if b == 0 {
        // Criamos e retornamos um erro de texto simples
        return 0, errors.New("divisão por zero")
    }
    return a / b, nil // Retorna nil quando tudo corre bem
}

func main() {
    resultado, err := dividir(4, 0)
    if err != nil {
        fmt.Println("Erro:", err)
        return
    }
    fmt.Println("Resultado:", resultado)
}
```
### O que o código faz?

A função **dividir** analisa os parâmetros recebidos. Se o divisor (**b**) for zero, ela interrompe o cálculo matemático e envia um erro criado na hora por **errors.New("divisão por zero")**. O último valor retornado indica o sucesso (**nil**) ou a falha.

### Por que usar?

É a maneira mais rápida e limpa de gerar mensagens de erro personalizadas baseadas em texto, sem a necessidade de criar estruturas de dados complexas.

## 3. Por baixo dos panos (A interface error)

O tipo **error** em Go não é um tipo primitivo mágico do compilador, mas sim uma interface nativa extremamente simples que dita as regras do jogo.

```go
type error interface {
    Error() string
}
```
### O que o código faz?

Esta interface define apenas um contrato: qualquer tipo de dado que possua um método chamado **Error()** que retorne uma **string** é considerado um erro legítimo pelo Go.

### Por que usar?

É a base do polimorfismo de erros no Go. Graças a essa simplicidade, você pode envelopar e criar qualquer tipo de estrutura para funcionar como um erro do sistema.

## 4. Customizando Erros (Criando sua própria Struct de Erro)

Utilizando a regra da interface **error**, você pode criar tipos complexos para transportar mais infornações sobre a falha (como códigos de status HTTP ou timestamps) além de um texto simples.

```go
package main

import (
    "fmt"
)

// Criamos uma estrutura para carregar o erro
type MeuErro struct {
    Mensagem string
}

// Implementamos o método da interface nativa do Go
func (e *MeuErro) Error() string {
    return e.Mensagem
}

func dividir(a, b float64) (float64, error) {
    if b == 0 {
        // Passamos o ponteiro da nossa struct customizada como erro
        return 0, &MeuErro{Mensagem: "divisão por zero"}
    }
    return a / b, nil
}

func main() {
    resultado, err := dividir(4, 0)
    if err != nil {
        fmt.Println("Erro:", err)
        return
    }
    fmt.Println("Resultado:", resultado)
}
```
### O que o código faz?

Criamos a struct **MeuErro** e anexamos a ela o método **Error() string**. Na função **dividir**, passamos o endereço de memória dela (**&MeuErro**) caso o divisor seja zero. O Go aceita isso perfeitamente porque a struct cumpre o contrato da interface nativa.

### Por que usar?

Essencial para cenários onde a aplicação precisa tomar decisões baseadas no tipo de erro que ocorreu (por exemplo, diferenciar um erro de validação de dados de um erro interno do servidor).

## 5. Casos Extremos (Tratamento de Panics)

O **panic** representa uma falha catastrófica da qual o programa não sabe como se recuperar espontaneamente. O **recover** age como uma rede de proteção para salvar o software de uma queda definitiva.

```go
package main

import (
    "fmt"
)

func podePanicar() {
    // O defer garante que esta função será executada ao sair do escopo atual
    defer func() {
        // O recover captura o pânico em andamento e impede o travamento total
        if r := recover(); r != nil {
            fmt.Println("Recuperado do panic:", r)
        }
    }()
    
    panic("um problema ocorreu") // Força uma interrupção fatal
}

func main() {
    fmt.Println("Início")
    podePanicar()
    fmt.Println("Fim")
}
```
### O que o código faz?

A função **podePanicar** dispara intencionalmente um estado de choque no programa via **panic()**. No entanto, como declaramos um bloco **defer** contendo a função **recover()**, o Go intercepta o colapso e retoma o controle. O fluxo volta para a função **main()**, permitindo que o texto "Fim" seja impresso.

### Por que usar?

Deve ser usado estritamente para segurança interna em pontos críticos de servidores (como roteadores HTTP), garantindo que um erro bizarro ou inesperado em uma requisição de um único usuário não derrube o sistema inteiro para todos os outros.

!!! tip "Dica 💡"

    **Trate o erro imediatamente e limpe o caminho**

    Faça a checagem **if err != nil** logo na linha seguinte à chamada da função. Trate a falha (exiba a mensagem ou faça o **return**) para que o restante do seu código possa rodar livre de preocupações, mantendo o fluxo principal limpo e sem recuos (identações) desnecessários.

!!! warning "Alerta ⚠️"

    **Não use Panic como se fosse Exception!**

    O mecanismo de **panic/recover** lembra o **try/catch** de outras linguagens, mas ele não deve ser usado para erros comuns do dia a dia (como uma senha errada ou um arquivo não encontrado). Use o **panic** apenas para situações catastróficas onde o programa realmente não tem condições de continuar rodando, como a falta de uma configuração crítica na inicialização do sistema.

## Resumo

Para quem vem do **try/catch**, o modelo do Go pode parecer repetitivo, mas ele é um dos maiores superpoderes da linguagem.

Ao te forçar a lidar com a falha no momento exato em que ela ocorre, Go elimina surpresas desagradáveis em produção. O resultado é um código **previsível, seguro e altamente robusto**. Em Go, tratar erros não é um trabalho extra; é construir um software que resiste a falhas por padrão.