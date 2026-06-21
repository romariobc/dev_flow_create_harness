# Métricas do harness

Métricas a acompanhar ao longo do tempo, para saber se o harness está de fato melhorando a qualidade do que é produzido — e não apenas gerando burocracia.

## Métricas de qualidade

- **Taxa de aprovação na primeira avaliação** — proporção de gerações de código/agente que passam nos critérios de aceite (`criterios-aceite-padrao.md`) sem precisar de ajuste de contexto. Subindo ao longo do tempo = sinal de que o contexto está cada vez mais completo e claro.
- **Número de iterações do loop até aprovação** — quantas voltas do ciclo contexto→geração→avaliação foram necessárias até um resultado ser aceito. Alto e estável = contexto crônico insuficiente em alguma área específica (investigar qual).

## Métricas operacionais (via LangSmith, quando aplicável)

- Latência média por execução, separada por backend (Ollama local vs. modelo em nuvem).
- Custo médio por execução (quando há custo de API envolvido).
- Taxa de erro de chamadas de ferramenta (tool calls).

## Métricas de evolução do harness em si

- Número de regras do harness global que tiveram que ser "puxadas" de um adaptador de projeto (sinal de que uma lição aprendida em um projeto generalizou corretamente).
- Número de divergências documentadas em `context/adaptacoes.md` por projeto — muitas divergências no mesmo ponto do harness é sinal de que aquela regra global pode estar errada ou desatualizada, não que os projetos estão "desviando".
