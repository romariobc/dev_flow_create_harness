# Tipologia de controles: computacional × inferencial

Todo critério de `criterios-aceite-padrao.md` (e qualquer novo critério que vier a ser escrito) tem uma de duas naturezas. Saber qual é decide em que estágio do pipeline ele roda e quanto pode custar — tratar os dois tipos como equivalentes é o erro mais comum ao desenhar avaliação para sistemas com LLM no meio.

## Controle computacional

Executado por código fixo: o mesmo input sempre produz o mesmo veredito, e é sempre possível auditar exatamente por que passou ou falhou. Não há ambiguidade nem grau — passou ou não passou.

Exemplos neste stack:

- Lint e formatação (Ruff, Black) — ver `03-convencoes-padroes/estilo-python.md`.
- Testes unitários (pytest) sobre código determinístico.
- Validação de schema/contrato — ex.: o payload que uma tool LangChain recebe bate com o schema Pydantic esperado.
- Testes de arquitetura — ex.: garantir que `agentes/` nunca importa direto de `integracoes/` sem passar por `chains/` (ver `03-convencoes-padroes/estrutura-de-repositorio.md`).
- Limite de iterações de um grafo LangGraph (ver `o-que-nunca-fazer.md`, item sobre loop sem cap).

**Onde roda:** pre-commit ou no primeiro estágio do CI. É barato e rápido — deve capturar o erro o mais cedo possível no ciclo (princípio de "Keep Quality Left").

## Controle inferencial

Avaliado por julgamento — outro modelo (LLM-as-judge) ou um humano decidindo com critério em linguagem natural, não com uma regra fixa. O mesmo input pode, em tese, receber vereditos levemente diferentes em execuções distintas, porque a própria avaliação depende de interpretação.

Exemplos neste stack:

- LLM-as-judge avaliando se uma resposta de RAG é fiel à fonte (ex.: a resposta sobre um PCDT não contradiz o documento de origem), não apenas se tem o formato certo.
- Revisão semântica de código gerado por agente, antes de aceitar um PR.
- Avaliação de coerência, tom ou completude de uma resposta longa.
- Revisão humana de casos no dataset de eval (ver `criterios-aceite-padrao.md`, critério 3).

**Onde roda:** CI pós-integração, em lote periódico, ou como revisão humana assíncrona — nunca como gate único em pre-commit, porque custo e latência inviabilizam feedback rápido, e porque um único veredito de "juiz" não deveria travar sozinho algo crítico.

## Regra de decisão

Antes de transformar uma ideia em critério de aceite, pergunte: *o mesmo input sempre produz o mesmo resultado, e eu sei explicar exatamente por quê?* Se sim — é computacional, pode e deve rodar em todo commit. Se a resposta depende de nuance, contexto ou julgamento de qualidade — é inferencial: roda com menor frequência, custa mais, e nunca deve ser o único critério de aceite para algo crítico (saúde, segurança, dado sensível).

## Por que isso importa especialmente com LangGraph/Ollama

Um agente com loop de estado (LangGraph) tende a acumular controles inferenciais porque "parece mais inteligente" julgar tudo semanticamente. Isso é caro, lento e, em modelo local via Ollama, pode virar o gargalo do pipeline inteiro. A maioria dos erros reais que aparecem na prática — schema inválido, tool chamada com parâmetro errado, grafo sem condição de parada — é capturável por controle computacional, com custo quase zero. Reserve o controle inferencial para o que o computacional genuinamente não alcança: fidelidade factual, tom, aderência a uma regra de negócio implícita que não se reduz a uma checagem estrutural.

## Origem e referência cruzada

Distinção descrita originalmente por Böckeler/Fowler sobre harness engineering — ver `references/07-harness-engineering/sintese-conceitual.md` para o relato teórico completo. Este arquivo existe para que a distinção pare de ser apenas teoria lida e passe a ser regra ativa na hora de escrever ou revisar um critério de aceite.
