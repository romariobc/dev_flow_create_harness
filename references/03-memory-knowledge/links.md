# links.md — Memory & Knowledge

Fontes sobre bancos vetoriais, RAG, memória de agente e knowledge graphs. Substanciam decisões de arquitetura de memória/contexto em `02-dominio-arquitetura/` e o uso de RAG como alternativa a fine-tuning.

## Bancos vetoriais e busca semântica

- [pgvector — GitHub](https://github.com/pgvector/pgvector) — extensão de similaridade vetorial para PostgreSQL; opção preferida quando o projeto já usa Postgres e não justifica um banco vetorial dedicado (reduz superfície de infraestrutura).
- [Pinecone — Docs](https://docs.pinecone.io/guides/get-started/overview) — banco vetorial totalmente gerenciado; referência para quando escala/operação justificam um serviço dedicado em vez de pgvector.
- [Neo4j GraphRAG (Python) — Docs](https://neo4j.com/docs/neo4j-graphrag-python/current/) — pacote oficial Neo4j para RAG sobre knowledge graphs; relevante quando a relação entre entidades (não só similaridade textual) é parte do domínio.

## Técnicas de RAG

- [Anthropic — Contextual Retrieval](https://www.anthropic.com/engineering/contextual-retrieval) — técnica (Contextual Embeddings + Contextual BM25) que reduz falhas de retrieval em RAG; referência de melhor prática para chunking/indexação antes de qualquer implementação de RAG no harness.
- [Jason Liu — Context Engineering Series (índice)](https://jxnl.co/writing/2025/08/28/context-engineering-index/) — série prática sobre como desenhar respostas de ferramentas e fluxo de informação para agentes que exploram dados (não só recuperam); complementa a Contextual Retrieval com foco em agentic RAG.

## Memória de agente (longo prazo, multi-sessão)

- [MemGPT — "Towards LLMs as Operating Systems" (arXiv:2310.08560)](https://arxiv.org/abs/2310.08560) — paper original que introduz gestão hierárquica de memória (memória "em contexto" vs. "fora de contexto") para superar o limite de janela de contexto; base conceitual de Letta e de qualquer estratégia de memória paginada.
- [Letta (sucessor do MemGPT) — Docs](https://docs.letta.com) — implementação oficial/original da arquitetura MemGPT, com agentes "stateful" e ferramentas de auto-edição de memória.
- [Mem0 — GitHub](https://github.com/mem0ai/mem0) — camada de memória universal para agentes (memória de usuário/sessão/agente) com APIs simples de add/search; alternativa mais leve a Letta para personalização.
- [Zep — Docs](https://help.getzep.com/overview) e [paper "Zep: A Temporal Knowledge Graph Architecture for Agent Memory" (arXiv:2501.13956)](https://arxiv.org/abs/2501.13956) — memória de agente baseada em knowledge graph temporal (Graphiti); relevante quando o histórico de mudanças ao longo do tempo é importante, não só o estado atual.

## Nota de manutenção

Memória de agente é a área mais ativa de pesquisa/produtos entre as 6 camadas (novos entrantes além de Mem0/Letta/Zep aparecem com frequência). Revalidar comparativos antes de adotar uma solução em produção.
