# **Documentação do Projeto: CTT AP2**

Este repositório contém o projeto de documentação colaborativa sobre a linguagem Go , desenvolvido utilizando o gerador de sites estáticos Zensical. A publicação é totalmente automatizada via GitHub Actions e hospedada no GitHub Pages.

### **Integrantes do Grupo**

Arthur Pimentel – 2500707\
Gabriel Faria - 2500567\
Henrique Gonçalves Costa - 2500110\
Isabelle Oliveira da Silva - 2501128


### **Fluxo de Trabalho Colaborativo (Git Flow)**
Para garantir a estabilidade do projeto e simular um ambiente profissional de desenvolvimento , a equipe adotou um fluxo estrito de trabalho baseado em Feature Branches , Pull Requests (PRs) e Code Review.

### **Proteção da Branch main**

A branch main foi configurada com regras estritas de proteção ([Branch Protection Rules](https://github.com/impacta-gb/projeto-ctt-ap2-camelcase/settings/branches)) que impedem qualquer tipo de push direto. Toda e qualquer alteração deve, obrigatoriamente, passar pelo fluxo de validação e revisão.

### **Ciclo de Desenvolvimento de Conteúdo**

Cada página da documentação ou alteração de infraestrutura seguiu rigorosamente os seguintes passos: O desenvolvedor responsável criava uma Feature Branch para cada respectivo conteúdo, o conteúdo em Markdown é enriquecido com recursos do Zensical, utilizando Syntax Highlighting, Admonitions, tabelas e barras de navegação personalizadas. Ao finalizar o trabalho, o desenvolvedor enviava a branch para o GitHub e abria um PR direcionado à branch main. A abertura do PR engatilha automaticamente o pipeline de CI (GitHub Actions) para validar se o build do site quebrou antes da aprovação. Pelo menos um outro integrante do grupo revisava o código e o conteúdo do PR, deixando feedbacks construtivos e aplicando o Approve caso tudo estivesse correto. Com o sinal verde do pipeline e a aprovação do revisor, o merge era consolidado na main.


### **Estrutura do Conteúdo Desenvolvido**
O site foi mapeado para cobrir tópicos essenciais da linguagem Go, divididos em módulos específicos:

- [Introdução e Instalação](docs/01-introducao-e-instalacao.md)
- [Sintaxe Básica e Variáveis](docs/02-sintaxe-e-variaveis.md)
- [Estruturas de Controle (If, For, Switch)](docs/03-estruturas-de-controle.md)
- [Coleções Avançadas (Arrays, Slices e Maps)](docs/04-arrays-slices-e-maps.md)
- [Estruturas de Dados (Structs e Métodos)](docs/go-structs-métodos.md)
- [Tratamento de Erros (Error Handling)](docs/go-error-handling.md)
- [Concorrência I: Goroutines](docs/go-goroutines.md)
- [Concorrência II: Channels](docs/go-channels.md)
- [Gerenciamento de Pacotes (Go Modules)](docs/05-go-modules.md)
- [Testes Automatizados em Go](docs/go-testes-automatizados.md)

### **Arquitetura do Workflow (GitHub Actions)**
O fluxo de automação é composto por dois jobs desacoplados e dependentes:
Gatilhos Avançados (Triggers): O pull_request para a main dispara a validação de build em tempo real no PR para que o revisor saiba se o código está quebrando o site. O push na main dispara o build final e a publicação oficial após o merge do PR. O schedule executa uma rotina semanal automatizada utilizando a sintaxe cron para garantir a validação periódica.

Job de Construção e Resiliência (build_site): Estratégia de Matriz configurada para rodar simultaneamente utilizando a matrix em duas versões diferentes do Python, garantindo que a documentação seja gerada sem erros em diferentes ambientes. A action actions/cache mapea as bibliotecas do Python instaladas via pip para evitar downloads redundantes e acelerar o processo. O HTML gerado pelo Zensical é compactado e exportado usando a action de upload de artefato.

Job de Publicação Segura (deploy_site): Este job foi configurado para aguardar estritamente a conclusão bem-sucedida do build utilizando a diretiva needs. Contem uma condicional de segurança, uma trava de segurança que impede que o deploy ocorra durante a análise de Pull Requests. O deploy só é executado quando houver um push na branch main ou no evento de schedule. Por fim, faz o download do artefato gerado no ambiente de build antes de publicá-lo oficialmente no GitHub Pages.