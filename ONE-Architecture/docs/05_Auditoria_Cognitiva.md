# Auditoria Cognitiva e Transparência (Explainable AI)

Na implementação corporativa de Inteligência Artificial Generativa, especialmente em contextos analíticos, a natureza opaca dos LLMs (a "Caixa Preta") constitui um obstáculo regulatório e técnico. O aceite de respostas geradas requer métricas de verificabilidade da fonte de dados, coibindo a aceitação cega e mitigando o impacto de alucinações.

O ecossistema **ONE** implementa processos focados em **Inteligência Artificial Explicável (Explainable AI - XAI)**, expondo o pipeline de roteamento diretamente na Interface Gráfica.

## Funcionalidade de Mapeamento de Contexto (Context Mapping)
A aplicação refuta o isolamento do processo de *Retrieval-Augmented Generation* (RAG) no Backend. Através de eventos síncronos de comunicação interprocessos (IPC), o frontend reflete a árvore documental exata alocada na janela de contexto do LLM durante a fase processual da inferência.

### Espelhamento Técnico
No momento de uma requisição, o painel de status consolida os arquivos estáticos carregados e as respostas do [LLM-Gate](./01_RAG_Deterministico.md). O log expõe de forma direta:
- Metadados ativos no estado de Sessão.
- Bibliotecas físicas requisitadas de forma autônoma pelo motor (RAG *On-Demand*).
- Parametrizações lógicas adicionadas à camada base (Skills ativas).

## Justificativas de Design (Business & Tech Value)

### 1. Prevenção do Viés de Automação (Automation Bias)
A confiança excessiva em algoritmos compromete a avaliação crítica por operadores do sistema. A exposição transparente dos dados consultados alerta o usuário sobre eventuais *gaps* informacionais (e.g., se o modelo não acessou documentação relevante frente a uma query complexa).

### 2. Validação da Autonomia Algorítmica (Tool-Calling Audit)
A indicação visual do contexto demonstra as requisições iterativas (*Multi-pass*) exercidas pelo Roteador (Backend). Isto tangibiliza a eficácia do uso de chamadas de instrução (*Tool Calling*), atestando a capacidade do LLM de selecionar autonomamente referências adequadas perante a demanda estipulada.

### 3. Observabilidade Direta e SLA
As trilhas de auditoria garantidas pela interface se espelham nos dados remetidos à plataforma de [Telemetria e LLMOps](./03_Telemetria_e_Custos.md). Falhas analíticas relatadas pelos usuários finais podem ser reproduzidas pelo time de engenharia pela recuperação do payload *exato* de contexto injetado pelo modelo, garantindo precisão imediata em correções operacionais (Root Cause Analysis).
