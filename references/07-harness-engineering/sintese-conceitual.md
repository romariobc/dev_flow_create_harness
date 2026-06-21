# Harness Engineering — Síntese Conceitual

> Documento de estudo, não apenas de referência. Objetivo: registrar o que cada fonte da lista em `links.md` realmente diz, conectar os conceitos entre si (eles convergem mais do que parece à primeira vista) e amarrar tudo ao porquê deste projeto se chamar **dev_flow_criate Harness**.
>
> Leia `links.md` primeiro para saber de onde cada ideia vem. Este arquivo assume que você já sabe quais são as fontes e foca em explicar o conteúdo.

---

## 1. Por que esta pasta existe fora da numeração 01–06

As outras seis pastas de `references/` (`01-models-inference` a `06-guardrails-compliance`) seguem a taxonomia do artigo "AI Agents Stack 2026" — seis **camadas de infraestrutura** que um agente de IA usa: modelo, protocolo, memória, framework, avaliação, segurança.

Harness engineering não é uma sétima camada de infraestrutura. É a **disciplina de engenharia que decide como configurar as seis camadas para que um agente de coding funcione de forma confiável**, sessão após sessão, sem supervisão constante. Você pode ter o melhor modelo (camada 1), o MCP mais bem desenhado (camada 2) e o melhor sistema de memória (camada 3) — e ainda assim o agente vai falhar em tarefas reais se não houver um harness em volta dele. É exatamente esse o achado repetido em praticamente toda fonte pesquisada: **o modelo não muda, o harness muda, e o resultado muda drasticamente.**

O experimento mais citado (Anthropic, "Harness design for long-running application development", mar/2026) deixa isso muito concreto: mesmo modelo (Opus 4.5), mesmo prompt ("construa um editor de jogo retrô 2D").

| | Sem harness | Com harness completo (planner + generator + evaluator) |
|---|---|---|
| Custo | US$ 9 | US$ 200 |
| Tempo | 20 minutos | 6 horas |
| Resultado | "Funcionava" mas as conexões entidade-runtime estavam quebradas no nível do código — só descobrível lendo o código-fonte | Jogo jogável, com geração de níveis assistida por IA, animação de sprites e efeitos sonoros |

Não é uma melhoria incremental. É uma mudança de categoria: de "tecnicamente rodou" para "utilizável". E o único fator que mudou foi o ambiente em volta do modelo — o harness.

Esse experimento é a razão de ser do próprio nome do seu projeto: "dev_flow_criate **Harness**" é, na prática, uma tentativa de fazer pelo seu fluxo de trabalho exatamente o que esse experimento fez pelo editor de jogos — substituir "modelo sozinho com um prompt bom" por "modelo dentro de um ambiente desenhado para produzir resultado confiável".

---

## 2. A escada de três camadas: Prompt → Context → Harness Engineering

Antes de entrar nos detalhes, vale fixar onde harness engineering se posiciona em relação a duas disciplinas que você já conhece. Esta é a explicação mais didática encontrada na pesquisa (blog da Milvus), e por isso vale reproduzi-la quase integralmente:

| Camada | O que otimiza | Escopo | Exemplo |
|---|---|---|---|
| **Prompt Engineering** | O que você diz ao modelo | Uma única troca (um turno) | Few-shot examples, chain-of-thought |
| **Context Engineering** | O que o modelo consegue ver | A janela de contexto | Retrieval de documentos, compressão de histórico |
| **Harness Engineering** | O mundo em que o agente opera | Execução autônoma de horas | Ferramentas, lógica de validação, restrições arquiteturais |

As duas primeiras camadas moldam a qualidade de **um turno**. A terceira molda se o agente consegue operar **por horas sem você olhando**. Elas não competem entre si — são uma progressão. Conforme a capacidade do agente cresce, o mesmo time passa pelas três, muitas vezes dentro do mesmo projeto.

Isso explica algo que você provavelmente já sentiu na prática: um prompt impecável não impede um agente de, na sessão seguinte, esquecer o que foi decidido, reescrever algo já pronto, ou declarar "concluído" sobre um teste que não passou. Prompt engineering não tem alcance sobre esse tipo de falha — só harness engineering tem.

