# Active Zero: Edge AI & Cryptographic Traceability Engine (PoC)

## 📌 Visão Geral (Overview)
O **Active Zero** é um projeto independente de pesquisa aplicada (P&D) focado em **Arquitetura de Soluções** e **Edge Computing**. O objetivo principal é resolver um dilema crítico do mercado de compliance: Como auditar documentos sensíveis através de Inteligência Artificial sem expor segredos industriais em APIs de nuvem pública?

Este repositório documenta a viabilidade técnica de uma arquitetura 100% *off-grid* (air-gapped), utilizando LLMs locais combinados com mecanismos de trilha de auditoria criptográfica.

## 🏗️ Arquitetura do Sistema (System Architecture)
A arquitetura foi projetada sob o princípio de **Zero Data Leak**. O processamento é dividido em 3 camadas:
1. **Ingestão:** Extração de texto de PDFs complexos otimizada para borda.
2. **Motor de Decisão (Edge LLM):** Execução de modelos quantizados locais isolados da internet.
3. **Cadeia de Custódia:** Geração de hash SHA-256 amarrado à decisão e ao timestamp (RFC 3161). 

## 🔄 Fluxo de Dados (Pipeline)

```mermaid
graph TD
    subgraph Edge[Ambiente Local Off-Grid]
        direction TB
        A[Documento PDF] --> B(Extracao Local)
        B --> C{LLM Quantizado}
        C --> D[Extracao JSON]
        D --> E(Motor Determinista Python)
        E --> F[Assinatura Criptografica]
        F --> G[(Evidencia e Hash)]
    end

    subgraph Nuvem[Internet Publica]
        H[APIs Externas]
    end

    C -.-x|Acesso Bloqueado| H

