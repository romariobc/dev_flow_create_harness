# links.md — Eval & Observability

Fontes sobre avaliação de LLMs/agentes e observabilidade em produção. Substanciam `05-harness-evals/criterios-aceite-padrao.md` e `05-harness-evals/metricas.md`.

## Plataformas de observabilidade/tracing

- [LangSmith — Observability Docs](https://docs.langchain.com/langsmith/observability) — plataforma da LangChain para tracing, datasets de avaliação e LLM-as-judge; integra nativamente com LangGraph (stack padrão do harness) mas também com SDKs de outros provedores.
- [Langfuse — Docs](https://langfuse.com/docs) — plataforma open-source de observabilidade/eval/prompt management, auto-hospedável; alternativa ao LangSmith quando self-hosting é requisito (relevante para dado pessoal sensível de qualquer domínio, ver `04-guardrails-seguranca/dados-sensiveis-por-dominio.md`).
- [Braintrust — Docs](https://www.braintrust.dev/docs) — plataforma de eval com foco em CI/CD (evals automáticos em pull requests) e scorers prontos (biblioteca `autoevals`).
- [OpenTelemetry — Docs](https://opentelemetry.io/docs/) — framework vendor-neutral de instrumentação (traces/métricas/logs); referência quando o projeto precisa de observabilidade que não fique presa a um único fornecedor de LLM-ops.

## Frameworks de avaliação (métricas e testes)

- [RAGAS — Docs](https://docs.ragas.io/en/stable/) — framework de avaliação sem necessidade de referência humana para pipelines RAG (faithfulness, answer relevancy, context precision/recall); métricas usadas como base para `metricas.md` quando o projeto envolve RAG.
- [DeepEval — Docs](https://deepeval.com/docs/getting-started) — framework de testes de LLM no estilo "pytest" (G-Eval, hallucination, answer relevancy, RAGAS); permite transformar critérios de aceite do harness em testes automatizados de CI.

## Boas práticas de avaliação em produção (artigos)

- [Hamel Husain — "Your AI Product Needs Evals"](https://hamel.dev/blog/posts/evals/) e [Hamel Husain — LLM Evals FAQ](https://hamel.dev/blog/posts/evals-faq/) — referência prática (autor com experiência ajudando 30+ empresas a montar sistemas de avaliação) sobre por que e como construir evals antes de escalar um produto de IA.
- [Eugene Yan — Writing (tag "eval")](https://eugeneyan.com/tag/eval/) — coleção de artigos sobre avaliação de LLM-evaluators, construção de datasets e harnesses de avaliação; complementa Hamel Husain com foco mais analítico/metodológico.

## Nota de manutenção

LangSmith e a documentação LangChain foram unificadas em `docs.langchain.com` — se um link antigo (`langchain-ai.github.io` ou `docs.smith.langchain.com`) aparecer quebrado em outro lugar do projeto, atualizar para o domínio unificado.
