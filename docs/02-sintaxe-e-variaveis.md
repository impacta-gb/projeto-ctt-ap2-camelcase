# Sintaxe Básica e Variáveis em Go

## O que é uma Sintaxe Básica

É o conjunto de regras que define como o código deve ser escrito para que o computador consiga entendê-lo. É como a gramática de um idioma: se você não seguir as regras, a comunicação (ou a compilação do programa) falha.

## Exemplos de Sintaxe em Código

```go
package main

import "fmt"

func main() {
    // 1. Declaração Padrão (Explícita)
    // Usada para clareza total ou quando não há valor inicial imediato.
    var nome string = "Arthur"
    var idade int = 19

    // 2. Declaração Curta (Inferência de Tipo)
    // A forma mais ágil, onde o Go "descobre" o tipo pelo valor.
    cidade := "Osasco"

    // 3. Constantes
    // Para valores que jamais mudam, como nomes de instituições.
    const Professor = "Impacta"

    fmt.Println(nome, idade, cidade, Professor)
}
```

### O que o código faz? 

O código reserva espaços específicos na memória do computador (variáveis) para armazenar textos e números. Ele permite que o programa recupere e manipule esses dados durante a execução.

### Por que usar?

- Segurança de Tipos: Por ser rigoroso, o Go evita que você cometa erros de lógica, como tentar subtrair um texto de um número.

- Performance: Como a sintaxe é simples e compilada, o computador processa as instruções muito mais rápido do que em linguagens interpretadas.

- Legibilidade: A ausência de caracteres desnecessários torna o código mais fácil de ser revisado pelos seus colegas de grupo.

!!! tip "Dica 💡"
    Utilize sempre nomes de variáveis que façam sentido (ex: precoProduto em vez de apenas p). Em Go, a clareza é prioridade sobre a brevidade.    

!!! warning "Alerta ⚠️"
    Variáveis Não Usadas: O compilador do Go é "bravo": se você declarar uma variável e não usá-la no código, o programa não vai compilar. Isso garante que seu software seja sempre limpo e econômico.

    Operador Curto: Lembre-se que o := só funciona dentro de funções (como a main). Para declarar variáveis globais (fora das funções), você deve usar obrigatoriamente a palavra var.


## Resumo

A sintaxe do Go é como uma receita de bolo direta ao ponto: você define os ingredientes (variáveis), dá um nome a eles e começa a misturar. Sem regras confusas ou pontuações sobrando, o foco é total na eficiência do seu código.


## O que são Variáveis em Go

Variáveis são recipientes nomeados para armazenar dados que podem ser alterados durante a execução do programa. Por ser uma linguagem estaticamente tipada, cada variável tem um tipo definido que não muda.

## Exemplos de Código
Três formas principais de lidar com variáveis:

```go
package main

import "fmt"

func main() {
    // 1. Declaração com valor inicial (Explícita)
    var nome string = "Arthur"

    // 2. Declaração sem valor inicial (Assume o "valor zero")
    var idade int 
    idade = 19

    // 3. Declaração Curta (Inferência de tipo)
    // O Go define o tipo automaticamente com base no valor à direita.
    cidade := "Osasco"

    fmt.Println(nome, idade, cidade)
}
```

### O que o código faz?

Ele aloca um espaço na memória RAM do computador para guardar informações específicas (textos ou números) sob um nome que você escolhe.

### Por que usar?

- Manipulação de Dados: Variáveis permitem que você receba uma entrada do usuário, processe esse dado e exiba um resultado.

- Legibilidade: Em vez de usar valores soltos no código, você usa nomes (como precoTotal), o que torna o sistema compreensível para o seu grupo de trabalho.

- Consistência: Alterar o valor em um único lugar (na variável) atualiza automaticamente todas as partes do código que a utilizam.

!!! tip "Dica 💡"
    Se você declarar uma variável e não der um valor a ela, ela não fica "nula" ou "lixo". Inteiros viram 0, strings viram "" (vazio) e booleanos viram false.

!!! warning "Alerta ⚠️"
    Você não pode mudar o tipo de uma variável depois de criada. Se idade foi definida como int, você nunca poderá guardar o texto "dezenove" nela.

    Se você criar uma variável dentro de uma função e não usá-la em lugar nenhum, o compilador do Go impedirá a execução do programa até que você a remova ou a utilize.

## Resumo

Variáveis são as "caixas" onde você guarda as informações do seu programa. Você coloca uma etiqueta (nome), define o que cabe dentro (tipo) e pode trocar o conteúdo sempre que precisar, desde que respeite o tamanho e o formato da caixa.

