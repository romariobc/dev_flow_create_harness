# Ollama local vs. deployment em nuvem

## Quando usar local (Ollama)

- Desenvolvimento e prototipagem rápida, sem custo por chamada.
- Dados sensíveis que não devem saudavelmente trafegar para um provedor externo (ver `04-guardrails-seguranca/dados-sensiveis-saude.md` — relevante para qualquer projeto que toque dados de saúde).
- Cenários offline ou com restrição de conectividade.

## Quando usar nuvem

- Necessidade de modelos maiores/mais capazes do que o hardware local suporta.
- Cargas de produção com necessidade de escalar horizontalmente.
- Quando o custo por chamada é aceitável frente ao ganho de qualidade/capacidade.

## Padrão de projeto recomendado

Projetar a camada de acesso ao modelo (ver `python-langchain-langgraph.md`) de forma que troca entre Ollama local e um provedor em nuvem seja uma mudança de configuração, não de código — ex: uma variável de ambiente que seleciona o backend, com a mesma interface (LangChain `ChatModel` ou equivalente) por trás. Isso permite desenvolver e testar localmente, e só trocar para nuvem quando necessário, sem reescrever lógica de negócio.

## Checklist antes de decidir

1. Os dados que serão enviados ao modelo contêm informação sensível? Se sim, local é o padrão, nuvem é a exceção que precisa de justificativa.
2. O modelo local disponível é suficiente para a tarefa, ou a diferença de qualidade compromete o resultado?
3. O custo de operar em nuvem é sustentável dado o volume esperado de chamadas?
