# Ciclo de sessão (subsistema Session Lifecycle)

Origem teórica: `references/07-harness-engineering/sintese-conceitual.md`, seção 3 e seção 10 — o segundo dos dois subsistemas dos 5 do walkinglabs que ainda não tinham arquivo dedicado neste harness, explicitamente sinalizado ali como "a lacuna mais visível depois desta pesquisa". Promovido em 2026-06-21 (ver `CHANGELOG.md`).

## Por que existe

A diferença entre um agente confiável ao longo de várias sessões e um agente que precisa de correção manual constante não está, na maioria das vezes, em nenhuma sessão individual — está em **o que acontece entre uma sessão e a próxima**. A síntese conceitual (seção 3) registra essa diferença de forma direta:

```
SEM HARNESS                                  COM HARNESS
============                                 ============
Sessão 1: agente escreve código              Sessão 1: agente lê instruções
          agente quebra testes                         agente roda init.sh
          agente diz "concluído"                       agente trabalha em 1 feature
          você corrige manualmente                     agente verifica antes de declarar feito
                                                         agente atualiza o log de progresso
Sessão 2: agente começa do zero                         agente faz commit em estado limpo
          agente não tem memória do antes
          agente refaz trabalho             Sessão 2: agente lê o log de progresso
          ou faz outra coisa                          agente continua exatamente de onde parou
          você corrige de novo                         você revisa, não resgata
```

Este arquivo formaliza a coluna da direita como uma sequência de 4 etapas — as mesmas mostradas no diagrama apresentado nesta conversa.

## As 4 etapas

**1. Inicializa.** Antes de tocar em qualquer código: ler o `CLAUDE.md` do projeto (que por sua vez aponta para o harness global); ler `context/estado/progresso.md` — se a sessão anterior existir, ela já deixou um próximo passo explícito; ler `context/adaptacoes.md` para saber se algo neste projeto diverge do harness padrão; confirmar a baseline rodando a suíte de testes uma vez **antes** de qualquer alteração, para saber se algo já estava quebrado antes desta sessão (evita atribuir à sessão atual um problema que já existia).

**2. Trabalha.** Pegar exatamente 1 feature de `feature_list.json` com status `todo` ou `in_progress` — nunca duas ao mesmo tempo (ver a regra de Scope em `02-dominio-arquitetura/`). Se a feature parecer grande demais para a sessão, quebrar em sub-itens dentro do próprio `feature_list.json` antes de começar, não no meio do trabalho.

**3. Verifica.** Rodar os critérios aplicáveis de `criterios-aceite-padrao.md`. Só prova executável conta como "feito" — nunca declarar uma feature concluída por inferência verbal sobre o próprio trabalho. Esta é a etapa mais fácil de pular sob pressão de prazo, e por isso a mais citada na literatura como a que mais falta na prática (síntese conceitual, seção 3).

**4. Persiste e encerra.** Atualizar `feature_list.json` (status) e `progresso.md` (diário da sessão — ver `estado-e-progresso.md` para o formato), deixando um handoff explícito: o que ficou pendente, qual é o próximo passo concreto, e qualquer decisão tomada durante a sessão que deveria virar ADR e ainda não virou. Encerrar em estado limpo (sem testes quebrados sem registro do motivo).

## A regra de handoff

Nunca encerrar uma sessão com `progresso.md` registrando algo como "concluído" que não passou pela etapa 3. Isso é a mesma regra já presente em `01-identidade-memoria/persona.md` — nunca confiar em sucesso único como prova — aplicada especificamente à fronteira entre sessões, que é onde esse tipo de afirmação não verificada tende a se acumular sem ser notada.

## Ver também

- `estado-e-progresso.md` — formato de `feature_list.json` e `progresso.md`.
- `criterios-aceite-padrao.md` — o que precisa passar na etapa de Verificação.
- `tipologia-controles.md` — controle computacional vs. inferencial, para decidir como verificar.
- `02-dominio-arquitetura/` — a regra de Scope (uma feature por vez).
- `projetos/_template-adapter/context/estado/` — modelo pronto para copiar ao instanciar um projeto novo.
