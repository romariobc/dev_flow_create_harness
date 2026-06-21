# LangSmith — observabilidade e avaliação

## Por que isto é parte do harness, não opcional

Um agente que gera código ou respostas sem deixar rastro de **como** chegou ali é uma caixa-preta: quando algo dá errado, não há como saber se o problema foi no prompt, no contexto fornecido, na ferramenta chamada ou no próprio modelo. LangSmith resolve isso registrando cada passo (trace) de uma execução: prompts enviados, respostas recebidas, ferramentas chamadas, tempo e custo de cada etapa.

## Uso mínimo recomendado

- Toda chamada de agente/cadeia relevante (não necessariamente cada chamada de teste isolada) deve ser instrumentada com tracing do LangSmith.
- Cada projeto deve ter pelo menos um "dataset" de avaliação no LangSmith com casos de teste representativos do domínio daquele projeto — ligando diretamente a `05-harness-evals/`.
- Falhas identificadas em produção (ou em uso real) devem ser adicionadas ao dataset de avaliação do projeto, não só corrigidas pontualmente — isso transforma cada erro real em um teste de regressão permanente.

## Métricas a observar

- Taxa de sucesso em avaliações automatizadas (quantos casos do dataset passam no critério de aceite).
- Latência por execução — relevante especialmente comparando modelo local (Ollama) vs. nuvem.
- Custo por execução, quando aplicável (modelos pagos em nuvem).
- Taxa de chamadas de ferramenta (tool calls) que falham ou retornam erro.
