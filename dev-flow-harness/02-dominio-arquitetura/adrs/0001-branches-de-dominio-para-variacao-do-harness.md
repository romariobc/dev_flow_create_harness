# ADR 0001 — Branches de domínio para variação do harness

## Status

Aceito (implementado e verificado em produção nesta sessão: `main` em `2577b1a`, `dominio/saude` em `a6b6de7`, ambos com merge limpo e sem referências órfãs).

## Contexto

O trunk `dev-flow-harness/` é definido (`README.md`, `ARQUITETURA.md`) como "vale para qualquer projeto" — nenhum conteúdo específico de um domínio deveria viver ali. Em revisão de arquitetura solicitada pelo Romário, foi detectado que essa regra estava sendo violada: `04-guardrails-seguranca/dados-sensiveis-saude.md` continha vocabulário, base legal (LGPD Art. 5º, II) e exemplos exclusivos do domínio de saúde, diretamente no trunk, com outros 4 arquivos do harness apontando para ele.

Um grep por palavras-chave de domínio de saúde em todo o workspace confirmou 14 ocorrências. Sete eram legítimas — pertenciam à camada `references/` (que pode conter qualquer assunto, sem compromisso de uso) ou à identidade do operador em `persona.md` (Romário é farmacêutico; isso é dado biográfico do usuário, não regra do harness). As outras 7 indicavam acoplamento real do trunk a um domínio específico — o problema central que este ADR resolve.

O problema não era apenas "saúde está no lugar errado": o harness, por natureza, vai ser aplicado a múltiplos domínios e plataformas no futuro (outros domínios regulados, além de variações de plataforma como web/mobile). Faltava um mecanismo formal para variação por domínio que não contaminasse o trunk.

## Decisão

`main` nunca contém conteúdo específico de domínio. Qualquer guardrail, vocabulário ou exemplo que só faz sentido para um domínio (dado de saúde, dado financeiro, dado biométrico, dado de menor, entre outros que vierem) vive em uma branch derivada de `main`, nomeada `dominio/<nome>` (ex.: `dominio/saude`). Essa branch:

- parte de `main` e herda 100% do trunk agnóstico no momento da criação;
- adiciona apenas os arquivos específicos daquele domínio, instanciando — nunca duplicando — o padrão estrutural definido no trunk (`04-guardrails-seguranca/dados-sensiveis-por-dominio.md`);
- é onde projetos daquele domínio nascem e evoluem; dentro dela, cada projeto ainda decide sua própria arquitetura concreta (web, mobile, banco de dados) em `context/arquitetura.md` — plataforma/visão é detalhe de projeto, não vira branch nem camada nova do harness.

Verificável (como qualquer decisão registrada neste harness deve ser):

- nenhum arquivo de `dev-flow-harness/` em `main` deve conter conteúdo específico de domínio — checável por grep periódico contra termos de domínio conhecido;
- toda branch `dominio/<nome>` deve ter `main` como ancestral direto no momento da criação (fast-forward possível no fork);
- um padrão recorrente entre domínios (ex.: "cuidado com dado sensível") vive uma única vez no trunk como estrutura genérica; cada domínio só preenche a instância, nunca recria os princípios do zero.

## Alternativas consideradas

1. **Camada de "perfis" dentro do trunk** (`dev-flow-harness/perfis/dominio/` + `perfis/visao/`, dois eixos compostos) — descartada porque plataforma/visão (web, mobile) não é uma preocupação recorrente entre projetos do mesmo jeito que domínio legal/regulatório é. É detalhe de projeto (`context/arquitetura.md`), não harness.
2. **Pasta nova dentro do trunk por domínio** (`dev-flow-harness/dominios/saude/`) — descartada porque reintroduz exatamente o problema que motivou esta revisão: conteúdo de domínio dentro do que o harness define como agnóstico, só movido de lugar.
3. **Manter `dados-sensiveis-saude.md` no trunk como estava** — descartada porque viola a definição central de `dev-flow-harness/` e o próprio critério "não duplica" no momento em que um segundo domínio aparecer (o princípio teria que ser reescrito, ou duplicado, por domínio).

## Harnessability

N/A — decisão de estrutura de repositório/branching via git nativo, não de escolha de stack, linguagem ou framework de código.

## Consequências

Facilita: trunk permanece pequeno e genérico; cada domínio tem seu próprio espaço para vocabulário, base legal e exemplos sem poluir o que outros domínios herdam; o padrão estrutural (5 princípios de dado sensível) é escrito uma única vez e só instanciado, reduzindo o custo de manter múltiplos domínios simultâneos.

Custo, nomeado explicitamente — toda decisão tem um: branches de domínio acumulam dívida de merge, já que uma correção em `main` não chega automaticamente a `dominio/saude`. Mitigação operacional, já testada em produção nesta sessão com um merge real não-trivial (`Merge made by the 'ort' strategy`, sem conflitos): a cada promoção registrada em `CHANGELOG.md`, rodar `git checkout dominio/<nome> && git merge main` antes de continuar trabalhando na branch (ver `ARQUITETURA.md`, seção "Variação por domínio: branches", subseção "O custo, e como mitigar"). Ficou demonstrado também que erros de consistência (referências a um arquivo renomeado, em `references/*/links.md`) só aparecem depois da mudança — toda promoção de domínio deve terminar com um grep de verificação por referências órfãs antes de ser considerada concluída.

Decisão correlata, tomada na mesma sessão e registrada aqui por completude: não foi instalado nenhum conector/plugin de GitHub no Cowork. A pesquisa no registro de MCP não encontrou conector dedicado — GitHub só está disponível dentro do plugin `engineering@knowledge-work-plugins`, que empacota 10 conectores genéricos (Slack, Linear, Asana, Atlassian, Notion, PagerDuty, Datadog, Google Calendar, Gmail, GitHub) e 10 skills genéricas. Apresentado o trade-off ao Romário usando o próprio critério 3 de promoção do harness ("custo de adoção baixo a médio"), ele optou por continuar com git local, guiado por mim a partir do output colado do terminal. Reabrir esta discussão só se o custo de operar git manualmente se tornar alto na prática (ex.: frequência de PRs, necessidade de CI).

## Ver também

- `ARQUITETURA.md` — seção "Variação por domínio: branches" (texto-base por trás desta decisão).
- `04-guardrails-seguranca/dados-sensiveis-por-dominio.md` — primeiro (e até agora único) padrão estrutural instanciado por uma branch de domínio.
- `dev-flow-harness/CHANGELOG.md`, entrada de 2026-06-21 — registro da promoção em formato changelog.
