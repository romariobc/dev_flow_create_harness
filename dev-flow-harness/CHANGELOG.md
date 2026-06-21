# Changelog do harness

Registro de toda promoção de conceito: de `references/` para `dev-flow-harness/` (teoria → regra), ou de um adaptador de projeto para o harness global (lição aprendida → regra geral). Ver a regra de promoção completa em `../ARQUITETURA.md`.

Formato de cada entrada: data, conceito, origem, destino, critério que justificou a promoção.

## 2026-06-21

- **Tipologia de controles (computacional × inferencial)** — origem: `references/07-harness-engineering/sintese-conceitual.md` (distinção de Böckeler/Fowler). Destino: `05-harness-evals/tipologia-controles.md`. Critério: recorrente na fonte teórica, aplicável direto ao stack declarado, operacionalizável como regra de decisão, não duplicava nada existente.
- **Harnessability como critério de ADR** — origem: `references/07-harness-engineering/`. Destino: seção nova em `02-dominio-arquitetura/adrs/0000-template-adr.md`. Critério: gap confirmado por leitura direta do template real, custo de adoção baixo, encaixa na estrutura existente sem reescrever o template.
- **State (estado e progresso)** — origem: `references/07-harness-engineering/sintese-conceitual.md` (subsistema "State" dos 5 do walkinglabs, citado também na comparação SEM HARNESS × COM HARNESS da seção 3). Destino: `05-harness-evals/estado-e-progresso.md`, com scaffold real em `projetos/_template-adapter/context/estado/` (`feature_list.json` + `progresso.md`). Critério: recorrente em múltiplas fontes (walkinglabs, caso OpenAI), aplicável ao stack sem ferramenta nova, custo baixo (2 arquivos), gap real confirmado por leitura direta (não havia convenção de estado em nenhum lugar do harness), operacionalizável (schema de JSON + regra de transição de status ligada a `criterios-aceite-padrao.md`).
- **Session Lifecycle (ciclo de sessão)** — origem: `references/07-harness-engineering/sintese-conceitual.md`, seção 10 — gap explicitamente sinalizado ali como "a lacuna mais visível" antes desta promoção. Destino: `05-harness-evals/ciclo-de-sessao.md`. Critério: mesmos cinco critérios do item anterior; o conteúdo do próprio gap apontado na pesquisa serviu de prova de recorrência e de não-duplicação.
