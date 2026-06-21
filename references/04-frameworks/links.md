# links.md — Frameworks

Fontes sobre LangChain/LangGraph (framework de orquestração padrão do harness, ver `02-dominio-arquitetura/stacks-de-referencia/python-langchain-langgraph.md`) e alternativas de orquestração de agentes, usadas para comparação e justificativa de escolha.

## LangChain / LangGraph (padrão do harness)

- [LangChain & LangGraph — Docs unificados](https://docs.langchain.com) — documentação oficial atual (LangChain migrou para um hub único de docs); referência primária para qualquer código de agente/chain no harness.
- [LangChain — "State of Agent Engineering 2025"](https://www.langchain.com/state-of-agent-engineering) — pesquisa com a comunidade (1.340 respostas) sobre adoção de agentes em produção, principais barreiras (qualidade, segurança, latência) e padrões de observabilidade; substancia decisões de priorização do harness (ex.: por que eval/observability é tratado como camada própria).

## SDKs de agente dos grandes provedores

- [Claude Agent SDK (Python) — GitHub](https://github.com/anthropics/claude-agent-sdk-python) e [Anthropic Engineering — "Building agents with the Claude Agent SDK"](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk) — SDK oficial da Anthropic para agentes além de coding; referência quando o projeto quiser as mesmas primitivas do Claude Code/Cowork.
- [OpenAI Agents SDK (Python) — Docs](https://openai.github.io/openai-agents-python/) — sucessor de produção do Swarm; primitivas de agents, handoffs e guardrails nativos do lado OpenAI.
- [Google Agent Development Kit (ADK) — Docs](https://google.github.io/adk-docs/) — framework open-source code-first do Google para construir/avaliar/deployar agentes; relevante para integrações com Gemini/Vertex.

## Frameworks open-source alternativos

- [smolagents (Hugging Face) — Docs](https://huggingface.co/docs/smolagents/index) — biblioteca minimalista onde o agente escreve ações em código (CodeAgent); boa referência de simplicidade para comparar com a complexidade do LangGraph.
- [CrewAI — Docs](https://docs.crewai.com/) — framework de agentes multi-papel ("crews") com memória, conhecimento e guardrails embutidos; alternativa popular ao LangGraph para times que preferem abstração por papéis.
- [Microsoft Semantic Kernel — Docs](https://learn.microsoft.com/en-us/semantic-kernel/) — SDK leve para integrar LLMs a código existente (C#/Python/Java); nota: Microsoft está migrando o suporte de longo prazo para o **Microsoft Agent Framework**, sucessor do Semantic Kernel e do AutoGen — verificar status antes de iniciar projeto novo.
- [Microsoft AutoGen — Docs](https://microsoft.github.io/autogen/) — framework de agentes conversacionais multi-agente; nota: está em **modo de manutenção**, com Microsoft Agent Framework recomendado para novos projetos.

## Nota de manutenção

Frameworks de agente têm ciclo de consolidação rápido (Semantic Kernel + AutoGen → Microsoft Agent Framework é um exemplo recente). Antes de adotar um framework para um novo projeto, confirmar se ele ainda é a recomendação ativa do fornecedor.
