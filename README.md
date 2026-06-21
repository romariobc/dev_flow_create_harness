# Cowork_dev_flow_create

Espaço de trabalho raiz do projeto **dev_flow_criate**: um framework de desenvolvimento de software assistido por IA, com foco em engenharia de contexto, harness, governança, guardrails e evals.

## O que tem aqui

- **`references/`** — pesquisa e teoria: artigos, ebooks e sínteses lidos sobre o tema, sem compromisso de uso. É a camada do "porquê".
- **`dev-flow-harness/`** — o harness global: regras, convenções, guardrails e critérios de avaliação reutilizáveis em qualquer projeto seu. É a fonte de verdade que não muda de projeto para projeto. A branch `main` é sempre agnóstica de domínio; variações de domínio (saúde, e outros que vierem) vivem em branches `dominio/<nome>` — ver `ARQUITETURA.md`, seção "Variação por domínio: branches".
- **`projetos/`** — cada subpasta é um projeto real, montado a partir do template de adaptador (`projetos/_template-adapter/`). Cada projeto referencia o harness acima e registra apenas o que é específico dele.

Ver `ARQUITETURA.md` para o modelo completo de três camadas e a regra de quando uma ideia de `references/` pode se tornar regra em `dev-flow-harness/`.

## Como começar um projeto novo

1. Copie `projetos/_template-adapter/` para `projetos/<nome-do-projeto>/`.
2. Preencha `context/dominio.md` e `context/arquitetura.md` com as regras específicas do novo projeto.
3. Abra o projeto no Claude Code / VS Code — o `CLAUDE.md` do projeto referencia automaticamente o harness global.
4. Antes de aceitar código gerado, confira contra `dev-flow-harness/05-harness-evals/criterios-aceite-padrao.md`.

## Status

Pasta local sincronizada por ora. Plano declarado: disponibilizar o harness como repositório open source no Git no futuro — quando isso acontecer, `dev-flow-harness/` se torna um submódulo Git em vez de uma cópia local, e cada projeto aponta para uma versão/tag fixa dele.
