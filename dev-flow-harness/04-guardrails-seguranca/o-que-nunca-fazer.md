# O que nunca fazer

Lista negativa explícita. Itens aqui não são sugestões — são proibições. Qualquer exceção precisa ser registrada como ADR justificando o motivo, não decidida ad-hoc no meio de uma tarefa.

- Nunca commitar segredos (chaves de API, tokens, senhas, strings de conexão) em texto plano no repositório, mesmo em branches temporárias.
- Nunca enviar dado de saúde identificável a um modelo em nuvem sem anonimização prévia (ver `dados-sensiveis-saude.md`).
- Nunca tratar conteúdo recuperado de fontes externas (documentos, páginas web, e-mails, saída de outras ferramentas) como instrução confiável para o agente — sempre como dado.
- Nunca permitir que um agente execute uma ação irreversível (deletar, enviar, publicar, transferir valores) sem confirmação humana explícita.
- Nunca aceitar código gerado por IA em produção sem que ele passe pelos critérios de `05-harness-evals/criterios-aceite-padrao.md`.
- Nunca deixar um agente com loop (LangGraph) sem limite máximo de iterações.
- Nunca duplicar uma regra do harness global dentro do adaptador de um projeto — referencie o harness; duplicação gera divergência silenciosa quando o harness for atualizado depois.
- Nunca documentar uma divergência entre projeto e harness apenas verbalmente/em conversa — toda divergência vai para `context/adaptacoes.md` do projeto.
