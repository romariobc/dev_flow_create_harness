# Dados sensíveis de saúde

Instância, nesta branch (`dominio/saude`), do padrão estrutural definido em `04-guardrails-seguranca/dados-sensiveis-por-dominio.md`. Aplica-se a qualquer projeto criado dentro desta branch que processe dado de paciente, prescrição, prontuário, exame ou qualquer informação clínica identificável. Os 5 princípios (minimização, anonimização antes de nuvem, preferência local, consentimento/finalidade, auditoria) e o checklist-base vivem no arquivo do padrão — este arquivo só acrescenta o que é específico de saúde.

## Base legal e categoria de dado

- LGPD (Lei 13.709/2018), Art. 5º, II — dado de saúde é classificado como **dado pessoal sensível**, com regras de tratamento mais restritivas que dado pessoal comum (Art. 11: tratamento permitido apenas em hipóteses específicas — consentimento destacado, tutela da saúde, entre outras).
- Qualquer feature que combine identificador de pessoa (nome, CPF, prontuário) com informação clínica (diagnóstico, medicamento em uso, resultado de exame, posologia) está sob este guardrail.
- Não existe "anonimização suficiente" universal para dado clínico — combinações de poucos campos (ex.: CID-10 + idade + município) costumam ser reidentificáveis. Na dúvida, tratar como identificável e aplicar o princípio 3 do padrão (preferência por modelo local).

## Exemplos concretos do domínio

- Prontuário eletrônico, prescrição médica e posologia associada a um paciente nomeado.
- Histórico de dispensação de medicamento controlado vinculado a CPF/nome (relevante para qualquer projeto de farmácia/dispensação).
- Campo de texto livre clínico (anotação de atendimento, evolução do paciente) — é o caso mais fácil de vazar dado sensível sem querer, porque não tem schema que permita filtrar antes do envio a um modelo.
- Código CID-10, ou nome de medicamento controlado, associado a um identificador de paciente.

## Checklist específico de saúde

Além das 4 perguntas-base de `dados-sensiveis-por-dominio.md`, verifique antes de codificar:

- [ ] Este dado se classifica como dado de saúde pela LGPD Art. 5º, II? Se sim, o princípio 3 do padrão (preferência por modelo local quando identificável) é regra, não sugestão.
- [ ] Há campo de texto livre clínico no fluxo? Se sim, ele passa por alguma anonimização/triagem antes de qualquer chamada a modelo — mesmo modelo local?
- [ ] Em caso de incidente, o projeto consegue identificar quais registros de paciente foram expostos? (Rastreabilidade mínima, alinhada à LGPD Art. 48.)

## Ver também

- `04-guardrails-seguranca/dados-sensiveis-por-dominio.md` — padrão estrutural que este arquivo instancia.
- `02-dominio-arquitetura/stacks-de-referencia/ollama-local-vs-nuvem.md`.
- `04-guardrails-seguranca/o-que-nunca-fazer.md`.

> Nota: este arquivo não substitui orientação jurídica. A classificação de um dado específico como "dado de saúde" para fins de LGPD, em caso de dúvida real, deve ser validada com o DPO/responsável jurídico do projeto.
