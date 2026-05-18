# Estrutura de controle em Go

## O que são Estruturas de Controle 

Foram desenhadas para serem simples e diretas. O objetivo da linguagem é evitar códigos "sujos" e confusos, eliminando parênteses desnecessários e focando na lógica de execução.

## Exemplos de Estrutura de controle

```go
package main

import "fmt"

func main() {
    // 1. Condicional (If/Else)
    idade := 19
    if idade >= 18 {
        fmt.Println("✅ Maior de idade")
    } else {
        fmt.Println("❌ Menor de idade")
    }

    // 2. Laço de Repetição (For)
    for i := 0; i < 3; i++ {
        fmt.Println("Volta número:", i)
    }

    // 3. Escolha Múltipla (Switch)
    dia := "segunda"
    switch dia {
    case "segunda":
        fmt.Println("📚 Dia de estudar Go")
    default:
        fmt.Println("💻 Outro dia de código")
    }
}
```

### O que o código faz?

Esse código controla o fluxo do programa. Ele decide se uma mensagem deve ser exibida (condicionais) ou se uma ação deve ser repetida várias vezes (laços).

### Por que usar?

Tomada de Decisão: Sem o if, o programa seria apenas uma sequência linear; com ele, o software pode reagir a diferentes situações.

Automação: O for permite processar grandes listas de dados ou repetir tarefas cansativas sem precisar escrever o mesmo código várias vezes.

Simplicidade: Go não possui while ou do-while, o que reduz a curva de aprendizado e torna o código de todos os desenvolvedores muito parecido.

!!! tip "Dica 💡"
    Você pode usar uma "inicialização curta" dentro do if. Exemplo: if v := calcular(); v > 10 { ... }. Isso mantém a variável v limitada apenas àquele bloco, limpando a memória logo depois.

!!! warning "Alerta ⚠️"
    Chaves: Em Go, a abertura de chaves { deve estar na mesma linha que o if ou for. Se você colocar a chave na linha de baixo, o compilador dará erro.
    
    Loop Infinito: Se você escrever apenas for { ... } sem nenhuma condição, o programa rodará para sempre. Certifique-se de ter um break ou uma condição de saída.

## Resumo

As estruturas de controle são os "semáforos" e "rotatórias" do seu código. O if decide o caminho, o for faz a volta e o switch escolhe a melhor saída. Tudo isso sem parênteses e sem complicação.



