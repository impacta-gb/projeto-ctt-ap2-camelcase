# Structs e Métodos em Go

## O que são Structs?

Em Go, struct é uma coleção de campos de dados agrupados sob um único tipo. São usadas para agrupar dados relacionados e formar registros personalizados, servindo como base para estruturar informações.

## Exemplo de Struct

```go
type Pessoa struct {
    Nome string
    Idade int
    Profissao string
}

func main() {
    p1 := Pessoa {
        Nome: "Lucas",
        Idade: 26,
        Profissão: "Empreendedor"
    }

    fmt.Println(p1.Nome)
}
```
### O que o código faz?

1. Define a estrutura: O bloco **type Pessoa struct** avisa ao computador que, a partir de agora, existe um tipo de dado chamado **Pessoa** que sempre terá um nome (string), uma idade (int) e uma profissão (string).

2. Cria um registro: Na função **main**, preenchemos esse molde com dados específicos de uma pessoal real.

3. Organiza o acesso: O uso do ponto **(p1.Nome)** permite que o programa saiba exatamente qual pedeço de informação você quer recuperar daquele conjunto.

### Por que usar?

Agrupamento Lógico: Em vez de ter três variáveis soltas (**nome1, idade1, prof1**), você tem um único objeto **p1** que contém tudo. Isso deixa o código muito mais limpo.

Facilidade de Transporte: Se você precisar enviar os dados dessa pessoa para outra parte do prigrama (uma função, por exemplo), você envia apenas a **Pessoa**, e não cada campo individuaalmente.

Padrão de Mercado: É a forma padrão e mais eficiente de representar entidades(Usuários, Produtos, Pedidos) em sistemas modernos e APIs.

## Dica 💡

As structs são usadas o tempo todo para enviar dados para a internet (JSON). O Go permite colocar "tags" nos campos para dizer como eles devem ser chamados lá fora.

## Alerta ⚠️

Quando você passa uma struct para uma função, elas são copiadas por padrão, o Go não usa a original. Se quiser mudar o original ou economizar memória, use um **Ponteiro (*)**. É como dar a chave da casa original em vez de construir uma réplica.

## Resumo

As **structs** são o coração da organização de dados em Go. Elas permitem que você saia de variáveis soltas para criar seus póprios tipos de dados complexos e organizados.

## O que são Métodos?

Um **método** é basicamente uma função que possui um "destinatário" (chamado de receiver). Em termos simples, são funções especiais associadas a um tipo (geralemnte uma **struct**). Enquanto uma função comum pode ser chamada de forma independente, um método só pode ser chamado a partir de uma instância específica daquele tipo.

## Exemplo de Métodos

```go
type Pessoa struct {
    Nome string
    Idade int
}

func (p Pessoa) Saudacao() string {
    return "Olá, meu nome é " + p.Nome
}

func main() {
    p1 := Pessoa{Nome: "Lucas", Idade: 26}

    fmt.Println(p1.Saudacao())
}
```
### O que o código faz?

1. Define o Receiver: O trecho **(p Pessoa)** antes do nome da função avisa ao Go que essa função "pertence" ao tipo **Pessoa**.

2. Executa a Ação: O método acessa os dados internos da struct (**p.Nome**) para realizar uma tarefa.

### Por que usar?

Encapsulamento: Você agrupa o comportamento junto com o dado. Se uma **Pessoa** sabe falar, o código que faz ela falar deve estar "dentro" dela.

Organização: Evita que você tenha centenas de funções soltas no código. Você sabe exatamente quais ações cada tipo de dado pode realizar.

## Dica 💡

Use **nome curtos**

No Go, o nome do receiver (o **p** no exemplo) deve ser curto, geralmente a primeira letra do tipo. Evite nomes longos como **self** ou **this**, comuns em outras linguagens. Isso mantém o código limpo e no padrão da comunidade. 

## Alerta ⚠️

**Receiver de Valor vs .Ponteiro:**

Se você usar **(p Pessoa)**, o método recebe uma **cópia** e não consegue alterar os dados da pessoa original (ex: mudar a idade).

Se você usar **(p Pessoa)**, o método recebe um **ponteiro** e consegue modificar os dados originais.

Na dúvida, use ponteiros para evitar cópias desnecessárias na memória. 

## Resumo

Métodos dão "vida" às suas structs. Eles definem o que seus dados podem fazer.