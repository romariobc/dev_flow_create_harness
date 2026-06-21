# Critérios de aceite padrão

Critérios mínimos que qualquer código ou comportamento gerado por IA precisa satisfazer antes de ser integrado a um projeto que segue este harness. Critérios adicionais específicos de um projeto vão em `evals/casos-especificos.md` daquele projeto — este arquivo cobre apenas o que é universal.

Antes de escrever um novo critério (ou decidir em que estágio do pipeline um critério existente deve rodar), ver `tipologia-controles.md` — a distinção entre controle computacional (pre-commit/CI, barato, determinístico) e controle inferencial (CI pós-integração, caro, baseado em julgamento) muda onde e com que frequência o critério deve ser checado.

## Critérios funcionais

1. O código executa sem erro no ambiente de referência do projeto (versão de Python e dependências fixadas).
2. Testes unitários relevantes (pytest) passam, incluindo os testes novos que a feature deveria ter introduzido.
3. Para componentes que envolvem LLM/agente: existe ao menos um caso de avaliação (eval) representativo no dataset do projeto, e ele passa dentro do critério definido (ex: similaridade semântica acima de um limiar, ou validação estrutural da saída).

## Critérios de segurança

4. O checklist de `04-guardrails-seguranca/checklist-seguranca-llm.md` foi revisado para a feature em questão.
5. Nenhum item da lista `04-guardrails-seguranca/o-que-nunca-fazer.md` foi violado.

## Critérios de aderência ao harness

6. O código segue as convenções de `03-convencoes-padroes/` (estilo, nomenclatura, estrutura).
7. Decisões de arquitetura não-triviais tomadas durante a implementação foram registradas como ADR, não apenas implementadas silenciosamente.
8. Se a implementação divergiu de alguma regra do harness, a divergência está documentada em `context/adaptacoes.md` do projeto — não apenas implementada sem registro.

## Quando marcar como concluído

Uma feature só pode ter seu status atualizado para `done` em `feature_list.json` depois que os critérios acima passarem com prova executável — nunca por inferência verbal sobre o próprio trabalho. Ver `estado-e-progresso.md` para o formato do registro de status, e `ciclo-de-sessao.md` para o momento exato dentro de uma sessão em que essa verificação deve ocorrer (etapa 3 de 4).

## O que fazer quando um critério falha

Voltar ao loop contexto → geração → avaliação: identificar se a falha indica contexto insuficiente/ambíguo (ajustar o contexto, não só o código) ou um problema pontual de implementação (ajustar o código, contexto permanece válido). Essa distinção é o que diferencia ajuste de contexto de simples correção de bug — ver o diagrama de ciclo de vida do harness.
