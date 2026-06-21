# Active Zero: Edge AI & Cryptographic Traceability Engine (PoC)

## 📌 Visão Geral (Overview)
O **Active Zero** é um projeto independente de pesquisa aplicada (P&D) focado em **Arquitetura de Soluções** e **Edge Computing**. O objetivo principal é resolver um dilema crítico do mercado de compliance: **Como auditar documentos sensíveis através de Inteligência Artificial sem expor segredos industriais em APIs de nuvem pública?**

Este repositório documenta a viabilidade técnica de uma arquitetura 100% *off-grid* (air-gapped), utilizando LLMs locais combinados com mecanismos de trilha de auditoria criptográfica.

## 🏗️ Arquitetura do Sistema (System Architecture)

A arquitetura foi projetada sob o princípio de **Zero Data Leak** via infraestrutura externa. O processamento é dividido em 3 camadas locais:

1. **Ingestão (Data Parsing):** Extração de texto de documentos PDF complexos de forma otimizada para ambientes com restrição de memória.
2. **Motor de Decisão (Edge LLM):** Execução de modelos de linguagem quantizados (ex: Qwen via Ollama) diretamente na borda, isolados da internet pública.
3. **Cadeia de Custódia (Cryptographic Hashing):** Após a decisão heurística do LLM, um "Supervisor Determinístico" gera um hash SHA-256 amarrado à decisão e ao timestamp (baseado no conceito RFC 3161). 

## 🔄 Fluxo de Dados (Pipeline)

[ Documento Sensível (PDF) ] 
       │
       ▼ (Extração Local)
[ Camada de Texto ] 
       │
       ▼ (Prompt de Validação de Regras de Negócio)
[ Edge LLM (Quantizado em 4-bit) ] ---> [ Sem Acesso à Internet ]
       │
       ▼ (Extração de JSON e Decisão Heurística)
[ Supervisor Determinístico (Python) ]
       │
       ▼ (Geração de Assinatura + Timestamp)
[ Arquivo de Evidência Auditável (.JSON / Hash) ]

## 🛠️ Stack Tecnológico Utilizado
* **Linguagem Base:** Python 3
* **Edge AI:** Ollama, Llama.cpp, Modelos Quantizados (GGUF).
* **Processamento Local:** PyPDF, Regex.
* **Rastreabilidade:** Bibliotecas de Hash Criptográfico (SHA-256).

## 📊 Resultados da Pesquisa & Eficiência Operacional
Durante os testes de estresse documentais, a arquitetura provou a viabilidade de:
- **Redução de Exposição:** 100% de retenção dos dados dentro da infraestrutura local.
- **Eficiência:** Redução de processos de triagem documental complexa de ~45 minutos para execução em segundos (Batch Processing).
- **Provas Matemáticas:** Geração de trilhas de auditoria para áreas de Compliance sem necessidade de expor o documento original.

## 👨‍💻 Sobre o Autor
Desenvolvido por **Aderlan**.
Projeto construído como evidência prática de **Arquitetura de Soluções, Integração de IA e Compliance Tecnológico**.

