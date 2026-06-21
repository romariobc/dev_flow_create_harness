# Checklist de segurança para sistemas com LLM

Aplicar antes de considerar qualquer feature que envolva um LLM como "pronta".

## Prompt injection

- [ ] Toda entrada de usuário (ou de fonte externa: documento, página web, e-mail) que é inserida em um prompt foi tratada como **dado**, nunca como **instrução**? O modelo não deve executar comandos que apareçam dentro de conteúdo recuperado/colado.
- [ ] Ferramentas (tools) que o agente pode chamar têm permissões mínimas necessárias — um agente comprometido por injection não deve conseguir, por exemplo, deletar dados ou enviar mensagens externas sem confirmação.
- [ ] Saídas geradas a partir de conteúdo não-confiável são tratadas com a mesma cautela que entradas não-confiáveis (o modelo pode repetir uma instrução maliciosa embutida no conteúdo de entrada).

## Segredos e credenciais

- [ ] Nenhuma chave de API, token, senha ou string de conexão está hardcoded no código ou em prompts versionados — sempre via variável de ambiente ou cofre de segredos.
- [ ] Logs (incluindo traces do LangSmith) não capturam acidentalmente segredos que tenham sido passados como parte de um prompt ou contexto.

## Validação de saída

- [ ] Toda saída estruturada esperada (JSON, código, comandos) é validada antes de ser executada ou persistida — nunca confiar que o modelo respeitou o formato/contrato pedido sem checagem programática.
- [ ] Código gerado por um agente que será executado automaticamente (sem revisão humana) passa por sandboxing ou validação de segurança antes da execução.

## Limites operacionais

- [ ] Todo agente com loop (LangGraph) tem um limite máximo de iterações/chamadas, para evitar loops infinitos ou custo descontrolado.
- [ ] Ações irreversíveis (deletar, enviar, publicar, transferir) nunca são executadas automaticamente por um agente sem confirmação explícita de um humano.

Ver também `dados-sensiveis-por-dominio.md` para o padrão de guardrails de dado sensível por domínio, e `o-que-nunca-fazer.md` para a lista de proibições explícitas deste harness.