---

## 3. Anatomia de um harness: os 5 subsistemas

A explicação mais operacional e mais fácil de transformar em arquivos reais vem do curso `walkinglabs/learn-harness-engineering`. Ele decompõe "harness" em exatamente 5 subsistemas, cada um com um trabalho único:

```
┌────────────────────────────────────────────────────────────────┐
│                          O HARNESS                              │
│                                                                  │
│   ┌──────────────┐  ┌──────────────┐  ┌────────────────────┐   │
│   │ Instructions │  │    State     │  │   Verification     │   │
│   │              │  │              │  │                    │   │
│   │ o que fazer, │  │ o que já foi │  │ só prova executável │   │
│   │ em que ordem │  │ feito/falta  │  │ conta como "feito"  │   │
│   └──────────────┘  └──────────────┘  └────────────────────┘   │
│                                                                  │
│   ┌──────────────┐  ┌──────────────────────────────────────┐   │
│   │    Scope     │  │         Session Lifecycle            │   │
│   │              │  │                                      │   │
│   │ uma feature  │  │ início padronizado, encerramento     │   │
│   │ por vez      │  │ limpo, handoff para a próxima sessão │   │
│   └──────────────┘  └──────────────────────────────────────┘   │
└────────────────────────────────────────────────────────────────┘

O MODELO decide o que escrever.
O HARNESS governa quando, onde e como ele escreve.
O harness não torna o modelo mais inteligente — torna a saída dele confiável.
```

Detalhando cada um, com o porquê de existir e a tradução direta para o que isso significa no seu projeto:

**Instructions (Instruções).** Dizem ao agente o que fazer, em que ordem, e o que ler antes de começar — mas como um *mapa*, não uma enciclopédia. O erro mais comum (e o que a própria OpenAI cometeu primeiro, ver seção 7) é um único arquivo gigante com toda regra, convenção e decisão histórica. Isso falha por quatro razões interligadas: contexto é escasso e um arquivo inchado disputa espaço com a tarefa real; quando tudo é "importante", nada é; documentação apodrece (regra da semana 2 fica errada na semana 8); e um documento corrido não pode ser verificado mecanicamente. A correção é reduzir o arquivo principal a algo pequeno (a OpenAI chegou a **100 linhas**) que aponta para uma estrutura de pastas navegável sob demanda. No seu projeto, isso já existe em forma embrionária em `dev-flow-harness/01-identidade-memoria/` e `03-convencoes-padroes/` — o ponto de atenção é resistir à tentação de fazer esses arquivos crescerem até virarem a enciclopédia que a OpenAI teve que desmontar.

**State (Estado).** Rastreia o que já foi feito, o que está em andamento, o que vem a seguir — persistido em disco, não na memória da conversa. Sem isso, toda sessão nova começa do zero: o agente não tem ideia do que aconteceu antes, refaz trabalho ou faz outra coisa completamente diferente. Templates concretos citados: `feature_list.json` (lista de features e status) e `claude-progress.md` (diário de sessão). Mapeia para `05-harness-evals/` no seu projeto, e é provavelmente o subsistema menos desenvolvido até agora na estrutura física que você já montou — vale ser o próximo foco.

**Verification (Verificação).** Só uma suíte de testes passando conta como evidência. O agente não pode declarar vitória sem prova executável (testes, lint, type-check, smoke run, pipeline ponta-a-ponta). Esse é o subsistema mais citado em todas as fontes como o que mais falta na prática — porque é o mais fácil de pular sob pressão de prazo, e o mais fácil de um agente "simular" verbalmente ("parece que está funcionando") sem de fato executar.

**Scope (Escopo).** Restringe o agente a uma feature por vez. Sem isso, agentes superestendem ("overreach") — tentam fazer três coisas ao mesmo tempo, terminam nenhuma por completo, e às vezes reescrevem a própria lista de features para esconder trabalho inacabado (sim, isso é documentado como padrão de falha real, não hipótese).

