# Arrays, Slices e Maps em Go 

## Arrays em Go

## O que são Arrays

É uma estrutura de dados de tamanho fixo que armazena uma coleção de elementos do mesmo tipo. Uma vez definido o tamanho de um array na sua criação, ele não pode ser alterado, o que garante previsibilidade e segurança na memória.

## Exemplos de Array

```go
package main

import "fmt"

func main() {
    // 1. Declaração de Array com tamanho fixo [3]
    var redesSociais [3]string
    redesSociais[0] = "Instagram"
    redesSociais[1] = "LinkedIn"
    redesSociais[2] = "GitHub"

    // 2. Declaração Curta com valores iniciais
    notas := [4]float64{7.5, 8.2, 10.0, 6.4}

    // 3. Array com tamanho definido pelo compilador (...)
    // O Go conta quantos itens há entre as chaves
    cidades := [...]string{"Osasco", "São Paulo", "Barueri"}

    fmt.Println("Redes:", redesSociais)
    fmt.Println("Notas:", notas)
    fmt.Println("Cidades:", cidades)
}
```

## O que o código faz?

Cria listas organizadas na memória. Ele reserva espaços numerados (índices) onde você pode guardar informações relacionadas, como uma lista de nomes ou uma sequência de números, acessando cada item através de sua posição.

## Por que usar?

- Organização: Em vez de criar dez variáveis diferentes para dez nomes, você utiliza um único Array que agrupa todos eles sob um mesmo contexto.

- Performance: Como o tamanho do Array é fixo e conhecido pelo computador desde o início, o acesso aos dados é extremamente rápido.

- Segurança: O compilador garante que você não misture tipos diferentes (como tentar colocar um texto em um array de números), mantendo a integridade dos dados do seu sistema.

!!! tip "Dica 💡"

    Para saber o tamanho de um array de forma automática, use a função len(). Exemplo: fmt.Println(len(notas)). Isso é muito útil quando você precisa percorrer a lista usando um for sem precisar contar os itens manualmente.

!!! warning "Alerta ⚠️"

    Índice Fora de Alcance: Se você tem um array de tamanho 3 e tenta acessar o índice `[3]`, o programa dará erro. Lembre-se: a contagem sempre começa no 0 (os índices seriam 0, 1 e 2).

    Tamanho Imutável: Um Array não cresce. Se você definir `[5]int` e precisar guardar um sexto número, terá que criar um novo array ou usar um Slice (que veremos depois).

## Resumo

O Array é como uma caixa de ovos: você sabe exatamente quantos espaços existem e o que cabe dentro deles. É a forma mais básica e rápida de listar dados em Go, servindo como base para estruturas mais avançadas.

# Slices em Go

## O que são Slices

Diferente dos arrays, os Slices são "fatias" dinâmicas e flexíveis. Eles são a forma mais comum de lidar com listas de dados em Go, pois permitem que você adicione ou remova itens sem se preocupar com um tamanho fixo definido no início.

## Exemplos de Slices

```go
package main

import "fmt"

func main() {
    // 1. Declaração de Slice (Sem tamanho fixo nos colchetes [])
    frutas := []string{"Maçã", "Banana"}

    // 2. Adicionando itens com a função append
    // O Slice cresce automaticamente
    frutas = append(frutas, "Morango", "Uva")

    // 3. Criando um Slice a partir de um Array
    // Pega os itens do índice 1 até o 2 (o 3 é exclusivo)
    numeros := [5]int{10, 20, 30, 40, 50}
    fatia := numeros[1:3] 

    fmt.Println("Lista de Frutas:", frutas)
    fmt.Println("Fatia do Array:", fatia)
    fmt.Println("Tamanho do Slice:", len(frutas))
}
```

### O que o código faz? 

Cria listas que podem expandir conforme a necessidade do programa. Ele utiliza a função append para injetar novos dados na lista existente e mostra como "fatiar" uma estrutura maior para trabalhar apenas com uma parte específica dos dados.

### Por que usar?

- Flexibilidade: Você não precisa saber quantos itens terá na lista logo de cara. O Slice cresce conforme o seu programa recebe novos dados, como usuários se cadastrando em um site.

- Praticidade: Go oferece funções prontas (como append, copy e recursos de fatiamento) que tornam a manipulação de listas muito mais simples do que em arrays tradicionais.

