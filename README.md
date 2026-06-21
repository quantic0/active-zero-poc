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
```mermaid
graph TD
    subgraph Edge Environment [Ambiente Local / Off-Grid]
        direction TB
        A[Documento Sensível PDF/Docx] -->|Data Parsing| B(Motor de Extração Local)
        B -->|Texto Estruturado| C{LLM Local - Qwen/Llama}
        
        C -->|Prompt de Validação OFAC| D[Extração Heurística / JSON]
        
        D -->|Supervisão & Regex| E(Supervisor Determinístico)
        E -->|Validação Aprovada| F[Geração de Assinatura SHA-256]
        E -->|Validação Reprovada| G[Alerta de Risco]
        
        F -->|Timestamp RFC 3161| H[(Evidência Auditável .json)]
    end

    subgraph Internet Pública
        I[APIs de Nuvem Pública / OpenAI]
    end

    C -.->|Firewall / Air-gapped| I
    
    classDef secure fill:#0f172a,stroke:#38bdf8,stroke-width:2px,color:#e2e8f0;
    classDef danger fill:#7f1d1d,stroke:#f87171,stroke-width:2px,color:#fca5a5;
    classDef process fill:#1e293b,stroke:#64748b,stroke-width:1px,color:#f8fafc;
    
    class Edge Environment secure;
    class I danger;
    class B,C,D,E,F process;

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