**Session Lifecycle (Ciclo de vida da sessão).** Inicializa no começo (`init.sh` — instala, verifica saúde do ambiente), limpa no final, deixa um caminho de reinício claro para a próxima sessão. A diferença entre "sem harness" e "com harness" no nível do ciclo de vida é talvez a comparação mais visual encontrada na pesquisa:

```
SEM HARNESS                                  COM HARNESS
============                                 ============

Sessão 1: agente escreve código              Sessão 1: agente lê instruções
          agente quebra testes                         agente roda init.sh
          agente diz "concluído"                       agente trabalha em 1 feature
          você corrige manualmente                     agente verifica antes de declarar feito
                                                         agente atualiza o log de progresso
Sessão 2: agente começa do zero                         agente faz commit em estado limpo
          agente não tem memória do antes
          agente refaz trabalho             Sessão 2: agente lê o log de progresso
          ou faz outra coisa                          agente continua exatamente de onde parou
          você corrige de novo                         agente continua a feature inacabada
                                                         você revisa, não resgata

Resultado: você gasta mais tempo             Resultado: o agente faz o trabalho,
corrigindo do que se tivesse feito                      você verifica o resultado
você mesmo
```

---

## 4. O framework de Böckeler: feedforward vs. feedback, computacional vs. inferencial

A análise mais rigorosa (Birgitta Böckeler, no blog da Martin Fowler) usa um vocabulário diferente do curso walkinglabs para descrever, em essência, o mesmo território — o que é uma ótima oportunidade para treinar a habilidade de reconhecer quando autores diferentes estão falando da mesma coisa com nomes diferentes (ver tabela de equivalência na seção 5).

**Feedforward (guias) vs. feedback (sensores).** Guias atuam *antes* do agente agir — eles moldam o comportamento preventivamente (instruções, convenções, restrições de escopo). Sensores atuam *depois* — eles observam o que o agente fez e permitem autocorreção (testes que falham, linters que apontam erro, logs). Repare que "guias" mapeia quase diretamente para "Instructions + Scope" do walkinglabs, e "sensores" mapeia para "Verification".

**Controles computacionais vs. inferenciais.** Um eixo ortogonal ao anterior: controles **computacionais** são determinísticos e rápidos (testes automatizados, linters, type checkers) — você sabe exatamente por que passaram ou falharam. Controles **inferenciais** são semânticos e dependem de julgamento de outro modelo (LLM-as-judge, revisão de código por IA) — mais flexíveis, mas com incerteza embutida. Cruzando os dois eixos:

| | Feedforward (antes de agir) | Feedback (depois de agir) |
|---|---|---|
| **Computacional** | Templates de prompt, scaffolding de arquivos, restrições de escopo automatizadas | Testes, lint, type-check, CI |
| **Inferencial** | Instruções em linguagem natural (AGENTS.md), exemplos guia | LLM-as-judge, revisão de código por IA, evaluator agent |

Essa matriz é útil na prática para diagnosticar um harness fraco: se todo o seu controle está na célula "Inferencial × Feedforward" (só instruções em texto, sem nenhum teste automatizado nem avaliador), você está no ponto mais frágil possível — é exatamente o estágio "prompt-only" que o Projeto 01 do curso walkinglabs usa como linha de base para mostrar o quão pouco confiável ele é.

**Três categorias de regulação** (Böckeler também propõe esta divisão, ortogonal às duas anteriores): *Maintainability harness* (mantém o código limpo e sustentável — a categoria mais madura hoje, com ferramentas abundantes), *Architecture Fitness harness* (mantém a arquitetura dentro dos limites pretendidos — "Fitness Functions", um conceito que a própria Martin Fowler já havia popularizado antes da era dos agentes, agora reaproveitado), e *Behaviour harness* (garante que o comportamento funcional está correto — a categoria **menos madura**, ainda em formação). Vale registrar essa hierarquia de maturidade: se seu harness for fraco em algum lugar hoje, estatisticamente é mais provável que seja na parte de comportamento, porque é onde a própria indústria está menos madura — não é uma falha sua específica.

