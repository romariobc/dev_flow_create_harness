# links.md — Protocols & Tools

Fontes sobre o Model Context Protocol (MCP), tool calling, segurança de protocolo e comunicação entre agentes. Substanciam decisões em `04-guardrails-seguranca/checklist-seguranca-llm.md` sobre como expor e consumir ferramentas com segurança.

## Model Context Protocol (MCP)

- [Model Context Protocol — Documentação oficial](https://modelcontextprotocol.io) — especificação e guia de conceitos (tools, resources, prompts) do protocolo padrão para conectar LLMs a sistemas externos; base de qualquer integração de ferramentas no harness.
- [Introducing the Model Context Protocol — Anthropic](https://www.anthropic.com/news/model-context-protocol) — anúncio oficial com a motivação original do protocolo; útil para justificar a escolha de MCP em vez de integrações ad-hoc.
- [modelcontextprotocol/servers — GitHub](https://github.com/modelcontextprotocol/servers) — repositório oficial de servidores de referência (filesystem, git, fetch, memory, sequential-thinking) mantidos pelo steering group do MCP; ponto de partida para implementações próprias.
- [modelcontextprotocol/modelcontextprotocol — GitHub](https://github.com/modelcontextprotocol/modelcontextprotocol) — repositório da especificação formal do protocolo (schema, versionamento); referência para quem for implementar um servidor MCP do zero.

## Segurança de protocolo e ferramentas

- [OWASP MCP Top 10](https://owasp.org/www-project-mcp-top-10/) — projeto da OWASP catalogando os 10 riscos mais críticos em deployments MCP (ex.: tool poisoning, shadow servers, token mismanagement); referência direta para o checklist de segurança do harness. Projeto em beta — revisar versão mais recente antes de citar números/categorias específicas.
- [Endor Labs — State of Dependency Management 2025](https://www.endorlabs.com/lp/state-of-dependency-management-2025) — pesquisa sobre riscos de supply chain introduzidos por agentes de IA e servidores MCP (ex.: % de servidores MCP usando APIs sensíveis sem controles adequados); dado empírico que substancia a cautela do harness com MCP servers de terceiros.

## Comunicação entre agentes e automação de ferramentas

- [Agent2Agent (A2A) Protocol — GitHub (a2aproject)](https://github.com/a2aproject/A2A) — protocolo aberto (doado por Google à Linux Foundation em 2025) para comunicação entre agentes de frameworks/fornecedores diferentes; relevante se o projeto evoluir para multi-agente entre sistemas distintos.
- [browser-use — GitHub](https://github.com/browser-use/browser-use) — biblioteca open-source que expõe um navegador como ferramenta para agentes; referência para tool calling sobre superfícies web quando não há API estruturada disponível.

## Nota de manutenção

O ecossistema MCP está em rápida evolução (specs novas, ferramentas de segurança, registries). Revisar este arquivo com mais frequência que os demais.
