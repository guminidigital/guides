# GIT
Este guia apresenta o padrão de utilização do GIT utilizado pela Gumini. Aqui será apresentado as [boas práticas de commits](#commits), como [versionamos](#versionamento) as entrega, o [fluxo que utilizamos (gitflow)](#gitflow), como fazemos a [documentação básica](#documentacao) do projeto e alguns [comandos importantes](#comandos) para ter na manga. Enjoy!

## Commits
Acima de tudo, os commits são uma **documentação histórica do que foi feito no projeto**. Ajuda você mesmo e outro desenvolvedor que pegar o código a entender o porquê de determinadas decisões feitas. Por isso que um bom commit deve responder "O que?", "Como?" e, o mais importante, "Por quê?".

Nosso guia de boas práticas de commits é inspirado no guia de boas práticas do [Tim Pope](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)

### Ilustrando uma situação
**Cenário:** Solicitação para aumentar a largura de um modal que está muito pequeno no mobile, que tem espaço sobrando nas laterais. Esta task é aberta para um desenvolvedor que não conhece o projeto.

**O que o desenvolvedor sabe:**

- O menor device especificado é um iPhone 5, que tem 320px de largura
- O código está setado em 220px de largura, 100px a menos que a largura máxima
- O último desenvolvedor não se recorda do motivo de ter setado este tamanho

**Histórico:**

```
$ git log -L29:+1:css/arquivo.css
commit c42a109c362d2c8828b5a741421ccee0d6e68af9
Author: Fulano de Tal <fulano@gumini.com.br>
(...)
	Ajuste de tamanho da modal

-	width: 260px;
+	width: 220px;


commit b1e797bec22d95ad4f2379bd1bfffb5479901264
Author: Fulano de Tal <fulano@gumini.com.br>
(...)
	Criada Modal XYZ

+	width: 260px;
```

**Ação:** O dev vê que o modal começou com 260px e depois foi alterado para 200px, e o commit não diz nada além de "Ajuste de tamanho da modal". O dev então altera para 300px para extrair o tamanho máximo possível dentro das specs.

**Resultado:** Uma nova task é aberta reclamando que no iPhone 5 apareceu um scroll horizontal.

**Motivo:** No ambiente de produção, o CMS adiciona estilos próprios que deixam um padding muito grande na modal. E, sem documentação, o dev não tínha como saber.


### Estrutura de um bom commit
Um commit deve ser pensado em 2 blocos, a primeira linha é a **mensagem**, curta e direta, como um assunto de e-mail, e o restante é o **corpo**, que contem mais explicações.

No commit, o corpo nem sempre é necessário. Se você finalizou a programação de uma nova página, um simples `git commit -m "Adiciona front da página de contato"` já explica muito bem o que aconteceu.

Porém, em situações como no caso acima, é necessário mais explicações para futura consulta do código, um commit explicando 'o que?', 'como?' e 'por quê?' é importante:

```
Resolve #123: Reduz largura da modal

A lib de modal utilizada é padrão do CMS e, no ambiente 
de produção, é adicionado um padding de 50px nas 
laterais da modal. 

Para resolver este problema, a largura da modal foi 
alterada para 220px de largura, respeitando a especificação 
mínima do projeto que é iPhone 5 (320px)
```

### Word wrap, line break e formatação
Para melhor visualização do histórico no GIT, é recomendado respeitar algumas regras:

- Limite de 50 caracteres para a mensagem
- Limite de 72 caracteres para o corpo
- Espaço de uma linha entre mensagem e corpo
- Mensagem começando com letra maiúscula e sem ponto no final
- Cada parágrafo no corpo deve ter separação de uma linha

É importante ressaltar que esses limites são uma questão de boas práticas e não um requisito técnico.

### Commits em pt-BR
Porque, né... é a nossa língua! 

Inglês é a linguagem da programação? Sim. 
Todo mundo é fluente? Não. 

Se escrever um bom commit já pode ser desafiador em português, é muito mais complicado você tem que pensar em 'o que?', 'como?' e 'por quê?' em outra língua. Então, segue com pt-BR!

### Mensagem do commit
Para padronizar, é recomendado que a mensagem do commit complemente a frase **"Se aplicado, este commit..."**

- ... Melhora a renderização das fontes da home
- ... Torna dinâmica a página de quem somos
- ... Resolve #123: Quebra de layout nos serviços
- ... Adiciona front da página contato
- ... Altera a URL de Serviços
- ... Atualiza o banco de dados
- ... Corrige tamanho do box de contato

### Uma cola

1. Commits sempre em português
- Limite de caracteres: 50 para mensagem, 72 para o corpo
- Um commit deve responder 'o que?', 'como?' e 'por quê?'.
- Formatação! Letra maiúscula, não pontuar a mensagem, folga de uma linha.
- Mensagem do commit sempre completando a frase "Se aplicado, este commit..."

## Versionamento
*Under development*

## Gitflow
Para os projetos da Gumini, está sendo adotado o fluxo do GitFlow, métodologia do Vincent Driessen (nvie.com).

Informações completas sobre o método aqui: http://nvie.com/posts/a-successful-git-branching-model/

### Branches fixas
- `develop` representa a trilha principal de desenvolvimento. Todos os branches devem ser feitos à partir dela e ela deve estar sempre equiparada ou a frente da `master`
- `master` representa os deploys, todos devidamente tagueados. Os commits só entram na master após terem sido finalizadas as QAs internas e do cliente

### Branches dinâmicas
Os branches abaixo devem ser deletados do ambiente local e remoto uma vez que forem integrados aos branches fixos
- `feature/xyz` Branches para novas features. Devem ser feitas à partir do `develop` e constantemente feito rebases para manter atualizada.
- `release/1.0` Branches pré release. São feitas à partir da `develop`. É aqui onde são feitos os fixes da QA para depois ser integrada à `master`. Só pode ter um release por vez.
- `hotfix/1.1` Branches para hotfix, feitas à partir da `master` para corrigir erros em produção. Uma vez que o fix tenha sido concluido, deve ser feito merge de volta para a `master` e para `develop` 

### IMPORTANTE
Nada – absolutamente nada – volta para as branches fixas sem terem sido feitos o merge e testadas nas branches dinâmicas. Exemplo: se está trabalhando na `feature/xyz`, antes de mandar para `develop`, faça um merge de `develop` dentro de `feature/xyz`, corrija qualquer problema que possa ter acontecido e, só então, faça o merge em `develop`.

### Fluxo dentro do projeto
#### Iniciando um projeto
- Nasce com um readme.md na `master` com informações básicas do projeto. 
- Deste commit, nasce a branch `develop` e do `develop`, já uma branch `feature/xyz` onde seguira o desenvolvimento inicial.

#### Integrando ao `develop`
Fluxo adotado ao finalizar uma feature

- Faça o merge do `develop` em `feature/xyz`
- Teste. E teste de novo.
- Faça o merge do `feature/xyz` em `develop`
- [Deletar o branch](#deletando-um-branch-local-e-remoto) `feature/xyz` local e remoto

#### Entregando para QA
Desenvolvimento finalizado? Chegou a hora de fazer a entrega para QA! \o/

- Integre todos branchs de features que fazem parte do release no `develop`
- Faça um branch de release, à partir do `develop`, com o versionamento atual. Ex.: `release/1.0`. É neste branch que serão feitos os ajustes de QA.
- De preferência, faça todos os ajustes commitando direto neste branch, mesmo que tenha mais de uma pessoa trabalhando. Nenhum ajuste deve ser tão grande que precise dividir. Se for realmente necessário dividir, utilize o padrão `release/1.0/divisao`

#### Fazendo o release
Agora que toda QA foi finalizada, hora de mandar para a `master` e versionar.

- Faça o merge da `master` em `release/1.0`
- Teste. E teste de novo. E, como é a `master`, teste uma terceira vez.
- Faça o merge do `release/1.0` em `master`
- Crie um tag com a [versão](#versionamento) na `master`

## Documentação
*Under development*

## Comandos
*Under development*

### Deletando um branch, local e remoto
*Under development*

### Log de modificações de um arquivo
*Under development*

### Criando um tag
*Under development*
