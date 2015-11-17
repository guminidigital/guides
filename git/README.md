# GIT
Este guia apresenta o padrão de utilização do GIT utilizado pela Gumini. Aqui será apresentado o [padrão de commits](#commits), como [versionamos](#versionamento) as entrega, o [fluxo que utilizamos (gitflow)](#gitflow) e alguns [comandos importantes](#comandos) para ter na manga. Enjoy!

## Commits
*Under development*

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

## Comandos
*Under development*

### Deletando um branch, local e remoto
*Under development*

### Log de modificações de um arquivo
*Under development*

### Criando um tag
*Under development*
