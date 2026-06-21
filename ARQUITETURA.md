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

Hoje a decisão é humana (Romário, com ou sem apoio do tutor de IA), registrada em `dev-flow-harness/CHANGELOG.md` — cada promoção vira uma linha: data, conceito, origem em `references/`, destino no harness. Sem esse registro, a auditabilidade que o próprio harness exige de guardrails de dados sensíveis (`04-guardrails-seguranca/dados-sensiveis-saude.md`) não estaria sendo aplicada ao harness sobre si mesmo.

## O que este documento não resolve

Formalizar o modelo não substitui testá-lo. Todo critério de aceite, toda regra de promoção descrita acima, ainda não passou pelo único teste que importa: gerar código real, em um projeto real, e verificar se o harness de fato captura o que deveria capturar. Essa etapa foi deliberadamente adiada — ver `dev-flow-harness/01-identidade-memoria/persona.md`, que define como regra "nunca confiar em um único sucesso como prova". O inverso também vale: nenhuma regra deste harness tem ainda sequer um sucesso para se apoiar.

## Ver também

- `dev-flow-harness/README.md` — estrutura interna do trunk e como um projeto o referencia.
- `dev-flow-harness/01-identidade-memoria/glossario.md` — termo "Promoção (de conceito)".
- `dev-flow-harness/05-harness-evals/metricas.md` — métricas de evolução do harness.
- `dev-flow-harness/CHANGELOG.md` — registro histórico de promoções.
