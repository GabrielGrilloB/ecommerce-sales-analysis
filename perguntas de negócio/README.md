# Ecommerce Sales Analysis

Análise de vendas de um e-commerce americano utilizando Power BI e modelagem em estrela. O projeto responde perguntas de negócio sobre receita por região, desempenho por categoria de produto, segmento de clientes e evolução do faturamento ao longo do tempo — simulando uma análise comercial real para apoio à tomada de decisão.

## Fonte de Dados

Dataset público do Kaggle — [Superstore Sales Dataset](https://www.kaggle.com/datasets/rohitsahoo/sales-forecasting)

## Tecnologias

- Power BI Desktop
- DAX
- Power Query (M)

## Modelagem

O dataset original foi transformado em um modelo estrela com as seguintes tabelas:

- `fVendas` — tabela fato com as transações de venda
- `DClientes` — dimensão de clientes e segmentos
- `DProdutos` — dimensão de produtos, categorias e sub-categorias
- `DLocalizacao` — dimensão de localização (cidade, estado, região)
- `DEnvio` — dimensão de modalidade de envio

## Perguntas Respondidas pelos Dados

### 1. Quais são as categorias mais vendidas?

**Resultado:** Technology lidera com ~49% da receita total, seguida por Office Supplies (22%) e Furniture (29%).

**DAX Query:**
```dax
EVALUATE
SUMMARIZECOLUMNS(
    Dprodutos[Category],
    "Receita Total", [Receita Total]
)
```

---

### 2. Quais são as regiões com maior receita?

**Resultado:** West lidera com 0,36 Bi, seguida por Central (0,32 Bi), East (0,29 Bi) e South (0,15 Bi).

**DAX Query:**
```dax
EVALUATE
SUMMARIZECOLUMNS(
    DLocalizacao[Region],
    "Receita Total", [Receita Total]
)
```

---

### 3. Qual o ticket médio por pedido?

**Resultado:** R$ 113,75 mil por pedido em média.

**Medida DAX:**
```dax
Ticket Médio = DIVIDE([Receita Total], COUNTROWS(fVendas))
```

---

### 4. Qual a média mensal de vendas?

**Resultado:** 23,22 Mi por mês.

**Medida DAX:**
```dax
Média Mensal de Vendas = 
DIVIDE(
    [Receita Total],
    DISTINCTCOUNT(fVendas[AnoMes])
)
```

---

### 5. Qual a média de dias para envio do produto?

**Resultado:** 3,96 dias em média entre o pedido e o envio.

**Medida DAX:**
```dax
Média de Dias para Envio = ROUND(AVERAGE(fVendas[Dias para Envio]), 2)
```

**Coluna calculada (Power Query):**
```m
Dias para Envio = Duration.Days([Ship Date] - [Order Date])
```

---

## Dashboard

![Dashboard](dashboard.png)
