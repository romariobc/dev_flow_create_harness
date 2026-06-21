# links.md — Guardrails & Compliance

Fontes sobre segurança de LLM, normas internacionais de risco em IA e regulação brasileira (LGPD/ANVISA). Base direta de `04-guardrails-seguranca/checklist-seguranca-llm.md`, `04-guardrails-seguranca/dados-sensiveis-por-dominio.md` e `04-guardrails-seguranca/o-que-nunca-fazer.md`.

## Bibliotecas de guardrails

- [NVIDIA NeMo Guardrails — GitHub](https://github.com/NVIDIA-NeMo/Guardrails) e [Docs](https://docs.nvidia.com/nemo/guardrails/latest/index.html) — toolkit para adicionar guardrails programáveis (self-check de input/output, detecção de jailbreak/injection, fact-checking) a aplicações conversacionais via Colang.
- [Guardrails AI — Docs](https://guardrailsai.com/guardrails/docs) e [Guardrails Hub](https://guardrailsai.com/hub) — framework Python para validadores de input/output (RAIL format) e para forçar saída estruturada de LLMs; biblioteca de validadores prontos (PII, toxicidade, etc.) reutilizável no harness.
- [LangGraph — Interrupts (human-in-the-loop)](https://docs.langchain.com/oss/python/langgraph/interrupts) — mecanismo nativo do LangGraph para pausar um grafo e aguardar aprovação humana antes de uma ação irreversível; implementação de referência para o princípio "humano no loop" do harness.

## Normas e frameworks de risco

- [OWASP Top 10 for Large Language Model Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) — os 10 riscos de segurança mais críticos em aplicações LLM (prompt injection, vazamento de informação sensível, excessive agency, etc., versão 2.0/2025); checklist de referência direta para `checklist-seguranca-llm.md`.
- [NIST AI Risk Management Framework (AI RMF)](https://www.nist.gov/itl/ai-risk-management-framework) — framework voluntário (2023, com perfil específico para IA generativa publicado em 2024 — NIST-AI-600-1) para incorporar confiabilidade no design/desenvolvimento/uso de sistemas de IA; referência de governança mais ampla que complementa o OWASP (que é focado em segurança técnica).
- [Lilian Weng — "LLM Powered Autonomous Agents"](https://lilianweng.github.io/posts/2023-06-23-agent/) — artigo de referência sobre os componentes de um agente (planejamento, memória, uso de ferramentas) e seus riscos/limitações; leitura de fundamentação conceitual antes de aplicar guardrails específicos.

## Regulação brasileira (contexto de saúde/farmácia do Romário)

- [ANPD — Autoridade Nacional de Proteção de Dados (gov.br)](https://www.gov.br/anpd/pt-br) — autoridade responsável por fiscalizar a LGPD no Brasil; fonte oficial para qualquer decisão de minimização/anonimização de dado pessoal descrita em `dados-sensiveis-por-dominio.md`.
- [ANVISA — RDC 657/2022 (Software como Dispositivo Médico) — Perguntas e Respostas](https://www.gov.br/anvisa/pt-br/assuntos/noticias-anvisa/2022/software-como-dispositivo-medico-perguntas-e-respostas) — norma que regula softwares com finalidade diagnóstica/terapêutica/de monitoramento como dispositivo médico (SaMD); relevante se qualquer funcionalidade de IA do harness avançar para sugestão de conduta clínica ou análise de sintomas (ver nota abaixo).

> **Nota importante (lacuna regulatória):** até a data de pesquisa, a ANVISA **não tem norma específica para IA** em produtos de saúde — a RDC 657/2022 foi desenhada para software "estático" e não cobre integralmente sistemas que se reconfiguram com dados do mundo real (aprendizado contínuo). Avaliar caso a caso se uma funcionalidade do projeto se classifica como SaMD antes de assumir que está fora do escopo regulatório.

## Nota de manutenção

Esta é a camada com maior risco de desatualização jurídica (normas e versões do OWASP Top 10 mudam por ano/versão). Revisar antes de qualquer decisão de compliance com impacto real (não apenas educacional).
