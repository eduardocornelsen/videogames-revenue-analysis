![Optimizing Videogame Sales Strategy](images/Optimizing-Videogame-Sales-Strategy.png)

# 🎮 Otimizando a Estratégia de Vendas de Videogames para a loja Ice

Análise estratégica de dados históricos (1980-2016) para identificar drivers de sucesso e otimizar o planejamento de campanhas para 2017 na loja online "Ice".

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-blueviolet?logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-darkblue?logo=seaborn&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-white?logo=scipy&logoColor=blue)

---

## 🎯 Desafio de Negócio

A loja online **Ice** nos contratou para analisar seu banco de dados de vendas de videogames. Com dados históricos de avaliações, gêneros e plataformas, o desafio é identificar os padrões que determinam o sucesso comercial de um jogo.

O objetivo estratégico é usar essa análise (baseada em dados até Dezembro/2016) para **planejar a campanha publicitária de 2017**, otimizando a alocação de orçamento de marketing e a seleção de inventário.

## 📖 Dicionário de Dados

Após a limpeza, os dados foram estruturados da seguinte forma. A correção mais crítica foi validar que as colunas `*_sales` representam **milhões de cópias vendidas**, e não milhões de USD, uma distinção vital para a análise.

| Coluna | Descrição | Exemplo de Valor |
| :--- | :--- | :--- |
| `name` | Nome do jogo. | `'Wii Sports'` |
| `platform` | Plataforma (ex: Wii, PS2). | `'Wii'` |
| `year_of_release` | Ano de lançamento. | `2006` |
| `genre` | Gênero do jogo. | `'Sports'` |
| `na_sales` | Vendas na América do Norte (em milhões de **cópias**). | `41.36` |
| `eu_sales` | Vendas na Europa (em milhões de **cópias**). | `28.96` |
| `jp_sales` | Vendas no Japão (em milhões de **cópias**). | `3.77` |
| `other_sales` | Vendas em outros países (em milhões de **cópias**). | `8.45` |
| `critic_score` | Pontuação da crítica especializada (máximo de 100). | `76.0` |
| `user_score` | Pontuação dos usuários (máximo de 10). | `8.3` |
| `rating` | Classificação etária (ESRB). | `'E'` (Everyone) |
| `total_sales` | (Coluna criada) Soma de todas as vendas regionais. | `82.54` |

---

## 🧹 Processo de Análise e Metodologia

Para transformar dados brutos em insights acionáveis, segui um processo de 5 etapas:

1.  **Limpeza e Preparação:**
    * Padronizei os nomes das colunas para `snake_case`.
    * Corrigi tipos de dados: `user_score` (Object) foi convertido para `float64`, tratando o valor `'tbd'` (To Be Determined) como `NaN` (nulo).
    * Tratei valores ausentes: `rating` (classificação) teve `NaN`s preenchidos com `'Unknown'`, preservando dados de jogos anteriores ao sistema ESRB.
    * Removi 1 "dado fantasma" (um jogo de `DS` listado em 1985) e 4 duplicatas lógicas.

2.  **Engenharia de Features:**
    * Criei a coluna `total_sales` (soma de todas as regiões) para servir como nossa métrica primária de sucesso.

3.  **Análise Temporal (EDA):**
    * Analisei o ciclo de vida das plataformas (ver Descoberta 1) e determinei que o ciclo médio de relevância é de 10-11 anos.
    * Decidi **filtrar o dataset para 2012-2016**. Esta decisão foi crucial para remover o "ruído" estatístico de plataformas obsoletas (como PS2, Wii) e focar nas tendências da 8ª Geração (PS4, XOne), que são relevantes para prever 2017.

4.  **Análise de Mercado (EDA):**
    * Comparei a lucratividade das plataformas no período relevante (Descoberta 2).
    * Analisei a correlação entre avaliações (crítica/usuário) e vendas (Descoberta 3).
    * Identifiquei os gêneros mais lucrativos por volume total e por venda mediana.

