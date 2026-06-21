# Cuidados com dados sensíveis de saúde

Relevante para qualquer projeto que processe dados de pacientes, prescrições, histórico clínico ou qualquer informação de saúde identificável — área de atuação direta do Romário como farmacêutico.

## Por que isto é uma camada própria, e não só "segurança geral"

Dados de saúde são uma categoria especial de dado sensível sob a LGPD (Lei Geral de Proteção de Dados), com exigências mais estritas do que dados pessoais comuns. Um vazamento ou uso indevido de dado de saúde tem consequências legais e éticas mais graves do que a maioria dos outros tipos de dado tratados em software comum — por isso este harness trata o tema como guardrail dedicado, não como item genérico de checklist.

## Princípios

1. **Minimização** — só colete, armazene e envie ao modelo (local ou em nuvem) o dado de saúde estritamente necessário para a tarefa. Se o LLM não precisa do nome completo do paciente para a tarefa, não envie o nome completo.
2. **Anonimização/pseudonimização antes de qualquer chamada externa** — se um dado de saúde precisa ser processado por um modelo em nuvem (fora do seu controle direto), ele deve ser anonimizado ou pseudonimizado antes do envio, sempre que tecnicamente viável.
3. **Preferência por modelo local quando o dado é identificável** — ver `02-dominio-arquitetura/stacks-de-referencia/ollama-local-vs-nuvem.md`: dado de saúde identificável é o caso canônico onde local é o padrão, não a exceção.
4. **Consentimento e finalidade** — qualquer processamento de dado de saúde real (não sintético/de teste) precisa de uma base legal e finalidade claramente definidas, alinhadas à LGPD.
5. **Auditoria** — todo acesso e processamento de dado de saúde identificável deve ser registrável (log de quem/quando/para qual finalidade), mesmo quando o processamento é feito por um agente automatizado.

## Antes de codificar qualquer feature que toque dado de saúde

- [ ] O dado realmente precisa estar identificável para esta tarefa, ou um identificador anonimizado resolve?
- [ ] Existe uma base legal (consentimento, execução de contrato, obrigação legal) documentada para este processamento?
- [ ] O fluxo de dado evita enviar informação identificável a um provedor de modelo em nuvem sem necessidade?
- [ ] Há registro de auditoria para este processamento?

> Nota: este arquivo não substitui orientação jurídica. Para decisões de conformidade LGPD em produção real, validar com responsável jurídico/DPO do projeto.
