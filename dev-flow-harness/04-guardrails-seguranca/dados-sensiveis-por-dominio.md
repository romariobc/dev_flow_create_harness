# Cuidados com dados sensíveis específicos de domínio

Padrão estrutural, agnóstico de domínio. Vale para qualquer projeto que processe uma categoria de dado pessoal com exigência legal ou regulatória própria — dados de saúde, dados financeiros, dados biométricos, dados de menores, entre outros que vierem a aparecer. Cada domínio concreto instancia este padrão em uma branch própria (ver `ARQUITETURA.md`, seção "Variação por domínio: branches"), preenchendo o detalhe legal/regulatório que só faz sentido naquele domínio específico.

## Por que isto é um padrão no trunk, e não um guardrail de um domínio só

Diferentes categorias de dado pessoal sensível — dado de saúde sob a LGPD, dado financeiro sob normas do Banco Central, dado biométrico, dado de menor sob o ECA — compartilham a mesma estrutura de risco, mas cada uma tem exigência legal própria. Manter o padrão agnóstico aqui no trunk, e a instância concreta na branch de domínio, evita duas falhas opostas: (a) o trunk virar uma lista crescente de domínios que ele não deveria conhecer, violando a definição de `dev-flow-harness/` como "vale para qualquer projeto"; e (b) cada branch de domínio reinventar os princípios do zero, sem a base comum já testada aqui.

## Os 5 princípios — instancie estes, não invente novos por domínio

1. **Minimização** — só colete, armazene e envie ao modelo (local ou em nuvem) o dado estritamente necessário para a tarefa. Se o LLM não precisa de um identificador direto para cumprir a tarefa, não envie o identificador direto.
2. **Anonimização/pseudonimização antes de qualquer chamada externa** — se o dado precisa ser processado por um modelo fora do seu controle direto (nuvem), anonimize ou pseudonimize antes do envio, sempre que tecnicamente viável.
3. **Preferência por modelo local quando o dado é identificável** — ver `02-dominio-arquitetura/stacks-de-referencia/ollama-local-vs-nuvem.md`: dado sensível identificável é o caso canônico onde local é o padrão, não a exceção.
4. **Consentimento e finalidade** — qualquer processamento de dado real (não sintético/de teste) precisa de uma base legal e finalidade claramente definidas.
5. **Auditoria** — todo acesso e processamento de dado sensível identificável deve ser registrável (log de quem/quando/para qual finalidade), mesmo quando o processamento é feito por um agente automatizado.

## Como instanciar este padrão em uma branch de domínio

Ao criar `dominio/<nome>` (ex.: `dominio/saude`), adicione nessa branch um arquivo `04-guardrails-seguranca/dados-sensiveis-<nome>.md` que:

- referencia este arquivo como o padrão estrutural que está sendo instanciado;
- nomeia a categoria de dado e a base legal específica daquele domínio (ex.: LGPD + dado de saúde; ECA + dado de menor);
- preenche o checklist abaixo com qualquer pergunta adicional específica do domínio, sem remover as quatro perguntas-base.

## Checklist mínimo antes de codificar qualquer feature que toque o dado

- [ ] O dado realmente precisa estar identificável para esta tarefa, ou um identificador anonimizado resolve?
- [ ] Existe uma base legal (consentimento, execução de contrato, obrigação legal) documentada para este processamento?
- [ ] O fluxo de dado evita enviar informação identificável a um provedor de modelo em nuvem sem necessidade?
- [ ] Há registro de auditoria para este processamento?

> Nota: este arquivo, e qualquer instância de domínio dele, não substitui orientação jurídica. Para decisões de conformidade em produção real, validar com o responsável jurídico/DPO do projeto.

## Ver também

- `ARQUITETURA.md` — seção "Variação por domínio: branches".
- `02-dominio-arquitetura/stacks-de-referencia/ollama-local-vs-nuvem.md`.
- `04-guardrails-seguranca/o-que-nunca-fazer.md`.
