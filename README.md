# Nexa Crédito | Pipeline Analítico & Dashboard Executivo

Este projeto apresenta a consolidação e engenharia de dados da operação de crédito da fintech **Nexa Crédito**, integrando fluxos de propostas, concessão, cobrança e pagamentos em uma camada analítica única e confiável.

---

## 📊 Dashboard Executivo (Power BI)

![Visão Geral da Operação](images/dashboard.png)

> **Arquivo do Power BI:** Disponível em [`power_bi/Nexa_Dash.pbix`](power_bi/Nexa_Dash.pbix)  
> **Versão em PDF:** [`power_bi/Nexa_Dash.pdf`](power_bi/Nexa_Dash.pdf)

---

## 🏗️ Arquitetura e Engenharia de Dados

Para evitar distorções volumétricas e duplicidade em cálculos financeiros (gerados pela junção direta de propostas e parcelas múltiplas), os dados foram estruturados em duas tabelas consolidadas (One Big Tables - OBTs):

1. **`obt_funil_originacao` (Grão: Proposta / Contrato):**
   * Consolida cadastro de clientes, dados da proposta e dados de efetivação do contrato.
   * Permite avaliar volumetria de entrada, taxas de conversão e crédito concedido por canal e segmento.
2. **`obt_carteira_cobranca` (Grão: Parcela / Evento de Pagamento):**
   * Consolida fluxo financeiro, vencimentos, valores amortizados, multas, status de adimplência e score de crédito.
   * Permite medir a liquidez da carteira e a taxa de inadimplência.

### Tratamentos Aplicados:
* **Conversão de Tipos:** Cast de campos monetários (`valor_solicitado`, `valor_pago`) que estavam armazenados como string.
* **Padronização de Status:** Tratamento com `TRIM` e `UPPER` nos status de parcela para mitigar inconsistências textuais (`EM_ATRASO`, `PAGA`).
* **Preservação de Integridade:** Uso de junções controladas (`LEFT JOIN`) e tratamento de nulos (`COALESCE`) para garantir que propostas sem contrato ou parcelas em aberto fossem computadas corretamente.

---

## 💡 Principais Insights de Negócio

* **Dominância do Canal Mobile:** O aplicativo concentra **44,7%** do volume total de propostas (407 de 910), comprovando tração orgânica no canal digital e justificando a priorização de investimentos em onboarding mobile.
* **Composição de Carteira:** Os segmentos **Premium** (R$ 1,59 Mi) e **PME** (R$ 1,53 Mi) respondem pela maior fatia do crédito concedido, indicando oportunidade para linhas de crédito rotativo ou capital de giro com tickets maiores.
* **Eficácia do Modelo de Risco:** A taxa de inadimplência decresce monotonicamente à medida que o score melhora (**19,25% no Alto Risco** $\rightarrow$ **13,88% no Médio** $\rightarrow$ **9,84% no Baixo**), validando a precisão preditiva do algoritmo de concessão.

---

## 📂 Estrutura do Repositório

```text
├── notebooks/       # Código executado no Google Colab (PySpark/SQL)
├── power_bi/        # Arquivo .pbix e versão em PDF do dashboard
├── data/            # Arquivos analíticos gerados (.parquet)
├── images/          # Capturas de tela do relatório
└── README.md        # Documentação da solução
```
