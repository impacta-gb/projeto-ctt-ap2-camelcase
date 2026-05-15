# Concorrência I: Goroutines em Go

Em linguagens tradicionais, para executar tarefas simultaneamente, geralmente criamos Threads do sistema operacional, que são pesadas e consomem muita memória. Go resolve isso de forma brilhante com um conceito exclusivo: as **Goroutines**.

## O que são Goroutines?

Uma Goroutine é, essencialmente, uma **função que executa de forma independente e concorrente** em relação ao resto do programa.

Pense nelas como "threads de peso-pena". Elas não são threads reais do sistema operacional; elas são gerenciadas pelo próprio ecossistema de execução do Go (Go Runtime).

### Por que elas são tão inovadoras? 

**Consumo Mínimo de Memória:** Enquanto uma thread tradicional do sistema operacional costuma iniciar consumindo cerca de 1 MB a 2 MB de memória, uma Goroutine começa precisando de míseros **2 KB**.

**Criação em Massa:** Devido ao custo quase zero, você pode criar dezenas de milhares (ou até milhões) de Goroutines rodando ao mesmo tempo no seu computador sem travar o sistema.

**Gerenciamento Inteligente:** O Go possui um mecanismo interno que distribui essas milhares de Goroutines de forma automática entre os núcleos do seu processador. Se uma Goroutine ficar travada esperando uma resposta da internet, o Go coloca outra para rodar no lugar dela instantaneamente.

## Exemplo de Goroutines

```go
package main

import (
	"fmt"
	"time"
)

func exibirMensagem(texto string) {
	for i := 1; i <= 3; i++ {
		fmt.Printf("%s: %d\n", texto, i)
		time.Sleep(100 * time.Millisecond) // Simula uma tarefa que leva um tempinho
	}
}

func main() {
	// 1. Chamada comum (síncrona): o programa espera terminar para ir para a próxima linha
	exibirMensagem("Função Normal")

	// 2. Chamada com Goroutine (concorrente): o programa dispara a função e segue em frente na hora
	go exibirMensagem("Goroutine 1")

	// 3. Outra Goroutine rodando ao mesmo tempo
	go exibirMensagem("Goroutine 2")

	// Pausa necessária na main para dar tempo das Goroutines executarem antes do programa fechar
	time.Sleep(500 * time.Millisecond)
	fmt.Println("Fim do programa!")
}
```
### O que o código faz?

1. Execução Sequencial: O programa começa chamando **exibirMensagem("Função Normal")**. Como é uma chamada comum, o código trava ali e imprime os números de 1 a 3 antes de avançar.

2. A Palavra-Chave **go**: Quando o Go encontra **go exibirMensagem(...)**, ele não espera a função terminar. Ele cria uma nova Goroutine em segundo plano para rodar aquela função e passa imediatamente para a linha de baixo.

3. Execução Simultânea: As linhas "Goroutine 1" e "Goroutine 2" rodam ao mesmo tempo. No terminal, você verá os resultados delas se misturarem, provando que estão competindo pela tela simultaneamente.

4. A Espera na Main: A função **main** também é uma Goroutine (a principal). Se ela chegar ao fim, o programa morre e mata todas as outras de surpresa. O **time.Sleep** no final serve apenas para segurar a porta aberta para as outras terminarem o trabalho.

### Por que usar?

Ganho de Performance: Em vez de esperar uma tarefa terminar para começar outra (como baixar 10 imagens sequencialmente), você dispara 10 Goroutines e baixa todas ao mesmo tempo.

Simplicidade Absoluta: Você não precisa configurar bibliotecas complexas de gerenciamento de threads. Basta digitar a palavra **go** antes de qualquer função e o Go cuida de todo o resto.

## Dica 💡

**Use Funções Anônimas para tarefas rápidas**

Você não precisa necessariamente declarar uma função com nome usando **func** fora da **main** apenas para criar uma Goroutine. É muito comum e prático em Go disparar blocos de código na hora usando funções anônimas (clausuras):

```go
go func() {
    fmt.Println("Rodando de dentro de uma função anônima!")
}() // Esses parênteses no final servem para executar a função imediatamente
```

## Alerta ⚠️

**Cuidado com o fechamento prematuro da **main** e com o "Garbage" de memória!**

O uso de **time.Sleep** no final do código (como fizemos no exemplo) serve **apenas para fins didáticos**. Em sistemas reais, você nunca deve adivinhar o tempo que uma tarefa vai levar. Se a função **main** terminar sua execução, ela fechará o programa imediatamente, matando todas as outras Goroutines pela metade sem que elas terminem o trabalho.

## Resumo

As Goroutines tornam a concorrência incrivelmente simples: basta digitar **go** antes de uma função.

Contudo, disparar tarefas ao mesmo tempo é só metade do caminho. O verdadeiro poder surge quando essas tarefas conversam e se sincronizam de forma segura. Para resolver essa comunicação sem gerar conflitos, o Go utiliza os **Channels**, que veremos a seguir.