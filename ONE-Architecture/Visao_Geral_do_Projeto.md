# Visão Geral da Arquitetura: Projeto ONE

O **ONE** é um sistema corporativo (B2B) focado em avaliações psicológicas, tipológicas e mapeamento organizacional. A aplicação atua como um assistente especialista, integrando testes de perfil, geração de dossiês e análise de dinâmicas de equipe através do uso orquestrado de Inteligência Artificial Generativa.

O sistema foi arquitetado visando **precisão clínica, alta disponibilidade e segurança de dados**. Este documento consolida o fluxo lógico da aplicação e a interação entre os componentes do ecossistema.

---

## Ciclo de Vida da Requisição (End-to-End Flow)

Para ilustrar a arquitetura, detalhamos o fluxo de processamento de uma requisição complexa. Exemplo de *Input* de um consultor na plataforma: *"Avalie a interação projetada entre o Perfil A (Gestor) e as características do Tipo 8w7."*

Durante o ciclo de vida desta mensagem, os seguintes módulos são acionados:

### 1. Isolamento e Segurança (BFF e Zero-Trust)
A camada de apresentação (React) transmite a requisição estritamente via comunicação interprocessos (IPC) para a camada de serviço (Node.js/Electron Main). A interface gráfica atua de forma "burra" (dumb component), sem deter regras de negócio ou chaves de API. De acordo com o princípio *Zero-Trust*, os dados transacionais permanecem armazenados localmente, assegurando aderência a normas de privacidade ([ver Governança e BFF](./docs/07_Seguranca_e_Arquitetura_BFF.md)).

### 2. Hidratação de Contexto Global (Macro-RAG)
Antes da construção do Payload HTTP, o backend acessa a base local, identificando os dados transacionais relevantes ("O Perfil A é associado ao cargo X na empresa Y"). Estes metadados estruturados são serializados e injetados diretamente no *System Prompt*. Este processo ([Context Hydration](./docs/06_Prompt_Orchestration.md)) viabiliza a execução de lógicas analíticas sobre a base de dados sem exigir rotinas pesadas de indexação vetorial.

### 3. Extração e Roteamento (RAG Determinístico e Multi-Hop)
Concorrentemente, uma rotina de extração de entidades (Regex) avalia a requisição e identifica referências técnicas (ex: "Tipo 8w7"). O backend fornece ao LLM um mapeamento do inventário de arquivos disponíveis localmente. Se o modelo requerer embasamento, ele devolve uma chamada estruturada (`###BUSCAR:8w7.md###`). O backend resolve essa chamada recuperando os arquivos na íntegra. Esta delegação lógica previne a degradação de contexto típica de buscas vetoriais parciais ([RAG Determinístico](./docs/01_RAG_Deterministico.md) e [Multi-Hop](./docs/08_Multi_Hop_RAG_e_Offline_LLMs.md)).

### 4. Transparência Algorítmica (Explainable AI)
A confirmação das operações de RAG (os arquivos acessados "sob demanda") é espelhada instantaneamente na interface do usuário através de flags visuais. Este princípio de [Auditoria Cognitiva](./docs/05_Auditoria_Cognitiva.md) consolida a confiabilidade clínica do sistema, permitindo que o operador valide a fonte teórica que embase a resposta.

### 5. Resiliência de API (Circuit Breaker)
Caso o provedor primário sofra uma instabilidade de rede (Time Out) ou rejeite a chamada por Rate Limit (HTTP 429), a requisição é interceptada pelo [Motor de Fallback](./docs/02_Sistema_Fallback.md). A aplicação permuta automaticamente a configuração de roteamento e o modelo alvo, repassando o Payload para o provedor secundário de forma imperceptível na camada de negócios, mantendo o *Uptime* funcional.

### 6. Observabilidade e LLMOps
Na resolução bem-sucedida, o middleware final processa as métricas da transação (Tokens In/Out, Latência, Provedor e Contexto RAG) e registra as informações na base de log local. Estes dados alimentam o [AI Analytics](./docs/03_Telemetria_e_Custos.md), fornecendo ao gestor uma visão tática sobre o custo por operação e facilitando sessões de debug em casos de geração imprecisa.

---

## Síntese
A arquitetura do ONE consolida padrões de desenvolvimento focados em sistemas corporativos de IA. O ecossistema é projetado de forma modular, permitindo inclusive o desligamento da camada em nuvem para operação estritamente local (Offline-Readiness), atestando flexibilidade estrutural para cenários de alta regulação e escalabilidade futura.
