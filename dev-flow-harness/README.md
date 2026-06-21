# dev-flow-harness

Harness global e reutilizável para desenvolvimento de software assistido por IA. Esta pasta concentra tudo que deve valer **para qualquer projeto**, independentemente do domínio: identidade do agente, padrões de arquitetura para o stack Python/LangChain/LangGraph/LangSmith/Hugging Face/Ollama, convenções de código, guardrails de segurança e critérios de avaliação (evals).

## Por que isto existe

Um agente de IA não tem memória entre sessões. O que sobrevive de uma sessão para outra não é o que o modelo "lembrou" — é o que está escrito em arquivos que o próximo agente vai ler de novo. Sem um harness centralizado, cada projeto novo reinventa (de forma inconsistente, às vezes insegura) decisões que já deveriam estar resolvidas. Este harness é a tentativa de fixar essas decisões uma única vez e reaproveitá-las sempre.

## Estrutura

| Pasta | Camada do modelo de contexto | Conteúdo |
|---|---|---|
| `01-identidade-memoria/` | Identidade & memória | Quem o agente deve ser, vocabulário comum |
| `02-dominio-arquitetura/` | Domínio & arquitetura | Padrões de stack, ADRs |
| `03-convencoes-padroes/` | Convenções & padrões | Estilo de código, estrutura de repositório |
| `04-guardrails-seguranca/` | Guardrails & segurança | O que nunca fazer, cuidados com dados sensíveis |
| `05-harness-evals/` | Harness & evals | Critérios de aceite, testes, métricas, estado/progresso entre sessões, ciclo de vida da sessão |

## Como um projeto usa isto

O `CLAUDE.md` de cada projeto (em `projetos/<projeto>/CLAUDE.md`) referencia este harness explicitamente, geralmente assim:

```markdown
## Contexto herdado
Este projeto segue as convenções, guardrails e critérios de avaliação definidos em
`../../dev-flow-harness/`. Leia especialmente:
- 03-convencoes-padroes/estilo-python.md
- 04-guardrails-seguranca/checklist-seguranca-llm.md
- 05-harness-evals/criterios-aceite-padrao.md
- 05-harness-evals/ciclo-de-sessao.md
- 05-harness-evals/estado-e-progresso.md

## Exceções deste projeto
(ver context/adaptacoes.md)
```

Isso garante que o agente, ao abrir qualquer projeto, carregue tanto o que é universal (harness) quanto o que é local (adaptador), sem duplicar texto entre os dois.

## Evolução

Toda vez que uma lição aprendida em um projeto específico se mostrar útil para os demais, ela deve "subir" do adaptador do projeto para este harness — e não ficar presa lá. É assim que o framework melhora com o tempo em vez de ficar estático.
