# 📊 [Análise Estatística Complementar da Bola de Ouro 2019]

> **[Análise exploratória dos dados de desempenho futebolístico para comparar a classificação oficial da Bola de Ouro 2019 com um índice complementar baseado em Z-score posicional.]**

---

## 📌 Visão Geral do Projeto (Metodologia CRISP-DM)

> Este projeto aplica a metodologia CRISP-DM para construir um índice estatístico complementar à Bola de Ouro 2019, normalizando desempenho ofensivo e defensivo por posição (Z-score) com dados do FBref e do ranking oficial da France Football. Entre os 22 finalistas analisados, o índice reordena a classificação — Alexander-Arnold sobe da 19ª para a 1ª posição, e Messi e Van Dijk caem para 2º e 12º —, mas não comprova viés sistemático na votação oficial: a regressão linear testada tem baixo poder explicativo (R²=0,14) e não houve validação fora da amostra. Como aplicação prática, propõe-se a campanha "Além dos Gols", voltada à transparência metodológica e ao debate sobre como diferentes posições contribuem para o resultado coletivo.

```text
       ┌────────────────────────────────────────────────────────┐
       │                                                        │
       ▼                                                        │
┌──────────────┐      ┌──────────────┐      ┌──────────────┐    │
│ 1. Entendimento     │ 2. Entendimento     │ 3. Preparação│    │
│    do Negócio│ ───> │    dos Dados │ ───> │    dos Dados │    │
└──────────────┘      └──────────────┘      └──────────────┘    │
                                                   │            │
                                                   ▼            │
┌──────────────┐      ┌──────────────┐      ┌──────────────┐    │
│ 6. Implantação      │ 5. Avaliação │ <─── │ 4. Modelagem │    │
│   (BI e Prod)│ <─── │  (Métricas)  │      │  (Algoritmos)│    │
└──────────────┘      └──────────────┘      └──────────────┘    │
       │                                                        │
       └────────────────────────────────────────────────────────┘
```

---

## 🛠️ Ciclo de Vida do Projeto (Fases do CRISP-DM)

### 1. Entendimento do Negócio
* **Problema de Negócio:** A Bola de Ouro 2019 foi decidida por apenas 7 pontos entre Messi e Van Dijk, mas a votação é subjetiva e historicamente tende a valorizar métricas ofensivas (gols e assistências) em detrimento de contribuições defensivas, dificultando uma comparação objetiva entre jogadores de posições diferentes.
* **Objetivo:** Construir um índice estatístico exploratório (Z-score normalizado por posição, 50% ofensivo + 50% defensivo) e comparar sua ordenação com a classificação oficial dos 22 finalistas de linha da Bola de Ouro 2019 com dados reais da temporada 2018–2019.
* **Solução Proposta:** Um índice estatístico exploratório (Z-score posicional), apoiado por regressão linear e Random Forest para auditar o peso implícito de ataque e defesa na votação oficial, documentado em notebook e exportado em CSV (ranking_auditado_bola_de_ouro_2019.csv).

### 2. Entendimento dos Dados
* **Fontes:** Dados do FBref — Big 5 European Leagues Combined, temporada 2018–2019 (standard_2018_2019.csv e defensive_2018_2019.csv) — e ranking oficial da Bola de Ouro 2019 (France Football, 30 finalistas).
* **Análise Exploratória (EDA):** Identificação de distribuições, valores nulos e correlações iniciais. (Toda a análise está no notebook Projeto_CRISPDM_BolaDeOuro2019.ipynb, Fases 2 e 3.)

### 3. Preparação dos Dados
* **Limpeza e Tratamento:** Remoção de linhas de cabeçalho duplicadas, padronização do identificador único (Player_ID), conversão de colunas numéricas e preenchimento de ausentes com zero; consolidação de atletas que jogaram por dois clubes na temporada via agregação por Player_ID.
* **Engenharia de Recursos (Feature Engineering):** Cálculo de Z-score por posição (Pos_Primary) para gols, assistências, desarmes ganhos e interceptações; construção dos índices compostos Z_Ofensivo, Z_Defensivo, Z_Disciplina e Índice Equitativo (50% ofensivo + 50% defensivo).
* **Modelagem Dimensional (BI):** Não se aplica neste projeto. A base tratada foi consolidada em uma única tabela analítica (ranking_auditado_bola_de_ouro_2019.csv), pronta para consumo em apresentação ou dashboard.

### 4. Modelagem
* **Algoritmos Testados:** Regressão Linear Múltipla, Random Forest Regressor (200 árvores, profundidade máxima 3) e um índice normativo de equidade posicional (Z-score, pesos fixos 50/50).
* **Abordagem:** Os dois modelos estatísticos (regressão e Random Forest) foram ajustados e avaliados na própria amostra dos 22 finalistas, sem separação em treino/teste — resultado exploratório, não uma predição validada fora da amostra.

### 5. Avaliação
* **Métricas do Modelo:** A Regressão Linear obteve R² = 0,1403 (explica apenas ~14% da variação dos votos oficiais). O Random Forest obteve R² = 0,7125, mas ajustado e avaliado na própria amostra, sem validação cruzada — pode haver sobreajuste.
* **Métricas de BI / KPIs de Negócio:** 
  * **KPI 1 — Alcance e exposição: contas alcançadas, impressões e visualizações dos conteúdos da campanha “Além dos Gols”.
      KPI 2 — Engajamento qualificado e compreensão: compartilhamentos, salvamentos, cliques em materiais técnicos, e percentual de acerto em questionário sobre Z-score/critérios, aplicado antes e depois da campanha.

### 6. Implantação
* **Pipeline em Produção:** Não há pipeline automatizado. Recomenda-se recalcular médias e desvios-padrão por posição ao final de cada temporada europeia (maio/junho), já que a distribuição estatística muda ano a ano (data drift).
* **Camada de Visualização (BI):** A base exportada está pronta para consumo em Power BI/Looker Studio, mas o projeto entrega como aplicação prática a campanha de conteúdo “Além dos Gols” (infográficos, comparativo oficial x índice, nota técnica), detalhada no Relatório Final.
  1. Visão Executiva: comparativo ranking oficial x índice equitativo dos 22 finalistas, destacando Alexander-Arnold (19º→1º) e Van Dijk (2º→12º).
  2. Visão Operacional: tabela com os 22 finalistas, Z_Ofensivo, Z_Defensivo e Índice Equitativo, para consulta detalhada.
---

## 📁 Estrutura de Pastas (Mapeada pelo CRISP-DM)

```text
├── docs/                  # 1. Negócio: Dicionário de dados, atas e escopo do projeto
├── notebooks/             # 2. Dados: Jupyter Notebooks de Análise Exploratória (EDA)
│   ├── 1_eda/
│   └── 2_testes_modelagem/
├── src/                   # 3, 4 e 6. Scripts de Produção
│   ├── data_prep.py       # Preparação e ETL
│   ├── train_model.py     # Script de treinamento e tunagem
│   └── predict.py         # Script de inferência diária para produção
├── bi/                    # 6. Implantação: Arquivos e consultas SQL do Dashboard (.pbix, .twbx)
├── requirements.txt       # Bibliotecas e dependências do projeto
└── README.md              # Este arquivo
```
*Para abrir e editar a camada de BI, abra o arquivo localizado em `bi/[nome_do_painel].pbix` utilizando o [Power BI Desktop].*

---

## 👥 Autores e Contato

* **[Artur, Ariel, Francisco e Matheus]** 
