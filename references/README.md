# references/

Biblioteca de fontes externas (documentação oficial, papers, artigos, ebooks) que substanciam as decisões técnicas do `dev-flow-harness/` e dos projetos em `projetos/`. Enquanto o harness diz **o que fazer**, esta pasta guarda **por que isso é considerado melhor prática hoje**, com a fonte original para consulta e atualização.

## Organização

Estrutura alinhada às 6 camadas do stack de agentes de IA (mesma taxonomia usada no artefato "AI Agents Stack 2026"):

| Pasta | Camada | Cobre |
|---|---|---|
| `01-models-inference/` | Models & Inference | Provedores de modelo, serving local/nuvem, benchmarks |
| `02-protocols-tools/` | Protocols & Tools | MCP, tool calling, segurança de protocolo, comunicação entre agentes |
| `03-memory-knowledge/` | Memory & Knowledge | Bancos vetoriais, RAG, memória de agente, knowledge graphs |
| `04-frameworks/` | Frameworks | LangChain/LangGraph e alternativas de orquestração de agentes |
| `05-eval-observability/` | Eval & Observability | Avaliação, tracing, observabilidade em produção |
| `06-guardrails-compliance/` | Guardrails & Compliance | Segurança de LLM, normas e regulação (incl. LGPD/ANVISA) |

Cada uma dessas pastas tem um `links.md`: lista curada com título, URL e uma linha explicando por que a fonte é relevante/o que ela substancia no harness.

### Camada transversal (não posicional)

| Pasta | Camada | Cobre |
|---|---|---|
| `07-harness-engineering/` | Harness Engineering (transversal) | A disciplina de projetar o ambiente de execução de agentes de coding — não substitui nenhuma das 6 camadas acima, mas é a engenharia que organiza como todas elas são configuradas na prática. Fonte conceitual direta do nome do projeto (`dev_flow_criate Harness`). |

Esta pasta tem dois arquivos, não só um: `links.md` (fontes) e `sintese-conceitual.md` (síntese didática conectando os conceitos entre si e à estrutura de `dev-flow-harness/`) — a profundidade do tema justificou um documento de estudo além da lista de links.

- `ebooks/` — arquivos longos (PDFs, e-books) baixados para consulta offline. Coloque o arquivo aqui e referencie-o pelo nome no `links.md` da camada correspondente.
- `artigos/` — artigos individuais salvos localmente (PDF/HTML/Markdown) quando o conteúdo é importante o suficiente para não depender só do link externo.

## Regra de manutenção

Toda vez que uma decisão do harness (`dev-flow-harness/`) for baseada numa fonte externa, essa fonte deve estar registrada aqui — não apenas citada de memória. Links que ficarem fora do ar ou desatualizados devem ser substituídos, nunca deixados quebrados silenciosamente.
