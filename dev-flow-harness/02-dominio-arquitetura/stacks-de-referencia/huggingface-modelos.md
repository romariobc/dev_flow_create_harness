# Hugging Face — critérios de seleção de modelo

## Quando recorrer ao Hugging Face Hub

Use o Hub para encontrar modelos open-weight (para rodar localmente via Ollama ou auto-hospedados), datasets para fine-tuning ou avaliação, e bibliotecas de apoio (ex: `transformers`, `sentence-transformers`).

## Critérios de seleção, em ordem de prioridade

1. **Licenciamento** — confirmar que a licença do modelo permite o uso pretendido (comercial, pesquisa, etc.) antes de qualquer outro critério. Modelos com licenças restritivas (ex: uso não-comercial) são descartados de imediato para projetos com potencial uso comercial.
2. **Tamanho vs. hardware disponível** — para uso local via Ollama, o tamanho do modelo (parâmetros, quantização) precisa ser compatível com a máquina disponível. Documentar no projeto qual configuração de hardware foi usada para validar a escolha.
3. **Benchmarks relevantes ao domínio** — preferir modelos com benchmarks publicados próximos da tarefa real (ex: benchmarks de raciocínio para tarefas analíticas, benchmarks multilíngues para conteúdo em português).
4. **Atividade e manutenção do modelo/repositório** — preferir modelos com atualizações recentes e comunidade ativa, especialmente para uso em produção.

## Boas práticas

- Registrar no ADR do projeto (`02-dominio-arquitetura/adrs/`) qual modelo foi escolhido e por quê — isso evita reavaliar a mesma decisão do zero em projetos futuros.
- Nunca commitar pesos de modelo grandes em repositórios de código — usar cache local (ex: `~/.cache/huggingface`) ou Ollama, e documentar como reproduzir o download.
