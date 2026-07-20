# Trade-offs e Decisões de Engenharia

O desenho arquitetural do **ONE** prioriza pragmatismo técnico em detrimento de convenções engessadas. Focando na modelagem analítica e em dados sensíveis, efetuamos concessões operacionais estratégicas (*Trade-offs*) essenciais para estabilidade corporativa e aceleração de desenvolvimento.

## 1. Persistência Local vs. Bancos de Dados em Nuvem
- **Abordagem:** O armazenamento (dados transacionais, perfis, logs) reside via arquivos em disco localmente na infraestrutura do usuário, e não em instâncias de BD (como AWS RDS ou Cloud SQL).
- **Concessão:** Desprovimento temporário de sincronia em rede e concorrência nativa (operações multi-device via cloud web apps).
- **Vantagem de Engenharia:** 
  - *Data Sovereignty* e *Privacy-By-Design*: Os perfis corporativos e avaliações restritas jamais trafegam na nuvem, adequando-se nativamente a requisitos pesados de compliance (GDPR/LGPD).
  - Remoção completa de latência de rede em I/O (Leitura e gravação O(1) diretamente no NVMe local).
- **Desacoplamento:** Todas as lógicas persistem em arquitetura de microsserviços internos. A futura integração de um ORM relacional no backend requererá zero modificações na camada de Frontend (React).

## 2. Inversão de Roadmap (Infraestrutura *Before* Core Logic)
- **Abordagem:** Alocação agressiva de recursos operacionais primários em observabilidade (Telemetria), segurança de rede (Fallback), e modelagem do RAG antes do aprofundamento das rotinas clínicas centrais (Testes de Avaliação).
- **Concessão:** Atraso na entrega das "features-chave" de produto em favorecimento da fundação estrutural.
- **Vantagem de Engenharia:** Mitigação contundente do risco de Falha Silenciosa (*Silent Failures*). Executar arquiteturas densas de IA Generativa sem suporte infraestrutural converte *bugs* menores em *crashes* inexplicáveis. O estabelecimento antecipado do AI Analytics e Circuit Breakers garante que todas as falhas lógicas supervenientes sejam rastreadas de imediato, facilitando *Debug* cirúrgico das futuras lógicas pesadas.

## 3. Storage Markdown vs. Vector Databases 
- **Abordagem:** Bibliotecas teóricas armazenadas em `.md` no ambiente e consumidas na íntegra via Regex Matching (*RAG Determinístico*), abstendo a camada técnica do uso de *Semantic Embeddings* e *Vector DBs* (Pinecone, ChromaDB).
- **Concessão:** Restrição na capacidade de executar buscas de linguagem difusa (procura baseada em similaridade).
- **Vantagem de Engenharia:** Proteção contra inconsistência de Chunking (retornos incompletos ou misturados). Injetar clusters completos assegura que o modelo tenha leitura restrita da teoria curada sem perder o sequenciamento narrativo vital para diagnoses técnicas. 

Este enquadramento valida o esforço arquitetural voltado para precisão cirúrgica em um contexto corporativo complexo, privilegiando segurança e previsibilidade a adoções vazias das ferramentas "de moda" (Hype Driven Development).