5.  **Análise Regional e Teste de Hipóteses:**
    * Criei perfis de usuário para as regiões NA, EU e JP, identificando preferências distintas de plataformas e gêneros.
    * Usei **Testes T (Student's t-test)** para validar estatisticamente duas hipóteses centrais do negócio.

---

## 📊 Principais Descobertas (EDA)

A análise focada no período de 2012-2016 revelou drivers de mercado claros:

#### Descoberta 1: O Ciclo de Vida do Console é Finito (Aprox. 10 Anos)
A análise histórica mostrou que plataformas têm um ciclo de vida útil de 9-11 anos. Focar em plataformas no fim de seu ciclo (como o PS3 em 2016) para uma campanha de 2017 seria um erro.

| Plataforma | Ciclo de Vida Útil |
| :--- | :---: |
| `PS2` | 2000-2011 (11 anos) |
| `Wii` | 2006-2016 (10 anos) |
| `PS` | 1994-2003 (9 anos) |
| `DS` | 2004-2013 (9 anos) |

#### Descoberta 2: Plataformas Lucrativas (O Foco para 2017)
No período relevante (2012-2016), o mercado mostra uma clara transição. O PS4 estabeleceu dominância sobre o XOne. Estas são as duas únicas plataformas em crescimento ou alta estabilidade, tornando-as o foco principal para 2017.

| Platform | Sales Total (2012-2016) | Tendência |
| :--- | :--- | :---: |
| PS4 | 314.1MM | 📈 Crescimento |
| PS3 | 288.8MM | 📉 Declínio Rápido |
| X360 | 236.5MM | 📉 Declínio Rápido |
| 3DS | 194.6MM | 📉 Declínio Lento |
| XOne | 159.3MM | 📈 Crescimento |
| WiiU | 82.2MM | 📉 Obsoleta |

#### Descoberta 3: O "Efeito Metacritic" é Real (Notas da Crítica Importam)
A análise de correlação no PS4 (a plataforma líder) foi conclusiva:
* **Crítica vs. Vendas (r ≈ 0.41):** Existe uma **correlação positiva moderada**. Jogos com notas boas da crítica *tendem* a vender mais.
* **Usuário vs. Vendas (r ≈ -0.03):** A correlação é **nula**. A nota do usuário (muitas vezes afetada por "review bombing") não é um indicador confiável de vendas.

*(Insira seu gráfico de dispersão (scatterplot) de Crítica/Usuário vs. Vendas aqui)*
`[Imagem dos gráficos de dispersão (Crítica vs. Vendas) e (Usuário vs. Vendas)]`

---

## 💡 Recomendações Estratégicas para 2017

Com base nas descobertas, minhas recomendações para a campanha de 2017 da Ice são:

1.  **Foco de Inventário e Marketing:** Alocar a maior parte do orçamento de marketing e inventário para as plataformas **PS4** e **XOne**. As plataformas da 7ª Geração (PS3, X360) devem ser consideradas "mercado de liquidação" (clearance).
2.  **Seleção de Títulos (Gêneros):** Priorizar títulos de alto potencial nos gêneros **`Action`** e **`Shooter`**, que dominam tanto o volume total de vendas quanto a lucratividade mediana.
3.  **Indicador-Chave (KPI) para Publicidade:** Utilizar o **`critic_score`** (ex: notas do Metacritic) como um indicador-chave para decidir quais jogos promover. Jogos com notas acima de 80 devem receber um impulso de marketing, enquanto jogos com notas baixas devem ter o investimento reduzido. A pontuação do usuário (`user_score`) deve ser desconsiderada para fins de planejamento.
4.  **Marketing Regional:** Adaptar as campanhas regionalmente. Aumentar o marketing de RPGs (`Role-Playing`) no Japão e focar em `Action`/`Shooter` na América do Norte e Europa.

---

## 💻 Stack Tecnológica e Execução

Este projeto foi desenvolvido inteiramente em Python 3, utilizando as seguintes bibliotecas:

* **Pandas:** Para manipulação e limpeza dos dados.
* **NumPy:** Para cálculos numéricos e transformações.
* **Matplotlib & Seaborn:** Para visualização de dados (gráficos de linha, boxplots, heatmaps e scatterplots).
* **SciPy (Stats):** Para a execução dos testes T de hipóteses.

### Como Executar

1.  Clone o repositório:
    ```bash
    git clone [https://github.com/eduardocornelsen/videogames-revenue-analysis.git](https://github.com/eduardocornelsen/videogames-revenue-analysis.git)
    cd videogames-revenue-analysis
    ```
2.  Crie e ative um ambiente virtual:
    ```bash
    python -m venv venv
    source venv/bin/activate
    ```
3.  Instale as dependências:
    ```bash
    pip install pandas numpy matplotlib seaborn scipy
    ```
4.  Execute o Jupyter Notebook:
    ```bash
    jupyter notebook
    ```