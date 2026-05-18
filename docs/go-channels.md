# Concorrência II: Channels em Go

Agora que você já sabe como disparar milhares de tarefas simultâneas usando as Goroutines, o próximo desafio é fazer com que elas conversem entre si de forma organizada e segura. É aqui que entram os **Channels** (Canais).

## O que são Channels?

Channels são os **canais de comunicação** do Go. Eles funcionam como tubulações blindadas que permitem que uma Goroutine envie dados para outra com total segurança, sem o risco de que uma atropele a memória da outra.

### Por que elas são tão importantes?

**Sincronização Automática:** Um canal não serve apenas para passar dados; ele também funciona como um semáforo de trânsito. Se uma Goroutine tenta ler um canal que está vazio, o Go congela essa Goroutine automaticamente até que outra Goroutine coloque um dado lá dentro. Você ganha controle de fluxo sem precisar escrever códigos complexos de espera.

**Segurança de Memória (Thread-Safety):** Em outras linguagens, se duas tarefas tentam mexer na mesma variável ao mesmo tempo, o programa quebra ou gera dados corrompidos. Com Channels, o dado é transferido de uma Goroutine para outra como um bastão em uma corrida de revezamento — apenas uma tem o controle por vez.

**Substitutos do time.Sleep:** Lembra que usamos um cronômetro para "adivinhar" quando as Goroutines iam terminar no capítulo anterior? Com Channels, a Goroutine principal avisa pelo canal que terminou, e o programa fecha no milissegundo exato em que o trabalho acaba.

## Exemplo de Channels

## 1. Canais com Buffer (A Fila de Espera)

Por padrão, um canal comum bloqueia a execução na hora. O Go permite criar canais com um "estoque temporário", chamado de **Buffer**. Isso permite enviar dados mesmo sem uma Goroutine ativa do outro lado esperando.

```go
package main

import "fmt"

func main() {
	// Criamos um canal com BUFFER de capacidade 2. 
	// Ele funciona como uma fila que aguenta até 2 itens sem travar o programa.
	canal := make(chan int, 2)

	// Enviando dados diretamente (só é possível por causa do buffer)
	canal <- 10
	canal <- 20

	// Lendo os dados do buffer
	fmt.Println(<-canal)
	fmt.Println(<-canal)
}
```
### O que o código faz?

1. Criação da Fila: O número **2** dentro de **make(chan int, 2)** avisa ao Go que esse canal tem duas vagas de estacionamento para dados.

2. Envio sem Bloqueio: A **main** consegue enviar os números **10** e **20** em sequência sem travar, porque o buffer armazena os valores temporariamente.

3. Leitura Direta: O programa esvazia o buffer linha por linha na hora de imprimir.

### Por que usar?

Se o seu sistema recebe dados mais rápido do que consegue processar, o buffer serve como uma área de escape para a aplicação não travar enquanto o resto do código termina o trabalho.

## 2. Leitura Continua com For Range e Close

Quando temos uma Goroutine enviando vários dados em sequência, ficar lendo linha por linha é inviável. Usamos o **for range** para ler o canal como se fosse uma lista móvel, e o **close** para avisar que a produção acabou.

```go
package main

import "fmt"

func gerarNumeros(canal chan int) {
	for i := 1; i <= 3; i++ {
		canal <- i
	}
	// AVISO ESSENCIAL: Fecha o canal para avisar que o envio terminou
	close(canal) 
}

func main() {
	novoCanal := make(chan int)

	// Dispara a Goroutine em segundo plano
	go gerarNumeros(novoCanal)

	// O 'for range' lê o canal automaticamente e só para quando detecta o 'close'
	for numero := range novoCanal {
		fmt.Println("Recebido da Goroutine:", numero)
	}
    
	fmt.Println("Fim da execução de forma segura!")
}
```
### O que o código faz?

1. Disparo em Segundo Plano: A **main** joga a função **gerarNumeros** para rodar em paralelo e criar os dados de 1 a 3.

2. Sinal de Finalização: Assim que o loop da Goroutine acaba, o comando **close(canal)** coloca uma placa de "Fim da Linha" no canal.

3. Leitura Inteligente: O **for range** fica escutando o canal. Ele puxa cada número assim que ele chega e encerra o loop sozinho quando percebe que o canal foi fechado.

### Por que usar?

Evita que você tenha que adivinhar quantas mensagens vai receber. Além disso, usar o **close** protege seu programa contra travamentos (Deadlocks), garantindo que nenhuma leitura vai ficar esperando dados para sempre.

!!! tip "Dica 💡"

	**Use canais unidirecionais para aumentar a segurança do código**

	Quando você passa um canal como argumento para uma função, você pode dizer ao Go se essa função vai apenas enviar ou apenas receber dados. Isso evita bugs onde uma função altera o canal sem querer.

```go
// Esta função apenas RECEBE dados do canal (Somente Leitura)
func consumir(canal <-chan string) {
    fmt.Println(<-canal)
}
```

!!! warning "Alerta ⚠️"

	**Nunca envie dados para um canal fechado!**

	Embora ler de um canal fechado seja seguro (ele apenas retorna o valor padrão do tipo), tentar enviar qualquer dado para um canal que já foi encerrado pelo comando **close()** vai quebrar o seu programa imediatamente com um erro de **panic: send on closed channel**. A regra é clara: quem envia é quem fecha.

## Resumo

Os Channels são o encaixe perfeito para as Goroutines. Eles eliminam a necessidade de travas complexas (mutexes) ou cronômetros improvisados na hora de gerenciar concorrência.

Ao unir a leveza das Goroutines com a segurança de entrega dos Channels, Go cumpre sua maior promessa: oferecer um modelo de alta performance robusto, onde os dados fluem de forma controlada, previsível e incrivelmente fácil de manter.