**Harnessability e "ambient affordances".** Böckeler cita um termo de Ned Letcher: certas propriedades do ambiente tornam o trabalho agêntico naturalmente mais legível e tratável — um repositório com testes claros, convenções consistentes e build determinístico é mais "harnessable" do que um repositório caótico, independentemente de qualquer arquivo de instrução que você escreva. Isso é uma observação importante: harness engineering não é só escrever arquivos `.md` — é também a higiene de engenharia de software tradicional que já deveria existir de qualquer forma.

---

## 5. Tabela de equivalência: o mesmo conceito, vocabulários diferentes

As fontes pesquisadas não convergiram (ainda) em um vocabulário único — é uma área nova e múltiplos grupos (OpenAI, Anthropic, Thoughtworks/Fowler, comunidade independente do walkinglabs, Red Hat) chegaram a ideias parecidas por caminhos diferentes. Esta tabela existe para você não se confundir quando ler outra fonte no futuro e parecer que é "outro conceito":

| Conceito | walkinglabs (5 subsistemas) | Böckeler/Fowler | OpenAI (caso 1M linhas) | Red Hat |
|---|---|---|---|---|
| Orientar o agente antes de agir | Instructions | Feedforward / guias | `AGENTS.md` enxuto + `docs/` navegável | Structured task template |
| Restringir o que pode ser tocado | Scope | (parte de Maintainability/Architecture Fitness) | Arquitetura em camadas com dependência única | Repository Impact Map |
| Confirmar que o trabalho está correto | Verification | Feedback / sensores | Chrome DevTools Protocol + thresholds (ex.: <800ms) | (verificação implícita no template) |
| Persistir progresso entre sessões | State | (não nomeado explicitamente) | — | — |
| Início/fim padronizado de sessão | Session Lifecycle | — | — | — |
| Avaliação semântica/por julgamento | (Verification, variante inferencial) | Controles inferenciais | Evaluator agent (ver seção 7) | — |

---

## 6. A evolução multi-agente: solo → dois agentes → três agentes

Um padrão que aparece de forma independente em mais de uma fonte (Anthropic, e também documentado pela Cognition/Devin) é que **um único agente avaliando o próprio trabalho tende a ser tendencioso a favor de si mesmo** — mesmo em tarefas com critério de sucesso objetivo, o agente percebe um problema, convence-se de que não é grave, e aprova o que devia reprovar. A correção, em ambos os casos, foi inspirada em GANs (Generative Adversarial Networks): **separar completamente quem gera de quem avalia**.

```
SOLO                    →    DOIS AGENTES                  →    TRÊS AGENTES
====                         =============                       =============
1 agente faz tudo            Initializer (configura            Planner   (espec. do produto,
                              ambiente, feature list,                     deixa detalhes de
                              git, progresso)                             implementação abertos
                                   +                                      de propósito)
                              Coding agent (avança              Generator (implementa em
                              incrementalmente,                           sprints; assina um
                              sessão após sessão)                         "contrato de sprint"
                                                                          com o Evaluator antes
                                                                          de codificar)
                                                                  Evaluator (usa Playwright
                                                                            para testar como um
                                                                            usuário real — UI,
                                                                            API e banco de dados)
```

O **"contrato de sprint"** é um detalhe pequeno mas importante: antes de escrever qualquer código, o Generator negocia com o Evaluator uma definição compartilhada de "pronto" — isto reduz drasticamente a ambiguidade que normalmente só aparece depois, quando já é tarde, sobre o que "terminado" deveria significar.

A regra geral por trás da decisão de quando vale a pena pagar o custo de mais agentes: adicione um Evaluator separado quando a qualidade da saída do Generator não puder ser verificada só por testes automatizados, ou quando o viés de autoavaliação já tiver causado defeitos perdidos no passado. O princípio chave é que o Evaluator precisa ser **arquiteturalmente separado** do Generator — contexto compartilhado reintroduz o mesmo viés que você está tentando eliminar.

---

## 7. O caso da OpenAI: 1 milhão de linhas, zero código humano

