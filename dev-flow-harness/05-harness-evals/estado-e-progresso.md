# Estado e progresso (subsistema State)

Origem teórica: `references/07-harness-engineering/sintese-conceitual.md`, seção 3 e seção 10 — um dos dois subsistemas dos 5 do walkinglabs que ainda não tinham arquivo dedicado neste harness. Promovido em 2026-06-21 (ver `CHANGELOG.md`).

## Por que existe

Um agente de IA não tem memória entre sessões — o que sobrevive de uma sessão para a próxima é só o que está escrito em disco. Sem um subsistema de State, toda sessão nova começa do zero: o agente não sabe o que já foi feito, refaz trabalho já pronto, ou avança em uma direção diferente da sessão anterior sem perceber a inconsistência. É a coluna "SEM HARNESS" da comparação na síntese conceitual (seção 3) — especificamente as linhas "agente começa do zero" e "agente não tem memória do antes".

State é o que dá à `Verification` (ver `criterios-aceite-padrao.md`) e ao `Session Lifecycle` (ver `ciclo-de-sessao.md`) algo concreto para ler e escrever. Os três juntos formam o ciclo: o `ciclo-de-sessao.md` define **quando** ler e escrever o estado; este arquivo define **o que** escrever e **com que formato**.

## Os dois artefatos

Diferente das convenções deste harness, os artefatos de State não vivem aqui — eles são dados **específicos de cada projeto**, e por isso vivem em `projetos/<projeto>/context/estado/`. O que vive aqui é só a convenção de formato e a regra de quando um status pode avançar. Essa distinção segue o mesmo princípio já registrado em `ARQUITETURA.md`: o harness é regra ativa e reaproveitável, o projeto é a instância concreta.

**`context/estado/feature_list.json`** — lista de features planejadas para o projeto, com status individual. Schema mínimo:

```json
{
  "_comment": "Status válidos: todo, in_progress, blocked, done. Uma feature só pode virar done depois que o critério de aceite referenciado passar de fato — ver dev-flow-harness/05-harness-evals/criterios-aceite-padrao.md.",
  "features": [
    {
      "id": "F001",
      "titulo": "<nome curto da feature>",
      "status": "todo",
      "criterio_de_aceite": "<referência ao critério específico que precisa passar antes de marcar como done>",
      "depende_de": [],
      "observacoes": ""
    }
  ]
}
```

**`context/estado/progresso.md`** — diário de sessão, uma seção por sessão, em ordem cronológica. Ver o template já pronto em `projetos/_template-adapter/context/estado/progresso.md`.

## A regra de avanço de status

`todo` → `in_progress` → `done`, ou `in_progress` → `blocked` quando algo externo impede continuar. A transição para `done` só é válida depois que os critérios de `criterios-aceite-padrao.md` aplicáveis àquela feature passaram com prova executável — nunca por inferência verbal do próprio agente sobre o próprio trabalho ("parece que está funcionando"). Essa é a mesma regra de separação generator/evaluator descrita na seção 6 da síntese conceitual, aplicada em miniatura: o agente que implementa não é a mesma instância de julgamento que decide se o critério passou.

Se uma feature parecer grande demais para uma sessão, ela deve ser quebrada em sub-itens dentro do próprio `feature_list.json` antes de começar — não reescrita ou escondida no meio do trabalho. Reescrever a lista de features para esconder trabalho inacabado é um padrão de falha real documentado na literatura (seção 3 da síntese), não uma hipótese.

## Quem lê e quando

Toda sessão nova lê `progresso.md` e `feature_list.json` antes de tocar em qualquer código, e toda sessão escreve nos dois antes de encerrar. A sequência completa está em `ciclo-de-sessao.md` — este arquivo cobre o formato, aquele cobre o momento.

## Ver também

- `ciclo-de-sessao.md` — quando ler/escrever o estado dentro do ciclo de uma sessão.
- `criterios-aceite-padrao.md` — o que precisa passar antes de uma feature virar `done`.
- `tipologia-controles.md` — distinção entre controle computacional e inferencial, usada para decidir como verificar um critério de aceite.
- `projetos/_template-adapter/context/estado/` — modelo pronto para copiar ao instanciar um projeto novo.
