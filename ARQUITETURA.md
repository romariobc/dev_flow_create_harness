# Arquitetura do dev_flow_criate — modelo de três camadas

Este documento nomeia explicitamente um modelo que já existia implícito na estrutura de pastas, e define a regra que estava faltando: quando uma ideia teórica pode se tornar regra operacional do harness.

## As três camadas

**`references/`** — pesquisa e teoria. Tudo que foi lido, sintetizado ou diagramado a partir de fontes externas (artigos, ebooks, posts sobre harness engineering). Não há compromisso de uso: uma ideia pode viver aqui indefinidamente sem nunca se tornar regra. É a camada do "porquê" e do "o que existe na literatura".

**`dev-flow-harness/`** — trunk de engenharia de contexto, aplicado e reutilizável. Tudo aqui é regra ativa: vale para qualquer projeto, antes de qualquer linha de código ser escrita. É o produto real deste workspace — não um app, mas o scaffold que qualquer app futuro vai herdar.

**`projetos/<projeto>/`** — instância concreta. Onde o trunk encontra um domínio real, ganha `context/` (o adaptador) e finalmente `src/` (o código). Nenhum projeto real existe ainda — `_template-adapter/` é molde vazio.

## Por que nomear isso agora

Sem um modelo nomeado, cada decisão sobre "isso é teoria ou regra?" e "isso já existe ou é gap real?" precisa ser refeita do zero a cada conversa — exatamente o problema que motivou a auditoria do documento de gaps analisado recentemente, que comparou regras propostas contra um conjunto de arquivos que não existiam neste workspace. Nomear a camada não resolve sozinho esse tipo de erro, mas elimina a ambiguidade sobre onde cada peça de conhecimento deveria morar.

## As duas direções de promoção

O harness evolui em duas direções, e cada uma tem (ou agora tem) uma regra própria.

### 1. De baixo para cima: projeto → harness

Já estava descrita em `dev-flow-harness/README.md`, seção "Evolução": uma lição aprendida em um projeto real deve subir do `context/adaptacoes.md` daquele projeto para o harness global, generalizando a regra. `05-harness-evals/metricas.md` já mede isso ("número de regras puxadas de um adaptador de projeto").

### 2. De cima para baixo: teoria → harness

Esta direção não tinha regra escrita até hoje. Uma ideia de `references/` só sobe para `dev-flow-harness/` quando passa por todos os critérios abaixo — não basta "parecer importante":

1. **Recorrência** — apareceu em mais de uma fonte teórica independente, ou foi citada mais de uma vez em discussões reais sobre este projeto. Uma leitura isolada não justifica promoção.
2. **Aplicabilidade direta ao stack declarado** — serve ao Python/LangChain/LangGraph/LangSmith/Hugging Face/Ollama já adotado, sem exigir trocar de stack ou adotar ferramenta nova.
3. **Custo de adoção baixo a médio** — cabe como seção ou arquivo dentro da estrutura `01-05` existente, sem exigir reestruturação do harness.
4. **Não duplica algo que já existe sob outro nome** — checar contra o conteúdo real dos arquivos antes de assumir ausência. Foi o erro principal do documento de gaps analisado: declarar "ausente" sem ler o que já existia.
5. **Operacionalizável como critério checável** — se a ideia não pode se tornar um controle computacional ou inferencial verificável (ver `05-harness-evals/tipologia-controles.md`), ela permanece em `references/` como pano de fundo teórico, e não vira regra.

Se algum critério falhar, a ideia continua em `references/` — isso não é rejeição definitiva, é "ainda não madura para regra".

### Regra simétrica de revisão (downgrade)

Uma regra promovida não é permanente por padrão. Se `metricas.md` registrar divergências repetidas no mesmo ponto do harness — `context/adaptacoes.md` de múltiplos projetos divergindo do mesmo jeito da mesma regra — isso é sinal de que a regra promovida pode estar errada ou desatualizada, não que os projetos estão "desviando". Esse princípio já existia em `metricas.md`; este documento só o conecta explicitamente à regra de promoção acima.

## Quem decide e onde isso fica registrado

Hoje a decisão é humana (Romário, com ou sem apoio do tutor de IA), registrada em `dev-flow-harness/CHANGELOG.md` — cada promoção vira uma linha: data, conceito, origem em `references/`, destino no harness. Sem esse registro, a auditabilidade que o próprio harness exige de guardrails de dados sensíveis (`04-guardrails-seguranca/dados-sensiveis-por-dominio.md`) não estaria sendo aplicada ao harness sobre si mesmo.

## Variação por domínio: branches

As duas direções acima descrevem como uma ideia sobe ou desce entre `references/` e `dev-flow-harness/`. Há uma terceira pergunta, lateral às duas: como o mesmo trunk agnóstico serve domínios diferentes (saúde, e outros que vierem) sem que o trunk pare de ser agnóstico?

### A regra

`main` nunca contém conteúdo específico de domínio. Qualquer guardrail, vocabulário ou exemplo que só faz sentido para um domínio (dado de saúde, dado financeiro, etc.) vive em uma branch derivada, nomeada `dominio/<nome>` (ex.: `dominio/saude`). Essa branch:

- parte de `main` e herda 100% do trunk agnóstico;
- adiciona os arquivos específicos daquele domínio (ex.: `04-guardrails-seguranca/dados-sensiveis-saude.md`, instanciando o padrão de `dados-sensiveis-por-dominio.md`);
- é onde `projetos/<projeto>/` daquele domínio são de fato criados e evoluem. Dentro da branch de domínio, cada projeto ainda decide sua própria arquitetura concreta (web, mobile, backend, banco de dados etc.) em `context/arquitetura.md`. Plataforma/visão (web, mobile) é detalhe de projeto, não uma camada nova do harness.

