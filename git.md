# GIT
Este é um guia de padrões para utilização do GIT utilizado pela Gumini. Aqui serão apresentadas as [boas práticas de commits](#commits), como [versionamos](#versionamento) as entregas, o [fluxo que utilizamos (gitflow)](#gitflow), como fazemos a [documentação básica](#documentacao) do projeto e alguns [comandos importantes](#comandos) para se ter na manga. Enjoy!

## Commits
Acima de tudo, os commits são uma **documentação histórica do que foi feito no projeto**. Ajuda outros desenvolvedores (e você mesmo) a pegar o código e entender o porquê de determinadas decisões feitas. Por isso, um bom commit deve responder "O que?", "Como?" e, o mais importante, "Por quê?" daquela alteração.

Nossos padrões de commits são inspirados no guia de boas práticas do [Tim Pope](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).

### Ilustrando uma situação
**Cenário:** É solicitado para aumentar a largura de um modal que está muito pequeno no mobile, que tem espaço sobrando nas laterais. Esta task é aberta para um desenvolvedor que não conhece o projeto.

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

**Motivo:** No ambiente de produção, o CMS adiciona estilos próprios que deixam um padding muito grande na modal. E, sem documentação, o dev não tinha como saber.


### Estrutura de um bom commit
Um commit deve ser pensado em 2 blocos: a primeira linha é a **mensagem**, que deve ser curta e direta, como um assunto de e-mail. O restante é o **corpo**, que contem mais explicações sobre as alterações feitas.

No commit, o corpo nem sempre é necessário. Se você finalizou a programação de uma nova página, um simples `git commit -m "Adiciona front da página de contato"` já explica muito bem o que aconteceu.

Porém, em situações como no caso acima, é necessário mais explicações para futura consulta do código. Por isso, um commit explicando 'o que?', 'como?' e 'por quê?' é importante:

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
Para padronizar e facilitar o pensamento na hora de escrever, é recomendado que a mensagem do commit complemente a frase **"Se aplicado, este commit..."**

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
O versionamento acontece em 2 momentos: release candidate (publicação em homologação) e release (publicação em produção).

### Formato
Usamos o formato semântico de versionamento (http://semver.org/lang/pt-BR/). O formato apresentado é o de 3 números: 1.2.3, sendo:

1 - MAJOR: Envolve grandes mudanças, como um novo layout de um site ou uma reestruturação completa de um sistema. Qualquer mudança que torne incompatível a versão anterior.

2 - MINOR: Quando adicionadas novas funcionalidades, sem perder compatibilidade com a versão anterior. Por exemplo, uma nova tela que foi adicionada, um novo fluxo de login, uma nova funcionalidade no admin.

3 - PATCH: Uma correção da versão em produção. Se foi encontrado um BUG na versão que está em produção, após corrigido, acresce 1 no PATCH.

### Release candidate (homologação)
Cada versão que for disponibilizada para homologação, é um release candidate. É com esta versão que nós ou o cliente validamos e fazemos os ajustes necessários. O versionamento segue o padrão da versão que vai ser feito o release, seguido por -rc{numero}. Cada nova publicação recebe um novo rc: 1.0.0-rc1, 1.0.0-rc2, ect...

### Release
Toda versão que for colocada em ambiente de produção é uma versão de release e recebe a versão no formato 1.0.0. Cada adição de recurso, acresce um número na segunda casa (MINOR), tornando 1.1.0. Cada correção em produção (hotfix) recebe uma versão na terceira casa (PATCH), tornando 1.1.1.

## Gitflow
Para os projetos da Gumini, adotamos o GitFlow, metodologia do [Vincent Driessen (nvie.com)](http://nvie.com/posts/a-successful-git-branching-model/)

### Branches fixas
- **_develop_**: representa a trilha principal de desenvolvimento. Todos os branches devem ser feitos à partir dela (exceto hotfixes, como explicado abaixo) e ela deve estar sempre equiparada ou a frente da **_master_**
- **_master_**: representa os deploys, todos devidamente tagueados. Os commits só entram na master após terem sido finalizadas as QAs internas e do cliente

### Branches de apoio
Os branches abaixo devem ser deletados do ambiente local e remoto uma vez que forem integrados aos branches fixos

- **_nome-da-feature_** Branches para novas features. Devem ser feitas à partir do **_develop_** e constantemente feito rebases para manter atualizada.
- **_release/v1.0_** Branches pré release. São feitas à partir da **_develop_**. É aqui onde são feitos os ajustes apontados no QA para depois ser integrada à **_master_**. Só pode ter um release por vez.
- **_hotfix_** Branches para hotfix, feitas à partir da **_master_**, para corrigir erros em produção. Uma vez que o fix tenha sido concluido, deve ser feito merge de volta para a **_master_** e para **_develop_**. 

### IMPORTANTE
Nada – absolutamente nada – volta para as branches fixas sem terem sido feitos merge e testadas nas branches de apoio. 

Exemplo: se está trabalhando na feature **_recurso-xyz_**, antes de mandar para **_develop_**, faça um merge de **_develop_** dentro de **_recurso-xyz_**, corrija qualquer problema que possa ter acontecido e, só então, faça o merge em **_develop_**.

### Fluxo dentro do projeto
#### Iniciando um projeto
- Nasce com um readme.md na **_master_** com informações básicas do projeto. 
- Deste commit, nasce a branch **_develop_** e do **_develop_**, já uma branch de feature, como **_dev-inicial_** onde seguira o desenvolvimento inicial.

#### Integrando ao **_develop_**
Fluxo adotado ao finalizar uma feature

- Faça o merge do **_develop_** em **_recurso-xyz_**
- Teste. E teste de novo.
- Faça checkout do **_develop_**
- Faça o merge do **_recurso-xyz_** em **_develop_** com `--no-ff`: `$ git merge --no-ff recurso-xyz`
- [Delete o branch](#deletando-um-branch-local-e-remoto) **_recurso-xyz_**, local e remoto

#### Entregando para QA
Desenvolvimento finalizado? Chegou a hora de fazer a entrega para QA! \o/

- Integre todos branchs de features que fazem parte do release no **_develop_** (Ver "Integrando ao **_develop_**")
- Faça um branch de release, à partir do **_develop_**, com o versionamento atual. Ex.: **_release/v1.0_**. É neste branch que serão feitos os ajustes de QA.
- Faça todos os ajustes commitando direto neste branch, mesmo que tenha mais de uma pessoa trabalhando nos ajustes.

#### Fazendo a entrega
Agora que toda QA foi finalizada, hora de mandar para a **_master_** e versionar.

- Faça o merge da **_master_** em **_release/v1.0_**
- Teste. E teste de novo. E, como é a **_master_**, teste uma terceira vez.
- Faça checkout na **_master_**
- Faça o merge do **_release/v1.0_** em **_master_** com `--no-ff`: `$ git merge --no-ff release/v1.0`
- Crie uma tag com a [versão](#versionamento) na **_master_**: `$ git tag v1.0` seguido de `$ git push origin v1.0`
- [Delete o branch](#deletando-um-branch-local-e-remoto) **_release/v1.0_** local e remoto

#### Ajustes do código em produção
O cliente achou um erro em produção! Socorro!

- Faça um branch chamado **_hotfix_** à partir da tag mais atual na **_master_**
- Faça as correções necessárias e teste bem
- Faça checkout na **_master_**
- Faça o merge do **_hotfix_** em **_master_** com `--no-ff`: `$ git merge --no-ff hotfix`
- Crie uma tag com a [versão](#versionamento) na **_master_**: `$ git tag v1.0.1` seguido de `$ git push origin v1.0.1`
- Faça checkout na **_develop_**
- Faça o merge do **_hotfix_** em **_develop_** com `--no-ff`: `$ git merge --no-ff hotfix`
- [Delete o branch](#deletando-um-branch-local-e-remoto) **_hotfix_** local e remoto

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