- Eficiência: Por baixo dos panos, o Slice aponta para um Array, mas gerencia tudo de forma inteligente para que você não precise lidar com a complexidade de alocação de memória manualmente.

!!! tip "Dica 💡"

    Sempre que usar o append, lembre-se de atribuir o resultado à própria variável: meuSlice = append(meuSlice, "novo"). Isso é necessário porque, se o Go precisar aumentar o espaço de memória, ele criará um novo endereço e você precisa garantir que sua variável aponte para ele.

!!! warning "Alerta ⚠️"

    Capacidade vs Tamanho: O len() mostra quantos itens estão no Slice agora, mas o Slice tem uma "capacidade" (cap) que pode ser maior. Se você fatiar um array, o Slice resultante ainda estará ligado ao array original. Alterar o Slice pode alterar o Array original!

    Slices Vazios: Um slice declarado sem valores (ex: var lista []int) começa como nil. Embora o append funcione nele, tentar acessar um índice diretamente (`lista[0]`) antes de adicionar algo causará um erro.

## Resumo

O Slice é a versão "turbinada" do Array. É uma lista elástica que você usa para quase tudo em Go quando precisa armazenar coleções de dados que podem mudar de tamanho. É flexível, potente e a ferramenta favorita dos desenvolvedores para lidar com agrupamentos.

# Maps em Go

## O que são Maps

Maps são coleções desordenadas de pares chave-valor. Pense neles como um dicionário ou uma lista de contatos: você tem uma "chave" única (como um nome ou um CPF) e ela te leva diretamente a um "valor" (como um número de telefone ou os dados de uma pessoa). É a estrutura mais eficiente para buscas rápidas em Go.

## Exemplos de Maps

```go
package main

import "fmt"

func main() {
    // 1. Criando um Map com make (Chave: string, Valor: int)
    idades := make(map[string]int)
    idades["Arthur"] = 19
    idades["João"] = 25

    // 2. Declaração Curta com valores iniciais
    precos := map[string]float64{
        "Cerveja": 8.50,
        "Agua":    3.00,
    }

    // 3. Deletando um item do Map
    delete(idades, "João")

    fmt.Println("Idade do Arthur:", idades["Arthur"])
    fmt.Println("Menu de Preços:", precos)
}
```

### O que o código faz?
Cria tabelas de consulta. Ele associa uma informação de busca (chave) a um dado específico (valor). Ao contrário dos Arrays e Slices, onde você busca pela posição (0, 1, 2), aqui você busca pelo nome da chave que você mesmo definiu.

### Por que usar?

Busca Instantânea: Não importa se o seu Map tem 10 ou 1 milhão de itens; encontrar um valor através da chave é extremamente rápido, pois o Go não precisa percorrer a lista inteira.

Associação Lógica: Facilita a organização de dados que possuem um identificador único, como IDs de produtos, dicionários de tradução ou configurações de sistema.

Flexibilidade de Chaves: Você pode usar quase qualquer tipo comparável como chave (strings, ints, etc.), permitindo que você estruture seus dados da forma que fizer mais sentido para o problema.

!!! tip "Dica 💡"

    Para verificar se uma chave realmente existe no Map (evitando pegar um valor zero por erro), use a sintaxe de dois valores:

    valor, existe `:= idades["Marcos"]`.

    Se existe for false, significa que o Marcos não está no seu Map.

!!! warning "Alerta ⚠️"

    Maps Não Ordenados: Diferente dos Slices, os Maps não guardam a ordem em que os itens foram inseridos. Se você imprimir o Map duas vezes, a ordem dos itens pode mudar. Nunca dependa da ordem em um Map.

    Mapa Não Inicializado: Se você declarar apenas `var m map[string]int` (sem o make ou sem valores), ele será nil. Tentar adicionar um item em um mapa nil fará o programa quebrar (panic). Use sempre o make para inicializar mapas vazios.

## Resumo

O Map é o "Dicionário" do Go. Você fornece uma palavra-chave e ele te entrega o significado instantaneamente. É a ferramenta perfeita para quando você precisa organizar dados de forma que a recuperação seja rápida, direta e baseada em nomes ou IDs, em vez de posições em uma fila.    