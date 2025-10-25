# 🧠 Pipeline de Dados – Análise Comercial | Xperium “Profissional do Futuro”

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-005CA9?logo=microsoft&logoColor=white)](https://learn.microsoft.com/en-us/dax/)
[![ETL](https://img.shields.io/badge/ETL%20Process-4C9A2A?logo=databricks&logoColor=white)](#)
[![Machine Learning](https://img.shields.io/badge/ML.NET-68217A?logo=microsoft&logoColor=white)](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet)
[![Xperium](https://img.shields.io/badge/Xperium-E03C31?logoColor=white)](#)

> Projeto desenvolvido como parte do curso **Profissional do Futuro** da **Xperium**, com o objetivo de construir uma **pipeline de dados completa (ETL + Modelagem + Cálculos + Visualização)** para análise de desempenho comercial e margem bruta.

---

## 📸 Prévia do Projeto

| **Desempenho Comercial** | **Principais Influenciadores** | **Tooltip Personalizada** |
|---------------------------|--------------------------------|----------------------------|
|<img src="https://github.com/Cesar-AlmeidaLima/PBIDist/blob/main/Imagens/Captura de tela 2025-10-25 120912.png?raw=true"/> | <img src="https://github.com/Cesar-AlmeidaLima/PBIDist/blob/main/Imagens/Captura de tela 2025-10-25 121053.png?raw=true"/> | <img src="https://github.com/Cesar-AlmeidaLima/PBIDist/blob/main/Imagens/Captura de tela 2025-10-25 131805.png?raw=true"/>|

> 💡 As imagens acima ilustram as páginas interativas criadas no Power BI Desktop.

---

## 1️⃣ Estrutura da Pipeline

### 🔹 **Extração**
Exportação do dataset bruto disponibilizado pela Xperium, garantindo versionamento e integridade dos dados para as etapas seguintes.

---

## 2️⃣ **Transformação (ETL)**

Processos realizados no **Power Query**:

- ✅ Validação e correção de **tipos de dados** (datas, números, texto)  
- 🔄 **Mesclagem e limpeza** de colunas redundantes  
- 🧹 Remoção de colunas calculadas (`Custo Total`, `Faturamento Total`) — refatoradas em DAX para maior consistência e rastreabilidade  

---

## 3️⃣ **Modelagem de Dados**

#### 📅 **Tabela Calendário**

```DAX
Calendario = CALENDARAUTO()

```
## 🔹 Colunas Adicionais

- Ano  
- Mês  
- Dia  
- Mês Abreviado (classificado por mês)

## 🔗 Relacionamentos

- **1:N** entre **Calendario** e **Vendas**  
  Cada data é única na tabela **Calendario**, mas pode se repetir em **Vendas**.

## 4️⃣ Medidas DAX

| 🧾 Medida               | 💬 Descrição                        | 🎯 Importância                          |
|-------------------------|------------------------------------|----------------------------------------|
| Quantidade de Itens     | Volume total de itens vendidos     | Mede demanda agregada                   |
| Quantidade de Vendas    | Distinct count de notas fiscais    | Garante precisão transacional          |
| Faturamento             | Preço Unitário × Quantidade        | Mede receita bruta real                 |
| Custo                   | Custo Unitário × Quantidade        | Base da rentabilidade                   |
| Margem Bruta            | Faturamento - Custo                | Lucro operacional bruto                 |
| % Margem Bruta          | Margem Bruta / Faturamento         | Normaliza lucratividade                 |
| Ticket Médio            | Faturamento / Quantidade de Vendas | Indica comportamento de compra         |

## 5️⃣ Regras de Negócio e Cores Condicionais

### 🧾 Classificação de Margem
```DAX
MargemBruta = 
SWITCH(
    TRUE(),
    SELECTEDVALUE(Vendas[Linha Produto]) = "Bebidas" && [06. Margem Bruta %] >= 0.18, "acima da média",
    SELECTEDVALUE(Vendas[Linha Produto]) = "Alimentos" && [06. Margem Bruta %] >= 0.14, "acima da média",
    ISBLANK(SELECTEDVALUE(Vendas[Linha Produto])) && [06. Margem Bruta %] >= 0.14, "acima da média",
    "abaixo da média"
)

```

### 🎨 Cores Condicionais (Hexadecimal)
```DAX
Cor Margem = 
SWITCH(
    TRUE(),
    SELECTEDVALUE(Vendas[Linha Produto]) = "Bebidas" && [05. Margem Bruta %] >= 0.18, "#A9E9A0", -- Verde
    SELECTEDVALUE(Vendas[Linha Produto]) = "Alimentos" && [05. Margem Bruta %] >= 0.14, "#A9E9A0", -- Verde
    ISBLANK(SELECTEDVALUE(Vendas[Linha Produto])) && [05. Margem Bruta %] >= 0.14, "#A9E9A0", -- Verde
    "#FB8484" -- Vermelho
)

```
Essa regra colore automaticamente os valores abaixo das metas de margem (18% para bebidas e 14% para alimentos) em vermelho, destacando produtos de baixa rentabilidade.

## 6️⃣ Estrutura dos Dashboards
### 🔹 Desempenho Comercial
#### Principais visuais:
- 💰 Cartões de destaque para Faturamento e Margem Bruta
- 📈 Gráfico de linha: evolução mensal do faturamento
- 🍩 Gráfico de rosca: quantidade de vendas por linha de produto
- 📊 Gráfico de barras: quantidade por grupo de produto
- 🧑‍💼 Tabela de ranking dos vendedores com coloração condicional de margem

### 🔹 Principais Influenciadores
Uso do visual Key Influencers do Power BI para identificar fatores que mais impactam a margem bruta, baseado em algoritmos interpretáveis de Machine Learning (ML.NET).

O visual analisa padrões estatísticos e calcula a probabilidade de um fator (ex: “Linha Produto = Bebidas”) influenciar uma métrica (ex: “Margem acima da média”).

### 🔹 Tooltip Personalizada
- Quantidade de Vendas
- Margem Bruta %
- Grupo de Produto
- Imagem do Produto
> Integrada ao gráfico de “Quantidade por Grupo de Produto”.

## 7️⃣ Design & Experiência
- Interface baseada no layout original da Xperium
- Paleta azul-escura moderna com contraste otimizado para KPIs
- Tooltips e ícones contextuais para navegação fluida
- Layout limpo e responsivo, inspirado em painéis executivos corporativos

## 8️⃣ Tecnologias Utilizadas

| Categoria               | Ferramenta / Tecnologia       |
|-------------------------|-------------------------------|
| ETL                     | Power Query                   |
| Modelagem de Dados      | Power BI Desktop              |
| Linguagem de Cálculo    | DAX                           |
| IA / Machine Learning   | ML.NET (Key Influencers)      |
| Fonte de Dados          | Excel                         |
| Documentação            | Markdown + GitHub             |


## 9️⃣ Resultados Obtidos
✅ Painel consolidado com análise de desempenho comercial por produto, grupo e vendedor

✅ Identificação de margens abaixo da meta via cores condicionais

✅ Insights automáticos sobre fatores que mais influenciam a margem

✅ Visual moderno, ideal para tomada de decisão executiva

## 🔹 Créditos

| Tipo                        | Responsável                       |
|-----------------------------|----------------------------------|
| Dataset e Design Original   | Xperium – Curso Profissional do Futuro |
| Modelagem, DAX, Documentação | @Cesar-AlmeidaLima                    |


