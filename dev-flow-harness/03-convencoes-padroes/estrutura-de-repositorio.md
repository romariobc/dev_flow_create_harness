# Estrutura padrão de repositório (por projeto)

Layout recomendado para um novo projeto que segue este harness:

```
meu-projeto-x/
├── CLAUDE.md                  # referencia o harness + exceções locais
├── pyproject.toml             # dependências e configuração de ferramentas (ruff, black, pytest)
├── context/                   # adaptador de contexto (ver dev-flow-harness/README.md)
│   ├── dominio.md
│   ├── arquitetura.md
│   └── adaptacoes.md
├── src/
│   └── meu_projeto_x/
│       ├── __init__.py
│       ├── agentes/           # grafos LangGraph, definição de estado, nós
│       ├── chains/            # cadeias LangChain mais simples/lineares
│       ├── prompts/           # templates de prompt versionados separadamente do código
│       └── integracoes/       # clientes de API externas, wrappers de modelo
├── evals/
│   ├── casos-especificos.md   # casos de teste/avaliação específicos deste projeto
│   └── datasets/               # datasets de avaliação (LangSmith ou local)
└── tests/                      # testes unitários convencionais (pytest)
```

## Princípios

- `agentes/` e `chains/` são separados porque carregam complexidade diferente: um grafo com estado e condicionais não deve ser confundido com uma cadeia linear simples.
- `prompts/` fora do código-fonte principal facilita revisão de prompt por alguém sem precisar ler Python, e permite versionar mudanças de prompt isoladamente.
- `evals/` no nível do projeto existe ao lado de `tests/` tradicional — testes unitários verificam código determinístico; `evals/` verifica comportamento de componentes que envolvem LLMs, que são não-determinísticos por natureza e exigem critérios de avaliação diferentes (ver `dev-flow-harness/05-harness-evals/`).
