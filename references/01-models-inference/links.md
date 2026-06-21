# links.md — Models & Inference

Fontes sobre provedores de modelo (cloud e local), gateways de inferência, benchmarks de qualidade e pesquisa de modelos de raciocínio. Substanciam as decisões em `dev-flow-harness/02-dominio-arquitetura/stacks-de-referencia/` sobre quando usar modelo local (Ollama) vs. nuvem, e como avaliar/escolher modelos.

## Documentação oficial de provedores

- [Anthropic — Claude Docs](https://docs.anthropic.com) — documentação oficial da API Claude, usada como referência primária do harness (Anthropic é o provedor padrão assumido nos exemplos).
- [OpenAI — API Platform Docs](https://platform.openai.com/docs) — documentação oficial da API OpenAI, referência para comparação de capacidades/preços e para projetos que integrem múltiplos provedores.
- [Google AI for Developers — Gemini API Docs](https://ai.google.dev/docs) — documentação oficial do Gemini, terceiro provedor de referência para estratégias multi-modelo.
- [Ollama — GitHub](https://github.com/ollama/ollama) — execução local de LLMs; base da nossa estratégia "modelo local para dado sensível" descrita em `04-guardrails-seguranca/dados-sensiveis-por-dominio.md`.

## Gateways e abstração multi-provedor

- [LiteLLM — Docs](https://docs.litellm.ai/) — SDK/proxy que unifica 100+ APIs de LLM no formato OpenAI; relevante para desacoplar o harness de um único provedor e permitir trocar Ollama↔nuvem sem reescrever código.
- [Hugging Face — Inference Endpoints Docs](https://huggingface.co/docs/inference-endpoints/index) — deploy gerenciado de modelos open-weight (incluindo modelos de saúde/domínio específico) quando Ollama local não for suficiente em escala.

## Pesquisa e benchmarks

- [DeepSeek-R1 — "Incentivizing Reasoning Capability in LLMs via Reinforcement Learning" (arXiv:2501.12948)](https://arxiv.org/abs/2501.12948) — paper que popularizou modelos de raciocínio treinados via RL pura; referência para entender a classe de modelos "reasoning" hoje disponível via Ollama/HF.
- [Chatbot Arena (arena.ai)](https://arena.ai/) — benchmark de preferência humana (votação às cegas, ranking Elo) entre LLMs; usado como sinal complementar aos benchmarks acadêmicos ao escolher modelo para uma tarefa. Nota: a plataforma foi renomeada de "LMSYS Chatbot Arena" para **arena.ai** — revalidar a URL periodicamente.
- [Simon Willison's Weblog](https://simonwillison.net/) — análises práticas e atualizadas de ferramentas, modelos e padrões de engenharia agentic; boa fonte para "o que está funcionando na prática" além da documentação oficial.

## Nota de manutenção

URLs de provedores cloud e de benchmarks mudam com frequência (ex.: rebrands, reestruturação de docs). Revalidar este arquivo a cada novo projeto que dependa fortemente de um provedor específico.
