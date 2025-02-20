<h1>Vendas de Loja em Geral</h1>

<hr>

<h3><a href="https://app.powerbi.com/groups/me/reports/4d69fecb-4620-4a56-98bd-af6951b47107/b9bf2d5c8026345dfcb4?experience=power-bi"> Acesse o Dashboard Aqui</a></h3>

<hr>

<b><h2>Objetivos</h2></b>

Analisar o desempenho das vendas, detalhamento de clientes e entregas de uma Loja de Vendas que vende items no geral.

A análise é voltada para os seguintes aspectos:

- O panorama geral do mercado (vendas gerais)

- Detalhes de clientes

- Desempenho e funcionalidade das entregas

<hr>

<h4><b>Essa análise abrange os seguintes aspectos:</b></h4>

<b>Visão Geral:</b> Total de vendas por anos e principais produtos.

<b>Análise por Categoria e Produto:</b> Comparação de vendas entre as diversas categorias e produtos.

<b>Análise de Clientes:</b> Desempenho dos clientes, quantidade de vendas e top clientes consumidores.

<b>Análise de Entregas</b>: Desempenho de Entregas dentro do prazo, tipo de entrega mais usada, comparação de sempenho entre entregas

<b>Evolução Temporal:</b> Tendências de vendas ao longo do tempo.

<hr>

<h4>Estruturação do Relatório</h4>

O relatório é dividido em duas páginas contendo análises específicas.

<hr>

<h2>Visão Geral de Vendas</h2>

- <b>Objetivo:</b> Apresenta um panorama geral das vendas no geral.

- Gráficos e Elementos contidos na página:

<b>Cartão</b>

Medida DAX contida no gráfico:

Exibe a Receita Total

```python
Receita Total = SUM('Payout_Discount'[total_purchase_after_discount])
```

<hr>

<b>Cartão</b>

Exibe a Quantidade de Vendas Totais

Medida DAX contida no gráfico:

```python
Quantidade De Vendas = 
COUNTROWS(Payout_Discount)
```

<hr>

<b>Cartão</b>

Exibe o ticket médio

Medida DAX contida no gráfico:

```python
Ticket Médio = 
 DIVIDE(
    [Receita Total],
    [Transações Total],
    0
)
```

<hr>

<b>Cartão</b>

Exibe a Taxa de Desconto Médio dos Produtos Vendidos

Medida DAX contida no gráfico:

```python
Taxa Média Desconto = 
DIVIDE(
    SUM(Payout_Discount[total_purchase]) - SUM(Payout_Discount[total_purchase_after_discount]),
    SUM(Payout_Discount[total_purchase]),
    0
)
```

<hr>

<b>Gráfico de Colunas Empilhadas e Linha</b>

Exibe a evolução das vendas ao longo dos anos e a Quantidade de Vendas

Medida DAX contida no gráfico:

```python
Quantidade De Vendas = 
COUNTROWS(Payout_Discount)
```

```python
Receita Total = SUM('Payout_Discount'[total_purchase_after_discount])
```

Eixo X: Hierarquia de Data

Eixo Y da coluna: Receita Total

Eixo Y da linha: Quantidade de Vendas

<hr>

<b>Gráfico de Barras Empilhadas</b>

Exibe o ticket médio das categorias dos produtos

Medida DAX contida no gráfico:

```python
Ticket Médio = 
 DIVIDE(
    [Receita Total],
    [Transações Total],
    0
)
```

Eixo  Y: product_category

Eixo X: Ticket Médio

<hr>

<b>Gráfico de Barras Empilhadas</b>

Exibe a quantidade de vendas por categoria dos produtos

Medida DAX contida no gráfico:

```python
Quantidade Vendida Por Categoria = 
SUMX(
    SUMMARIZE(
        'Payout_Discount',
        Payout_Discount[total_purchase_after_discount],          -- Agrupa por Categoria
        "QuantidadePorCategoria", SUM(Payout_Discount[total_purchase]) -- Calcula a soma por Categoria
    ),
    [QuantidadePorCategoria]
)
```

Eixo Y: product_category

Eixo X: Quantidade Vendida Por Categoria

<hr>

<b>Matriz</b>

Exibe a receita total, quantidade de vendas, taxa média de desconto e ticket médio filtrados pelo tempo (Data)

Medidas DAX contidas no gráfico:

```python
Quantidade De Vendas = 
COUNTROWS(Payout_Discount)
```

```python
Receita Total = SUM('Payout_Discount'[total_purchase_after_discount])
```

```python
Taxa Média Desconto = 
DIVIDE(
    SUM(Payout_Discount[total_purchase]) - SUM(Payout_Discount[total_purchase_after_discount]),
    SUM(Payout_Discount[total_purchase]),
    0
)
```

```python
Ticket Médio = 
 DIVIDE(
    [Receita Total],
    [Transações Total],
    0
)
```

Linhas: Data (Hierarquia de Datas)

Valores: Receita Total, Qtde De Vendas, Txa Média Desconto, Ticket Médio

<hr>

<b>Cartão</b>

Exibe a % do faturamento comparado com o ano anterior.

Medida DAX contida no gráfico:

```python
% YoY Faturamento = 
VAR vAtual = [Receita Total]
VAR vAnt   = [Faturamento LY]
RETURN
DIVIDE(vAtual - vAnt, vAnt, 0)
```

<hr>

<b>Cartão</b>

Exibe a % de vendas comparado ao ano anterior.

Medida DAX presente no gráfico:

```python
% YoY Qtd Vendas = 
VAR vAtual = [Quantidade De Vendas]
VAR vAnt   = CALCULATE([Quantidade De Vendas], SAMEPERIODLASTYEAR(dCalendario[Data]))
RETURN
DIVIDE(vAtual - vAnt, vAnt, 0)
```

<hr>

<b>Cartão</b>

Exibe a % do ticket médio comparado ao ano anterior.

Medida DAX contida no gráfico:

```python
% YoY Ticket Médio = 
VAR vAtual = [Ticket Médio]
VAR vAnt   = CALCULATE([Ticket Médio], SAMEPERIODLASTYEAR(dCalendario[Data]))
RETURN
DIVIDE(vAtual - vAnt, vAnt, 0)
```

<hr>

<b>Cartão</b>

Exibe a % da taxa média de desconto comparado ao ano anterior.

Medida DAX presente no gráfico:

```python
% YoY Taxa Média = 
VAR vAtual = [Taxa Média Desconto]
VAR vAnt   = CALCULATE([Taxa Média Desconto], SAMEPERIODLASTYEAR(dCalendario[Data]))
RETURN
DIVIDE(vAtual - vAnt, vAnt, 0)
```

<hr>

<h2>Detalhes de Clientes</h2>

