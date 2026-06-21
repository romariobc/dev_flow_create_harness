# Python + LangChain + LangGraph — padrões de referência

## Quando usar LangChain vs. LangGraph

LangChain é a camada de integração: wrappers de modelos, prompts, parsers de saída, ferramentas (tools) e conectores com fontes de dados. Use-o quando a tarefa é montar um pipeline relativamente linear (ex: receber uma pergunta, buscar contexto, gerar resposta).

LangGraph é a camada de orquestração com estado: modela o fluxo como um grafo de nós (cada nó é uma função ou chamada de LLM) com arestas condicionais e estado persistente entre passos. Use-o quando a tarefa exige decisões (ex: "se a resposta não passou na validação, volte e tente de novo"), múltiplos agentes colaborando, ou qualquer loop — exatamente o tipo de ciclo "contexto → geração → avaliação → ajuste" que este harness assume como padrão de qualidade.

Regra prática: se você consegue desenhar o fluxo como uma linha reta, LangChain isolado é suficiente. Se você precisa desenhar setas que voltam (loops, condicionais, retries), use LangGraph.

## Convenções de código

- Toda chamada a um modelo (local via Ollama ou em nuvem) deve passar por uma função/wrapper único no projeto, nunca espalhada em múltiplos pontos — isso facilita trocar de modelo ou adicionar logging/observabilidade depois.
- Toda saída estruturada esperada de um LLM (JSON, listas, campos específicos) deve ser validada com um parser explícito (ex: `PydanticOutputParser` do LangChain, ou validação manual com Pydantic) — nunca confiar que o modelo respeitou o formato pedido sem checagem.
- Nós de um grafo LangGraph devem ser funções puras sempre que possível (recebem estado, retornam estado atualizado) — evite efeitos colaterais escondidos dentro de um nó.
- Cada agente/grafo deve declarar explicitamente seu estado (schema, geralmente com `TypedDict` ou Pydantic) — estado implícito é uma fonte comum de bugs difíceis de rastrear.

## Armadilhas conhecidas

- Prompts que funcionam bem com um modelo (ex: GPT) podem se comportar diferente com modelos menores rodando localmente via Ollama — sempre re-testar prompts críticos ao trocar de modelo.
- Grafos LangGraph sem limite de iteração podem entrar em loop infinito se a condição de saída depender de uma avaliação do próprio LLM que nunca "concorda" em parar — sempre definir um limite máximo de iterações como guardrail.
- Versões do LangChain mudam APIs com frequência — fixar versão no `requirements.txt`/`pyproject.toml` e documentar a versão testada neste arquivo quando relevante.
