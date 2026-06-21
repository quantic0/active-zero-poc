# Active Zero: Edge AI & Cryptographic Traceability Engine (V2)

## 🏗️ Arquitetura V2: Pipeline de Produção
O Active Zero V2 introduz o conceito de **Gating de Qualidade**, garantindo que nenhuma decisão de IA seja finalizada sem validação determinística e score de confiança.

## 🔄 Fluxo de Dados (Pipeline)

```mermaid
graph TD
    subgraph Edge[Ambiente Local - Produção]
        direction TB
        A[Documento PDF] --> B{Validacao OCR}
        B -->|OCR Necessario| C[OCR Fallback/Tesseract]
        C --> D[Parser Estruturado]
        B -->|Texto Limpo| D
        D --> E[LLM + Confidence Score]
        E -->|Confianca Alta| F[Motor Determinista]
        E -->|Confianca Baixa| G[Exception Queue]
        F --> H[Assinatura Ed25519]
        H --> I[(Evidencia Final)]
    end
    
    style Edge fill:#0f172a,stroke:#38bdf8,stroke-width:2px
    style G fill:#1e293b,stroke:#22c55e,stroke-width:2px
    style G fill:#7f1d1d,stroke:#fca5a5,stroke-width:2px

