# 🚢 Estudo de Caso 1: O Bunker Marítimo (Compliance OFAC Offline)

## 📌 O Cenário e o Problema de Negócio
Embarcações comerciais de grande porte operam rotineiramente em águas internacionais, lidando com milhares de manifestos de carga (*Bills of Lading*). Antes de atracar em portos globais, a tripulação precisa garantir que os destinatários e emissores da carga não constam em listas de sanções internacionais (como a **OFAC** - *Office of Foreign Assets Control*).

**As restrições críticas do ambiente marítimo:**
1. **Conectividade:** A internet via satélite em alto mar é cara, instável e possui alta latência.
2. **Segurança de Dados:** Enviar manifestos de carga inteiros para APIs de IA em nuvem pública (ex: OpenAI) expõe o navio a espionagem industrial (vazamento de rotas, valores e clientes).

## 🛠️ A Solução: Active Zero na Borda (Edge)
O **Active Zero** foi aplicado como um motor de processamento local (Edge AI), isolado em um servidor *air-gapped* dentro da embarcação.

* **Ingestão Offline:** O manifesto de carga (PDF) é processado localmente no navio, sem qualquer conexão com a internet externa.
* **Extração via LLM Quantizado:** Um modelo de linguagem (ex: Qwen 4-bit) extrai as entidades relevantes (Comprador, Vendedor, Porto de Origem) processando a linguagem natural do documento, mesmo que seja não-estruturado.
* **Validação Cruzada:** O motor Python cruza os nomes extraídos com o banco de dados da OFAC (baixado previamente no porto).

## 🔐 Cadeia de Custódia e Evidência (O Grande Diferencial)
Para provar às autoridades portuárias que a auditoria foi feita de forma correta e no tempo certo, o **Supervisor Determinístico** entra em ação:
1. Gera um JSON com a decisão da IA (Aprovado / Alerta de Sanção).
2. Cria um **Hash SHA-256** atrelando o PDF original, a decisão do LLM e o *Timestamp* (baseado no conceito RFC 3161).

## 📊 Resultados e Valor Entregue
* **Zero Data Leak:** O documento sensível nunca saiu do servidor do navio.
* **Agilidade Operacional:** A validação que levava horas de trabalho manual da tripulação foi reduzida para segundos de processamento em lote.
* **Defesa Regulatória:** Ao atracar, a embarcação transmite apenas o *Hash Criptográfico* e o laudo JSON para a matriz, comprovando matematicamente que a due diligence foi executada.

