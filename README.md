# Modelagem e Transformação de dados com DAX no Power BI

## Instruções de Entrega do Desafio

### Descrição do desafio

Utilizaremos a tabela única de Financial Sample para criar as tabelas dimensão e fato do nosso modelo baseado em star schema.
O processo consiste na criação das tabelas com base na tabela original. A partir da cópia serão selecionadas as colunas que irão compor a visão da nova tabela. Sendo assim, a partir da tabela principal serão criadas as tabelas:
Financials_origem (modo oculto – backup)

Foram criadas 7 tabelas (1 Fato e 5 Dimensão além da tabela de calendário):

### F_Vendas (Fato)

    - id_venda
    - id_produto
    - id_categoria
    - units Sold
    - Sale Price
    - Sales
    - Profit
    - Date

### D_Produtos (Dimensão)

    - id_produto
    - Products
    - Contagem
    - Valor mínimo de venda
    - Valor máximo de venda
    - Media do valor de venda
    - Mediana
    - Media da manufatura

### D_Categoria (Dimensão)

    - id_categoria
    - Segment
    - Country
    - Categoria

### D_Produtos_Detalhes (Dimensão)

    - id_venda
    - Sale Price
    - Units Sold
    - Manufacturing Price

### D_Detalhes (Dimensão)

    - id_venda
    - Gross Sales
    - Sales
    - COGS

### D_Descontos (Dimensão)

    - id_venda
    - Discount Band
    - Discounts

O relacionamento entre a tabela Fato e Dimensão se dá da seguinte forma (no modelo baseado em star schema):

- F_Vendas x D_Produtos (id_produto)
- F_Vendas x D_Categoria (id_categoria)
- F_Vendas x D_Produtos_Detalhes (id_venda)
- F_Vendas x D_Detalhes (id_venda)
- F_Vendas x D_Descontos (id_venda)
- F_Vendas x Calendar (Date)

## Etapas no processo de modelagem das tabelas

A partir da tabela Financial Sample criei a tabela **DD_Produto** (com opção de não habilitar a carga)

- retirei todas as colunas exceto a coluna Product;
- retirei toda a duplicidade da coluna Product;
- Criei um índice a partir da sequencia 1

A partir da tabela Financial Sample criei a tabela **D_Produto** conforme instrução de agrupamento das informações

- Através da função "Mesclar Consulta" acrescentei o índice (id_produto) criado na tabela DD_Produto.

A partir da tabela Financial Sample criei a tabela **D_Categoria**

- retirei todas as colunas exceto as colunas Segment e Country;
- retirei toda a duplicidade das colunas Segment e Country;
- dupliquei as colunas Segment e Country;
- Com a função "Mesclar Colunas", concatenei as colunas duplicadas Segment e Country separadas com "-"
- Criei um índice a partir da sequencia 1

A partir da tabela Financial Sample criei a tabela **F_Vendas**

- Criei um índice (id_venda) a partir da sequencia 1
- a partir da tabela F_Vendas, foram criadas as tabelas **D_Produtos_Detalhes**, **D_Detalhes** e **D_Descontos**;
- Através da função "Mesclar Consulta" acrescentei o índice (id_produto) criado na tabela DD_Produto.
- Com a função "Mesclar Colunas", concatenei as colunas Segment e Country separadas com "-"
- Através da função "Mesclar Consulta" acrescentei o índice (id_categoria) criado na tabela D_Categoria.
- retirei todas as colunas exceto as colunas Units Sold, Sale Price, Sales, Profit e Date.

Tabela D_Produtos_Detalhes

- retirei todas as colunas exceto as colunas id_venda, Sale Price, Units Sold e Manufaturing Price.

Tabela D_Detalhes

- retirei todas as colunas exceto as colunas id_venda, Gros Sales, Sales, COGS.

Tabela D_Descontos

- retirei todas as colunas exceto as colunas id_venda, Discount Band e Discounts.

A tabela de calendário (Calendar) foi criada através do DAX.

Calendar =
ADDCOLUMNS(

-          CALENDARAUTO(),
              "Ano", YEAR([Date]),
              "Mes 1", MONTH([Date]),
              "Mes 2", FORMAT([Date], "mmmm"),
              "Mes 3", FORMAT([Date], "mmm"),
              "Dia semana 1", WEEKDAY([Date]),
              "Dia semana 2", FORMAT([Date], "DDDD"),
              "Dia semana 3", FORMAT([Date], "DDD"),
              "Trimestre", "T"&QUARTER([Date]),
              "Semestre", IF(MONTH([Date]) <= 6, "S1", "S2")
  )

Obs.: Para exercitar foram criadas colunas com informações meses (completo e abreviado), dias da semana (completo e abreviado), informação de Trimestre e Semestre.
