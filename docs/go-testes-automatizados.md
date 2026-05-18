# Testes Automatizados em Go

No desenvolvimento de software moderno, garantir que o código funciona hoje e continuará funcionando amanhã é fundamental. Em Go, os testes não são um detalhe deixado para depois ou que exige ferramentas de terceiros; eles fazem parte do núcleo da linguagem.

## O que são os Testes Automatizados?

Testes automatizados são scripts de código escritos para verificar se o seu código principal está se comportando exatamente como esperado. Em vez de abrir o programa e testar manualmente cada fluxo clicando ou enviando dados toda vez que faz uma alteração, você roda um comando que valida tudo em milissegundos.

Um dos lemas da linguagem Go é "testar é parte do processo de build".

### Como eles funcionam em Go?

Para que o Go identifique e execute seus testes automaticamente, você só precisa seguir três regras de padronização:

1. **O Sufixo do Arquivo (_test.go)**

O compilador do Go ignora arquivos de teste na hora de gerar o executável final do sistema. Para o Go saber o que é código de produção e o que é código de teste, todo arquivo de teste deve terminar obrigatoriamente com **_test.go**.

- Exemplo: Se o seu arquivo de lógica se chama **calculadora.go**, o arquivo de teste dele será **calculadora_test.go**.

2. **O Nome da Função (TestXxx)**

Dentro do arquivo de teste, as funções de validação precisam começar com a palavra **Test** seguida de uma letra maiúscula. Elas também devem receber sempre o ponteiro de controle de testes do Go: **t *testing.T**.

- Exemplo: **func TestSoma(t *testing.T) { ... }**

3. **O Comando Mágico (go test)**

Para rodar os testes, você não executa o arquivo com **go run**. Você abre o terminal na pasta do projeto e digita:

```go
go test
```

O Go vai varrer a pasta, encontrar os arquivos **_test.go**, executar as funções **TestXxx** e exibir um relatório dizendo se o código passou (**PASS**) ou falhou (**FAIL**).

## Tipos de Testes em Go

O ecossistema do Go é incrivelmente maduro e divide as validações em camadas para garantir a segurança da aplicação:

- **Testes de Unidade:** Validam a menor parte do código isoladamente (como uma função de cálculo). Se o resultado for errado, usamos **t.Errorf** para avisar o Go.

- **Testes Orientados a Tabela (Table-Driven Tests):** A prática mais comum em Go. Em vez de criar dez funções de teste diferentes, criamos uma lista (tabela) com vários cenários de entrada e saída esperada e testamos todos de uma vez dentro de um único loop.

- **Benchmarks:** Servem para medir a performance e a velocidade do seu código. Usam funções que começam com a palavra **Benchmark** e são executados com o comando **go test -bench=.**.

- **Mocks e Integração:** Usados em cenários avançados para testar a comunicação entre seu código e o mundo externo. Os Mocks (como a ferramenta **gomock**) simulam comportamentos, enquanto os Testes de Integração testam o sistema real usando ferramentas como **Testcontainers** para subir bancos de dados (PostgreSQL, Redis) temporários durante o teste.

## Exemplos de Testes Automatizados

## 1. Teste de Unidade Básico (O Começo de Tudo)

Imagine que você tem um arquivo chamado **calculadora.go** com uma função simples:

```go
package calculadora

// Soma adiciona dois números inteiros
func Soma(a, b int) int {
	return a + b
}
```
Para validar essa função, criamos o arquivo **calculadora_test.go** na mesma pasta:

```go
package calculadora

import "testing"

func TestSoma(t *testing.T) {
	// 1. Cenário de teste
	resultadoEsperado := 5
	
	// 2. Execução da função real
	resultadoReal := Soma(2, 3)

	// 3. Validação
	if resultadoReal != resultadoEsperado {
		// Se der erro, o t.Errorf avisa o Go e para o teste
		t.Errorf("Soma(2, 3) falhou: esperado %d, obtido %d", resultadoEsperado, resultadoReal)
	}
}
```
### O que o código faz?

