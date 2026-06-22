# _template-adapter

Molde para instanciar um novo projeto sob o harness. Dois modos de uso:

---

## Modo A — Projeto interno (dentro deste repo)

Use quando o projeto vive em `projetos/<nome>/` dentro do próprio `dev_flow_create_harness`.

1. Copie esta pasta inteira para `projetos/<nome-do-projeto>/`
2. Renomeie e preencha `CLAUDE.md` (campos marcados com `<...>`)
3. Preencha `context/dominio.md`, `context/arquitetura.md` e `context/adaptacoes.md`
4. Popule `context/estado/feature_list.json` com as primeiras features (`status: "todo"`)
5. Mantenha os caminhos relativos do `CLAUDE.md` — eles já apontam para `../../dev-flow-harness/`

---

## Modo B — Bootstrap para repositório externo

Use quando o projeto tem seu próprio repositório GitHub (ciclo de vida independente, CI/CD, colaboradores).

### Passo 1 — Copiar a estrutura de contexto

No repositório de destino, crie:

```
context/
├── adaptacoes.md      ← copiar de context/adaptacoes.md deste template
├── arquitetura.md     ← copiar de context/arquitetura.md
├── dominio.md         ← copiar de context/dominio.md
└── estado/
    ├── feature_list.json   ← copiar de context/estado/feature_list.json
    └── progresso.md        ← copiar de context/estado/progresso.md
evals/
└── casos-especificos.md    ← copiar de evals/casos-especificos.md
```

`src/` não é copiado — código nasce no repo de destino.

### Passo 2 — Criar o CLAUDE.md do repo externo

O `CLAUDE.md` de um repo externo **não usa caminhos relativos** (o harness está em outro repo). Use este padrão:

```markdown
# <nome-do-projeto> — CLAUDE.md

## Harness de referência

Este projeto opera sob o harness de engenharia de contexto em:
https://github.com/romariobc/dev_flow_create_harness (branch: dominio/<nome>)

Antes de qualquer tarefa, leia (clonando ou via GitHub):
- dev-flow-harness/01-identidade-memoria/persona.md
- dev-flow-harness/03-convencoes-padroes/estilo-python.md
- dev-flow-harness/04-guardrails-seguranca/checklist-seguranca-llm.md
- dev-flow-harness/05-harness-evals/criterios-aceite-padrao.md
- dev-flow-harness/05-harness-evals/ciclo-de-sessao.md — leia ANTES de tocar em código
- dev-flow-harness/05-harness-evals/estado-e-progresso.md

## Regra de precedência

Se este CLAUDE.md contradizer o harness, este vence — mas a contradição deve estar
documentada em context/adaptacoes.md. Contradição não documentada é tratada como erro.

## Contexto específico deste projeto

- context/dominio.md — regras de negócio
- context/arquitetura.md — decisões de arquitetura
- context/adaptacoes.md — onde este projeto diverge do harness, e por quê
- context/estado/feature_list.json e context/estado/progresso.md — estado atual

## Resumo do projeto

(Preencher: o que este projeto faz, em 2-3 frases.)

## Stack

Python · LangChain · LangGraph · LangSmith · Hugging Face · Ollama
(salvo indicação contrária em context/adaptacoes.md)
```

### Passo 3 — Registrar no harness

No `dev_flow_create_harness`, registre o novo projeto em `dev-flow-harness/CHANGELOG.md`:

```
YYYY-MM-DD | bootstrap | <nome-do-projeto> | repo: <url> | branch: dominio/<nome>
```

### Passo 4 — Promover lições aprendidas

Quando uma decisão tomada no projeto externo for útil para outros projetos, abra um PR em `dev_flow_create_harness` atualizando o arquivo correto em `dev-flow-harness/`. Registre a promoção no `CHANGELOG.md`.

---

## O que não copiar

| Item | Motivo |
|---|---|
| `src/` | Código nasce no repo de destino |
| `../../dev-flow-harness/` (caminhos relativos) | Só funcionam no Modo A |
| Qualquer arquivo de `dev-flow-harness/` | O projeto referencia, não duplica |