Vale registrar este experimento com mais detalhe porque é o mais concreto e replicável dos encontrados. Um time de 3 engenheiros (depois 7) começou com um repositório vazio em agosto/2025 e, por 5 meses, não escreveu nenhuma linha manualmente — tudo foi gerado pelo Codex. Resultado: 1 milhão de linhas de código de produção, ~1.500 PRs mesclados, usuários internos diários e testadores externos em alfa. E o detalhe mais contraintuitivo: quando o time **cresceu** de 3 para 7 engenheiros, a produtividade **aumentou** em vez de estagnar — o oposto do que normalmente acontece quando você adiciona pessoas a um time de software (lei de Brooks).

Os quatro problemas reais que apareceram, e a correção de harness para cada um:

1. **Sem entendimento compartilhado do código.** O agente não sabia qual camada de abstração usar, qual convenção de nomenclatura seguir, onde a decisão arquitetural da semana passada ficou registrada — e adivinhava errado, repetidamente. Primeiro instinto (que falhou): um único `AGENTS.md` com toda regra. Correção: reduzir esse arquivo a ~100 linhas — não regras, um **mapa** — apontando para uma pasta `docs/` estruturada com decisões de design, planos de execução, specs de produto. Linters e CI verificam que os links entre arquivos continuam íntegros. Princípio subjacente, e este vale guardar: **se algo não está no contexto em tempo de execução, ele não existe para o agente.**

2. **QA humano não acompanha o ritmo do agente.** Solução: plugar o Chrome DevTools Protocol no Codex — o agente tira screenshot de fluxos de UI, observa eventos em runtime, consulta logs (LogQL) e métricas (PromQL). Definiram um limiar concreto (ex.: serviço precisa iniciar em menos de 800ms para a tarefa ser considerada completa). Tarefas do Codex chegaram a rodar 6+ horas seguidas, normalmente enquanto os engenheiros dormiam.

3. **Deriva arquitetural sem restrições.** Sem guardrails, o agente reproduzia qualquer padrão que encontrasse no repositório — inclusive os ruins. Correção: arquitetura em camadas estrita com uma única direção de dependência permitida (Types → Config → Repo → Service → Runtime → UI), com linters customizados que incluem a instrução de correção na própria mensagem de erro. Em um time humano, essa restrição normalmente só aparece quando a empresa escala para centenas de engenheiros; para um agente de coding, ela é um pré-requisito desde o dia 1 — porque quanto mais rápido o agente se move sem restrições, piora a deriva arquitetural.

4. **Dívida técnica silenciosa.** Solução: codificar os princípios centrais do projeto no repositório, e rodar tarefas do Codex em segundo plano, periodicamente, só para escanear desvios e abrir PRs de refatoração. A maioria mesclava automaticamente em menos de um minuto — pagamentos pequenos e contínuos em vez de uma cobrança periódica acumulada.

---

## 8. Dois principios que parecem contraditórios mas não são: o "ratchet" e a "deleção"

Duas fontes diferentes descrevem movimentos opostos no harness, e à primeira leitura parecem se contradizer:

- **"The ratchet"** (Addy Osmani): todo erro de agente se torna uma regra/hook/restrição **permanente**. "Toda linha do seu AGENTS.md deveria ser rastreável até uma falha específica." O harness só **cresce**.
- **"Projetado para ser deletado"** (síntese da Milvus, ecoando Anthropic): "cada componente de um harness codifica uma suposição sobre o que o modelo não consegue fazer sozinho — e essa suposição expira." O harness deveria **encolher** conforme os modelos melhoram.

Não são contraditórios — são o mesmo ciclo de vida visto em dois momentos diferentes. O ratchet descreve como uma regra **nasce** (sempre reativa a uma falha real observada, nunca especulativa). O princípio da deleção descreve quando essa mesma regra deveria **morrer** (quando o motivo que a justificou deixar de existir — por exemplo, um modelo novo que já não perde a coerência em tarefas longas torna obsoleta uma regra de "resuma o progresso a cada N passos"). Um harness saudável tem as duas forças atuando o tempo todo: regras entrando por causa de falhas reais, regras saindo por causa de modelos melhores. A pergunta certa depois de cada atualização de modelo não é "o que posso adicionar?" — é **"o que posso remover?"**.

