# Convenções de nomenclatura

## Código (Python)

- Variáveis e funções: `snake_case` em português ou inglês — manter consistência dentro de um mesmo projeto, não misturar os dois no mesmo arquivo.
- Classes: `PascalCase`.
- Constantes: `UPPER_SNAKE_CASE`.
- Nomes de grafos/agentes LangGraph: sufixo `_graph` (ex: `triagem_graph`) para distinguir de cadeias simples (`_chain`).

## Branches Git

- `feature/<descricao-curta>` — nova funcionalidade.
- `fix/<descricao-curta>` — correção de bug.
- `harness/<descricao-curta>` — mudança no harness global em si (quando este repositório se tornar versionado em Git).

## Commits

Formato recomendado (Conventional Commits, adaptado):

```
tipo: descrição curta no imperativo

tipo ∈ {feat, fix, docs, refactor, test, chore, harness}
```

Exemplo: `feat: adiciona nó de validação de saída no grafo de triagem`

## Arquivos de contexto e ADRs

- ADRs: `NNNN-titulo-curto.md`, numeração sequencial de 4 dígitos, nunca reaproveitar um número de ADR obsoleto.
- Arquivos de contexto do harness: nomes descritivos em português, minúsculas, com hífen (`checklist-seguranca-llm.md`), nunca espaços ou acentos no nome do arquivo.
