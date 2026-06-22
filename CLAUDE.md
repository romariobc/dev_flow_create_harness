# dev_flow_create_harness — CLAUDE.md

Este é o ponto de entrada para qualquer sessão Claude Code ou Cowork neste repositório.

## O que é este repo

Harness de engenharia de contexto para desenvolvimento assistido por IA. Não é um app — é o scaffold que qualquer projeto futuro herda. Três camadas:

| Camada | Pasta | Papel |
|---|---|---|
| Teoria | `references/` | Pesquisa externa; sem compromisso de uso |
| Trunk ativo | `dev-flow-harness/` | Regras operacionais reutilizáveis por qualquer projeto |
| Instâncias | `projetos/<projeto>/` | Código real + adaptador de domínio |

## Ao iniciar qualquer sessão — leia nesta ordem

1. `dev-flow-harness/CLAUDE.md` — carrega o contexto completo do harness (persona, stack, precedência de regras).
2. `dev-flow-harness/05-harness-evals/ciclo-de-sessao.md` — as 4 etapas obrigatórias (inicializa → trabalha → verifica → persiste).
3. Se estiver dentro de um projeto: `projetos/<projeto>/CLAUDE.md` e `projetos/<projeto>/context/estado/progresso.md`.

## Stack declarado

Python · LangChain · LangGraph · LangSmith · Hugging Face · Ollama

## Branches ativas

- `main` — trunk agnóstico (este arquivo e `dev-flow-harness/`)
- `dominio/saude` — adiciona guardrails de saúde; projetos RAG e assistente de prescrição vivem aqui

## Próximo projeto

`projetos/rag_pcdt_arvs/` na branch `dominio/saude` — RAG sobre PCDTs de ARVs. Ainda não criado; é o primeiro teste de ponta a ponta do harness.

## Documentos-chave

- `ARQUITETURA.md` — modelo de três camadas e regras de promoção teoria → harness
- `dev-flow-harness/CHANGELOG.md` — registro histórico de promoções
- `dev-flow-harness/02-dominio-arquitetura/adrs/` — decisões de arquitetura registradas