### Por que branch, e não uma pasta nova dentro do trunk

A divergência entre domínios é real e duradoura (vocabulário, base legal, exemplos), não uma variação superficial — o mesmo tipo de coisa que o modelo de três camadas já trata como divisão de responsabilidade. Uma pasta nova dentro do trunk (`dev-flow-harness/dominios/saude/`) reintroduziria o problema que motivou esta revisão: conteúdo de domínio morando dentro do que este documento define como "vale para qualquer projeto".

### O custo, e como mitigar

Toda branch de longa duração acumula dívida: uma correção feita em `main` precisa ser propagada manualmente para cada branch de domínio. Mitigação operacional: sempre que uma promoção for registrada em `CHANGELOG.md`, faça merge de `main` em cada branch de domínio ativa antes de continuar trabalhando nela (`git checkout dominio/<nome> && git merge main`). É trabalho real, mas previsível — e bem menor do que o custo de um trunk poluído com conteúdo que a maioria dos projetos nunca vai usar.

#### Risco conhecido: `merge main` pode apagar arquivo específico do domínio, mesmo sem fast-forward

`git merge main` dentro de uma branch de domínio não é garantia de segurança só porque o tip de `main` não é ancestral do tip da branch de domínio — essa verificação cobre fast-forward, não cobre o que acontece quando as duas pontas têm um histórico comum que passou por um `git revert` de PR. O mecanismo: um merge de três pontas resolve cada arquivo comparando seu estado na **base comum** (merge-base) contra cada lado; se o arquivo está inalterado num lado desde a base e foi apagado no outro lado desde essa mesma base, git aplica a remoção sem conflito e sem aviso. Um `git revert -m 1` de um merge de PR não remove os commits revertidos da árvore — só neutraliza o diff líquido — então esses commits continuam sendo ancestrais de `main`. Se a branch de domínio herdou, em algum ponto (ex.: por um cherry-pick anterior), um commit que descende de um desses commits revertidos, o merge-base entre as duas pontas pode recuar até antes do revert — e nesse ponto da história, um arquivo que hoje só existe na branch de domínio (ex.: `dados-sensiveis-saude.md`) ainda existia em ambos os lados. Resultado: o merge entende que `main` "apagou" o arquivo desde a base, e replica essa remoção na branch de domínio, mesmo que `main`, no seu estado atual, nunca devesse conter esse arquivo.

Caso real (2026-06-21): exatamente isso apagou `dev-flow-harness/04-guardrails-seguranca/dados-sensiveis-saude.md` de `dominio/saude` durante um `git merge main` considerado seguro antes do merge. Corrigido sem reescrever histórico, via `git checkout <commit-bom> -- <arquivo>` + novo commit (`29ab9c0`) — nunca via `push --force` ou `rebase`.

Verificação concreta antes de confiar em qualquer `merge main` numa branch de domínio que já passou por revert de PR: `git merge-base dominio/<nome> main` para achar a base real (não assumir que é o último ponto em que as branches "se separaram" na intuição), depois `git show <merge-base>:<caminho-do-arquivo-especifico-do-dominio>` para confirmar se o arquivo já existia naquela base. Se existia, revisar o `git status`/`git diff --stat` do merge antes do push, em vez de confiar que "tip não é ancestral" basta.

### O que não promover a uma branch de domínio

Eixos de variação que são detalhe de projeto, não do domínio, continuam em `projetos/<projeto>/context/`, nunca em uma branch: escolha de frontend (web, mobile, desktop), escolha de banco de dados, escolha entre monolito e microsserviços. Sinal para diferenciar: se a variação tem implicação legal/regulatória ou de vocabulário que se repete entre vários projetos do mesmo assunto, é domínio (branch); se é uma escolha técnica de um projeto específico, é adaptador.

### Não confundir com `context/dominio.md` do projeto

`projetos/<projeto>/context/dominio.md` já existe e documenta o domínio de negócio daquele projeto específico (ex.: regras de uma farmácia, de um hospital). A branch `dominio/<nome>` é uma camada acima: guardrails e vocabulário que valem para TODOS os projetos daquele domínio, não só um. Um projeto de saúde, na branch `dominio/saude`, ainda preenche seu próprio `context/dominio.md` com as regras de negócio específicas dele — as duas coisas convivem, em níveis diferentes de generalidade.

## O que este documento não resolve

Formalizar o modelo não substitui testá-lo. Todo critério de aceite, toda regra de promoção descrita acima, ainda não passou pelo único teste que importa: gerar código real, em um projeto real, e verificar se o harness de fato captura o que deveria capturar. Essa etapa foi deliberadamente adiada — ver `dev-flow-harness/01-identidade-memoria/persona.md`, que define como regra "nunca confiar em um único sucesso como prova". O inverso também vale: nenhuma regra deste harness tem ainda sequer um sucesso para se apoiar.

## Ver também

- `dev-flow-harness/README.md` — estrutura interna do trunk e como um projeto o referencia.
- `dev-flow-harness/01-identidade-memoria/glossario.md` — termo "Promoção (de conceito)".
- `dev-flow-harness/05-harness-evals/metricas.md` — métricas de evolução do harness.
- `dev-flow-harness/CHANGELOG.md` — registro histórico de promoções.
