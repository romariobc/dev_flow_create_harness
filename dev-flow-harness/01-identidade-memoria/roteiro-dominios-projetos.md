# Roteiro de domínios e projetos

## Por que este arquivo existe

`01-identidade-memoria/` já guarda quem o agente deve ser (`persona.md`) e como ele deve nomear as coisas (`glossario.md`). Faltava guardar **em que ordem** o Romário pretende usar o harness — porque essa ordem não cabe em nenhum `progresso.md` de projeto (não existe projeto ainda) nem em nenhum ADR (não é uma decisão de arquitetura, é uma sequência de intenção). Sem este arquivo, a informação só existiria na conversa, e se perderia na primeira sessão nova ou na primeira compactação de contexto — exatamente o problema que `05-harness-evals/ciclo-de-sessao.md` resolve para o progresso dentro de um projeto. Este arquivo faz o mesmo papel, um nível acima: progresso entre projetos e domínios.

## O roteiro

1. **`dominio/saude` → `rag_pcdt_arvs`** — primeiro projeto real do harness. Um RAG sobre PCDT (Protocolos Clínicos e Diretrizes Terapêuticas) ligados a ARVs (antirretrovirais). É o primeiro teste de ponta a ponta de tudo que está escrito em `dev-flow-harness/`: a primeira vez que uma regra de aceite, um guardrail ou uma convenção deste harness vai encontrar código e dados reais, em vez de só a descrição de como deveriam funcionar. `ARQUITETURA.md` já registra essa lacuna explicitamente, na seção final: "nenhuma regra deste harness tem ainda sequer um sucesso para se apoiar". Este projeto é o que começa a fechar essa lacuna.
2. **`dominio/saude` → assistente de análise de prescrição** — segundo projeto, ainda dentro do mesmo domínio. Reaproveita o guardrail de dados sensíveis de saúde (`04-guardrails-seguranca/dados-sensiveis-saude.md`) e qualquer lição que `rag_pcdt_arvs` já tiver promovido de `context/adaptacoes.md` para o harness global.
3. **Expansão para outros domínios** — vendas, marketing e outros que surgirem depois. Cada um se torna uma nova branch `dominio/<nome>`, seguindo exatamente o mecanismo do ADR-0001 (`02-dominio-arquitetura/adrs/0001-branches-de-dominio-para-variacao-do-harness.md`): parte de `main`, herda 100% do trunk agnóstico, adiciona só o que é específico daquele domínio.

## O papel constante do harness, em qualquer fase

Em todas as três etapas acima, `dev_flow_criate` é o pontapé inicial padrão: a forma como qualquer LLM passa a codificar sob supervisão humana, com o estado do trabalho persistido entre sessões (`05-harness-evals/estado-e-progresso.md`) e a documentação evoluindo automaticamente a cada decisão relevante (ciclo de sessão + ADR + `CHANGELOG.md`). O que muda de uma etapa para a outra é só o conteúdo de `dominio/<nome>` e o `context/` de cada projeto — a disciplina por baixo é a mesma.

## Onde cada peça mora fisicamente

- `dominio/saude` — branch já criada e publicada; tem hoje o guardrail de saúde e, a partir de agora, este roteiro (herdado de `main`).
- `projetos/rag_pcdt_arvs/` — será criado dentro da branch `dominio/saude` quando o projeto começar a sair do papel; ainda não existe. Hoje só `projetos/_template-adapter/` existe, como molde vazio.
- o projeto de análise de prescrição — mesma branch, mesmo padrão, depois do anterior.
- `dominio/vendas`, `dominio/marketing` etc. — só quando esses domínios entrarem em pauta de fato. Não antecipar a criação: uma branch de domínio vazia não tem valor até ter conteúdo real, e cada branch de longa duração já tem custo de manutenção próprio (ver `ARQUITETURA.md`, seção "O custo, e como mitigar").

## Status nesta data

Em 2026-06-21: `main` e `dominio/saude` publicados e sincronizados com seus remotos, ambos contendo o ADR-0001 (`main` sem o guardrail de saúde, `dominio/saude` com ele — exatamente como a regra do ADR-0001 prescreve). Nenhum projeto foi criado ainda. Este arquivo registra a ordem pretendida, não progresso real — progresso real só passa a existir quando `rag_pcdt_arvs` ganhar seu próprio `context/estado/feature_list.json`.

## Ver também

- `ARQUITETURA.md`, seção "Variação por domínio: branches"
- `02-dominio-arquitetura/adrs/0001-branches-de-dominio-para-variacao-do-harness.md`
- `01-identidade-memoria/glossario.md`, termo "Branch de domínio"
- `01-identidade-memoria/persona.md`
