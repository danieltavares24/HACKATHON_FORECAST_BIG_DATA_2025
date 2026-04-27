# HACKATHON FORECAST BIG DATA 2025

# Desafio Técnico – Hackathon Forecast Big Data 2025

**Autor:** Daniel Tavares de França  
**Melhor WMAPE:** 0,671013  
**Colocação Final:** 17º lugar

---

## 1. Descrição do Desafio

O objetivo deste desafio foi desenvolver um modelo de previsão de vendas (forecast) para apoiar o varejo na reposição de produtos. A tarefa consistiu em prever a quantidade de vendas semanais por Ponto de Venda (PDV) e SKU para as cinco semanas de janeiro de 2023, utilizando o histórico de vendas de 2022.

## 2. Metodologia Aplicada

A solução foi desenvolvida utilizando um pipeline de Machine Learning em Python, com as seguintes etapas:

* **Pré-processamento:** Os dados de transações, produtos e PDVs foram unificados. Os dados diários foram agregados para um nível semanal, que é a granularidade da previsão.
* **Engenharia de Features (Feature Engineering):** Para enriquecer o modelo, foram criadas diversas features, incluindo:
    * **Features de Lag:** Vendas da semana anterior (lags de 1 a 4 semanas).
    * **Features de Janela Móvel:** Média, desvio padrão, mínimo e máximo das vendas nas últimas 4 semanas.
    * **Features de Data:** Mês, semana do ano, e flags para início/fim de mês, capturando sazonalidades.
    * **Features de Popularidade:** Média de vendas por produto e por PDV para capturar a popularidade intrínseca de cada um. *(Esta foi uma das features de maior impacto)*.
* **Modelagem:** O modelo principal utilizado foi o **LightGBM**, um algoritmo de Gradient Boosting conhecido por sua performance e velocidade.
* **Validação:** A validação do modelo foi feita de forma temporal, utilizando as últimas 5 semanas de 2022 como conjunto de teste local para simular o cenário real de previsão.
* **Otimização:** Foram realizados ajustes nos hiperparâmetros do modelo (como `learning_rate` e `num_leaves`) e no objetivo de otimização (`objective`) para melhorar a performance na métrica WMAPE.
* **Filtro de Submissão:** Para atender à restrição de 1.5 milhão de linhas da plataforma, foi aplicado um filtro para gerar previsões apenas para os produtos considerados "ativos" (que tiveram vendas nos últimos 18 dias de 2022).

## 3. Insights Técnicos e Decisões de Projeto

* **A escolha do LightGBM:** Escolhi o LightGBM pela eficiência em lidar com grandes volumes de dados (Big Data) e pela velocidade de treinamento em comparação ao XGBoost. 
* **Feature de Popularidade:** Isso ajudou o modelo a diferenciar SKUs de alto giro vs. cauda longa em PDVs distintos. 
* **Filtro de Atividade:** O filtro de "produtos ativos" não foi apenas para bater o limite de linhas, mas uma estratégia de limpeza de ruído, focando o modelo onde a probabilidade de venda era real.

## 4. Como Reproduzir o Projeto

1.  **Executar o Notebook:**
    * Abra o notebook `Hackathon_Forecast_Big_Data_2025` em um ambiente como Google Colab ou Jupyter.
    * Faça o upload dos arquivos de dados (`transacoes_2022.parquet`, `cadastro_produtos.parquet`, `cadastro_pdvs.parquet`) para o mesmo ambiente.
    * Execute todas as células em ordem. O arquivo final `submissao.parquet` será gerado.

## 5. Estrutura do Repositório

* `Hackathon_Forecast_Big_Data_2025`: Notebook principal com todo o código da solução.
* `requirements.txt`: Lista de bibliotecas Python necessárias para executar o projeto.
* `README.md`: Este documento.
