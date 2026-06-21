# Glossário do harness

Termos usados de forma consistente em todo o harness e em todos os projetos. Mantenha este arquivo curto e vivo — adicione um termo quando ele for usado de forma ambígua mais de uma vez.

**Context engineering (engenharia de contexto)** — disciplina de projetar deliberadamente o que um modelo de IA enxerga antes de executar uma tarefa: domínio, convenções, guardrails e critérios de avaliação. Diferente de prompt engineering, que lida com a instrução pontual de uma tarefa específica.

**Harness** — conjunto de critérios, testes e mecanismos que avaliam objetivamente se um resultado gerado por IA é aceitável, antes de ser integrado ao projeto.

**Eval (avaliação)** — execução concreta de um critério do harness contra uma saída real (ex: rodar um conjunto de testes, ou comparar uma resposta gerada contra um gabarito).

**Guardrail** — restrição explícita que impede um comportamento indesejado do agente (ex: nunca expor uma chave de API, nunca processar CPF sem anonimização).

**ADR (Architecture Decision Record)** — registro formal de uma decisão de arquitetura: o problema, as opções consideradas, a decisão tomada e as consequências.

**Adaptador (de projeto)** — pasta de contexto específica de um projeto, que referencia o harness global e documenta apenas as divergências e regras locais.

**RAG (Retrieval-Augmented Generation)** — padrão de arquitetura onde o modelo busca informação relevante em uma base externa antes de gerar a resposta, em vez de depender só do que foi treinado nele.

**Agente** — sistema que usa um LLM para decidir uma sequência de ações (chamadas de ferramenta, consultas, geração de texto) em vez de gerar uma única resposta direta. LangGraph é o framework de referência deste harness para orquestrar agentes com estado.

**Tracing / observabilidade** — capacidade de inspecionar passo a passo o que um agente fez (prompts enviados, ferramentas chamadas, respostas recebidas). LangSmith é a ferramenta de referência deste harness para isso.

**Controle computacional** — critério de aceite verificado por código determinístico: mesmo input, mesmo veredito, sempre auditável (lint, teste unitário, validação de schema). Roda em pre-commit/CI inicial. Ver `05-harness-evals/tipologia-controles.md`.

**Controle inferencial** — critério de aceite verificado por julgamento semântico (LLM-as-judge ou humano): pode variar entre execuções porque depende de interpretação. Roda em CI pós-integração ou revisão assíncrona, nunca como gate único para algo crítico. Ver `05-harness-evals/tipologia-controles.md`.

**Harnessability** — o quão fácil uma stack, linguagem ou framework é de guiar com IA, antes de qualquer guardrail ser escrito (tipagem forte, convenções impostas, testabilidade). Critério a registrar em ADRs de escolha de stack — ver `02-dominio-arquitetura/adrs/0000-template-adr.md`.

**Promoção (de conceito)** — ato de mover uma ideia de `references/` (teoria) para `dev-flow-harness/` (regra ativa), ou de um adaptador de projeto para o harness global. Só ocorre quando a ideia passa pelos critérios de recorrência, aplicabilidade ao stack, custo de adoção, não-duplicação e checabilidade — ver `ARQUITETURA.md` na raiz do workspace e o registro em `dev-flow-harness/CHANGELOG.md`.

**State (estado)** — subsistema do harness responsável por persistir, em disco, o que já foi feito, o que está em andamento e o que vem a seguir num projeto — para que uma sessão nova não comece do zero. Materializado em `feature_list.json` e `progresso.md`, por projeto. Ver `05-harness-evals/estado-e-progresso.md`.

**Session Lifecycle (ciclo de sessão)** — subsistema do harness que define o início e o fim padronizados de uma sessão de trabalho de um agente: o que ler antes de codificar, e o que escrever/verificar antes de encerrar, garantindo handoff limpo para a sessão seguinte. Ver `05-harness-evals/ciclo-de-sessao.md`.
