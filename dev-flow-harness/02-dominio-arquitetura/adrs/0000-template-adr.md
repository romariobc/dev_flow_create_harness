# ADR 0000 — Template

> Copie este arquivo para `NNNN-titulo-curto-da-decisao.md` (numeração sequencial) ao registrar uma nova decisão de arquitetura. Um ADR documenta uma decisão tomada — não é um documento de planejamento aberto.

## Status

Proposto | Aceito | Substituído por ADR-XXXX | Obsoleto

## Contexto

Qual problema ou pergunta motivou esta decisão? Que restrições (técnicas, regulatórias, de prazo) estavam em jogo?

## Decisão

O que foi decidido, de forma direta e verificável (deve ser possível checar, lendo o código, se a decisão foi seguida).

## Alternativas consideradas

Lista breve de outras opções avaliadas e por que foram descartadas. Evita que a mesma alternativa seja "redescoberta" e debatida de novo sem necessidade.

## Harnessability (preencher quando a decisão envolve escolha de stack, linguagem ou framework)

Quanto mais fácil for guiar a IA dentro da opção escolhida, menor o esforço de harness (guardrails, evals, revisão) necessário depois. Avaliar isso na escolha evita herdar um esforço maior do que o necessário.

- **Tipagem:** forte / fraca / dinâmica.
- **Convenções:** o framework impõe estrutura própria, ou o projeto precisa definir a sua do zero?
- **Testabilidade:** isolamento de unidade alto / médio / baixo.
- **Affordances ambientais:** injeção de dependência nativa, schema explícito (ex.: Pydantic), LSP/tipagem rica em IDE.
- **Esforço de harness estimado:** baixo / médio / alto — quanto guardrail e quanto eval esta escolha provavelmente vai exigir.

> Por quê: linguagens fortemente tipadas e frameworks com convenções claras reduzem erro de IA "de graça" — antes de qualquer guardrail escrito. Ver `references/07-harness-engineering/` para a origem teórica do conceito.

## Consequências

O que essa decisão facilita, e o que ela torna mais difícil ou impõe como restrição futura. Toda decisão tem um custo — nomeá-lo aqui evita surpresas depois.