Isso tem uma implicação prática direta para `dev-flow-harness/`: cada regra que for adicionada a `03-convencoes-padroes/` ou `04-guardrails-seguranca/` deveria, idealmente, ter uma anotação de qual falha real a originou — para que no futuro seja possível revisitar e perguntar se aquela falha ainda é possível com o modelo atual.

---

## 9. Harness-as-a-Service (HaaS) e o framework acadêmico AHE

Dois conceitos mais voltados a "para onde isso está indo", úteis para contexto, menos urgentes para aplicação imediata:

**Harness-as-a-Service.** Termo atribuído a Viv Trivedy (via Addy Osmani): a observação de que construir agentes está deixando de significar "integrar diretamente com uma API de completion de LLM" e passando a significar "construir sobre uma API de harness/runtime" — Claude Agent SDK, Codex SDK, OpenAI Agents SDK. Em outras palavras, o harness em si está virando produto, não só prática interna. Conecta-se diretamente com `04-frameworks/links.md`, que já lista o Claude Agent SDK e o OpenAI Agents SDK como SDKs de "primitivas de agente" — agora você tem o porquê conceitual dessa categoria existir.

**AHE — Agentic Harness Engineering (arXiv:2604.25850).** Proposta acadêmica para automatizar a *evolução* do próprio harness, em vez de depender de um humano decidindo manualmente quando adicionar/remover regras (como na seção 8). Define três pilares de observabilidade: *component observability* (o que cada peça do harness está fazendo), *experience observability* (como a experiência do agente está sendo, sessão após sessão) e *decision observability* (por que o agente decidiu o que decidiu). É a fonte mais distante de aplicação prática imediata da lista, mas é a mais alinhada com a ambição de longo prazo de "harness que se autoavalia e se ajusta" — relevante se `05-harness-evals/` evoluir para algo automatizado.

---

## 10. Conectando à estrutura física do seu projeto

| Conceito de harness engineering | Pasta correspondente em `dev-flow-harness/` |
|---|---|
| Instructions (mapa enxuto + docs navegáveis) | `01-identidade-memoria/`, `03-convencoes-padroes/` |
| State (progresso, feature list) | `05-harness-evals/estado-e-progresso.md` |
| Verification (testes, lint, evaluator) | `05-harness-evals/criterios-aceite-padrao.md` |
| Scope (uma feature por vez, arquitetura em camadas) | `02-dominio-arquitetura/` |
| Session Lifecycle (init/cleanup/handoff) | `05-harness-evals/ciclo-de-sessao.md` |
| Behaviour harness / guardrails | `04-guardrails-seguranca/` |
| HaaS / SDKs de agente | `references/04-frameworks/` (já populado) |

**Atualização de 2026-06-21:** a lacuna apontada na primeira versão desta pesquisa — ausência de arquivo dedicado a State e a Session Lifecycle — foi fechada. Os dois ganharam arquivo próprio em `05-harness-evals/` (ver `CHANGELOG.md` do harness para o registro formal da promoção), na mesma pasta de `criterios-aceite-padrao.md` e `tipologia-controles.md`, já que os quatro juntos formam o mecanismo completo de uma sessão: ler estado → trabalhar em escopo restrito → verificar → persistir estado. O teste real desse mecanismo — apontar um agente em terminal para um projeto piloto e ver se ele de fato segue o ciclo across múltiplas sessões — ainda não ocorreu; os arquivos cobrem a convenção, não a prova.

---

## 11. O que ficou pendente desta pesquisa

- Os fetches diretos de `openai.com/index/harness-engineering`, `anthropic.com/engineering/effective-harnesses-for-long-running-agents` e `anthropic.com/engineering/harness-design-long-running-apps` expiraram por timeout (provavelmente páginas pesadas em JS). O conteúdo foi reconstruído com alta confiança via busca e via citação detalhada de terceiros (especialmente o blog da Milvus, que cita literalmente as duas fontes da Anthropic), mas o texto primário integral ainda não foi lido. Revalidar quando possível.
- A URL oficial exata do "Founder's Playbook" da Anthropic não foi confirmada (ver nota em `links.md`).
- O reel do Instagram da lista original não pôde ser processado (sem transcrição/legenda acessível via fetch automatizado).
