# Orquestração Dinâmica de Prompt e Macro-RAG

A dependência exclusiva de System Prompts monolíticos denota falta de escalabilidade em sistemas de Inteligência Artificial. Prompts extensivos não apenas ampliam progressivamente os custos (*Token Usage*) como induzem o modelo à perda de contexto periférico (*Lost in the Middle*).

O **ONE Architecture** contorna tais limites operacionalizando o Prompt como um *Estado Compilado Dinamicamente*, executando injeções sob demanda (*Dynamic Persona Swapping* e *Context Hydration*).

## 1. Modularidade Operacional (Persona Swapping)
De forma a acomodar lógicas analíticas multivariadas (especificidades de perfis, nuances patológicas ou abordagens de consultoria de RH), o modelo emprega *Skills* (módulos lógicos de *Instruction-Tuning* em *runtime*).

- **Gestão Tática:** A interface de administração gerencia módulos `.md` segregados portando escopos restritos.
- **Compilação Assíncrona:** A injeção no System Prompt base opera exclusivamente durante a requisição (*callLLMChat*). Modificações no tom e foco analítico prescindem da execução de operações intensivas (Fine-tuning de pesos) e de manutenções com *downtime*.

## 2. Hidratação de Contexto Amplo (Macro-RAG / Stateful Prompting)
Ao contrário do RAG padrão ([Focado em Base de Conhecimento](./01_RAG_Deterministico.md)), o *Context Hydration* lida com o repasse informacional Transacional (Banco de Dados).

Quando um fluxo analítico abrangente é instanciado na camada Global, o roteador abdica da pesquisa vetorial no DB e realiza:
1. Uma varredura no armazenamento em disco (Serialização Local de `db.json`).
2. Transposição relacional dos dados transacionais (Instâncias corporativas, perfis vinculados e avaliações) para uma sintaxe *LLM-Readable*.
3. Anexação integral ao System Prompt.

### Valor Produtivo e Desacoplamento (Stateful Prompting)
Esta metodologia de pré-carga viabiliza inferências de Big Data de alta complexidade (Ex: "*Extrair interseções conflitantes entre colaboradores C-Level e coordenadores do departamento X*") a partir de queries em texto livre. A formatação condicional assegura a consistência referencial requerida por tarefas do domínio de Data Science sem escalar clusters pesados de infraestrutura transacional acoplados a motores de linguagem, validando escalonamento inteligente (Token-Aware Engineering).
