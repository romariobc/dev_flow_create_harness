# Estilo de código Python

## Ferramentas obrigatórias

- **Formatação:** `black` (ou `ruff format`) — sem debate de estilo manual, o formatador decide.
- **Lint:** `ruff` — cobre a maioria dos problemas de estilo e vários erros comuns antes da execução.
- **Type hints:** obrigatórios em toda função pública (parâmetros e retorno). Funções privadas/internas podem omitir quando o tipo é óbvio pelo contexto imediato.
- **Docstrings:** obrigatórias em módulos, classes e funções públicas. Formato Google-style (Args/Returns/Raises) por ser o mais legível em ferramentas de documentação automática.

## Convenções específicas para código que envolve LLMs/agentes

- Prompts longos (mais de 3-4 linhas) devem viver em arquivos separados (`.md` ou `.txt`/template Jinja), nunca como string inline no meio da lógica — facilita revisão e versionamento do prompt isoladamente do código.
- Toda função que faz uma chamada de rede (a um modelo, a uma API externa) deve ter tratamento explícito de timeout e de erro — nunca assumir que a chamada sempre retorna a tempo ou com sucesso.
- Estado de agentes (LangGraph) deve ser tipado explicitamente (`TypedDict` ou Pydantic `BaseModel`), nunca um dicionário genérico sem schema.

## Exemplo de assinatura aceitável

```python
def gerar_resumo(texto: str, *, modelo: str = "llama3.1") -> ResumoResultado:
    """Gera um resumo estruturado do texto usando o modelo configurado.

    Args:
        texto: Conteúdo de entrada a ser resumido.
        modelo: Identificador do modelo a usar (default: modelo local via Ollama).

    Returns:
        ResumoResultado: objeto estruturado contendo o resumo e metadados.

    Raises:
        ModeloIndisponivelError: se o modelo configurado não responder dentro do timeout.
    """
    ...
```