1. Importação do pacote **testing**: Dá acesso ao tipo ***testing.T**, que gerencia o status do teste (se passou ou falhou).

2. Comparação manual: Usamos um **if** simples para checar se o comportamento bate com o esperado.

3. Mensagem de Erro (**t.Errorf**): Se o resultado for diferente, o Go registra a falha e mostra exatamente onde o cálculo errou quando você rodar **go test**.

### Por que usar?

É a forma mais rápida de isolar e garantir que uma função específica do seu sistema está calculando ou processando dados corretamente antes de enviá-la para produção.

## 2. Table-Driven Test (O Padrão de Mercado)

Ficar criando uma função de teste inteira para cada cenário (um teste para números positivos, outro para negativos, outro para zero) gera muita repetição de código. Em Go, resolvemos isso criando uma **tabela de cenários**.

No mesmo arquivo **calculadora-test.go**, podemos testar múltiplos casos assim:

```go
package calculadora

import "testing"

func TestSomaEmMassa(t *testing.T) {
	// Criamos uma estrutura anônima para definir os campos da nossa tabela
	testes := []struct {
		nome     string // Nome do cenário para identificar no relatório
		a, b     int    // Entradas da função
		esperado int    // Saída que a gente espera receber
	}{
		{"Soma de positivos", 2, 3, 5},
		{"Soma com zero", 5, 0, 5},
		{"Soma de negativos", -1, -1, -2},
		{"Soma resultando em zero", -2, 2, 0},
	}

	// O loop roda cada linha da tabela de forma automatizada
	for _, cenario := range testes {
		// t.Run isola cada linha como se fosse um mini teste independente
		t.Run(cenario.nome, func(t *testing.T) {
			resultado := Soma(cenario.a, cenario.b)
			if resultado != cenario.esperado {
				t.Errorf("Falha no cenário '%s': Soma(%d, %d) = %d; esperado %d", 
					cenario.nome, cenario.a, cenario.b, resultado, cenario.esperado)
			}
		})
	}
}
```
### O que o código faz?

1. Estrutura de Dados (**struct**): Criamos uma "matriz" ou lista onde cada linha representa um teste completo (com entradas e saídas esperadas).

2. Subtestes com **t.Run**: Essa função permite rodar cada cenário individualmente. Se apenas o teste de números negativos falhar, o relatório do terminal vai apontar exatamente para a linha **"Soma de negativos"**, enquanto as outras passam com sucesso.

### Por que usar?

É o padrão absoluto da comunidade Go. Evita a duplicação de código de validação (os **if/else** de erro) e permite adicionar dezenas de novos cenários de teste apenas adicionando uma nova linha de texto na struct, mantendo o código limpo e escalável.

## Dica 💡

**Use go test -v para ver os detalhes**

O comando padrão é muito econômico. Adicionando a flag **-v** (verbose), o Go mostra o nome de cada cenário testado e o tempo exato que ele levou para rodar.

## Alerta ⚠️

**Cuidado com o Cache do Go!**

Se o código não mudou, o Go pode reaproveitar o resultado do teste anterior para economizar tempo (mostrando **(cached)**). Se o seu teste depende de coisas externas (como a hora do sistema), use **go** **test -count=1** para forçar o Go a rodar o teste de verdade.

## Resumo

Escrever testes automatizados em Go deixa de ser uma obrigação burocrática e se torna uma extensão natural do desenvolvimento. Graças às ferramentas nativas e robustas da linguagem, você ganha a certeza de que qualquer refatoração futura não vai quebrar as regras de negócio já existentes.

Dominar os testes orientados a tabela e entender como isolar suas validações é o divisor de águas que transforma um programador iniciante em um desenvolvedor pronto para construir sistemas resilientes e escaláveis em nível de produção.