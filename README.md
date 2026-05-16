# 📊 AniList Top 100 Anime Insights & Cluster Personas

Este repositório contém um pipeline completo de Engenharia de Dados, Análise Exploratória (EDA), Machine Learning e Inteligência Artificial Generativa focado na análise estatística e comportamental dos 100 melhores animes da plataforma **AniList**. O projeto extrai dados diretamente da API GraphQL, executa sanitização avançada de dados textuais e estruturados, implementa agrupamento não supervisionado inteligente com validação métrica e faz a integração com o modelo **Gemini 2.5 Flash** para gerar relatórios automatizados de personas de consumo.

---

## 🚀 Arquitetura do Pipeline

O ecossistema do script está estruturado sob o princípio de pipelines modulares, dividido de forma sequencial:

```
[ API AniList ] ➜ [ Data Cleaning & Parsing ] ➜ [ Análise Exploratória (EDA) ]
                                                            ⬇
[ Relatório de Personas GenAI ] ⬅ [ Otimização e Clustering K-Means ]
```

1. **Ingestão e Coleta (GraphQL API):** Mapeamento automatizado através de requisições `requests` para coletar dados ricos sobre títulos (romaji, inglês), score médio, contagem de episódios, popularidade, gêneros primários/secundários, estúdios de animação principais e corpos textuais de avaliações (*reviews*) enviadas por utilizadores.
2. **Processamento e Higienização de Texto:** Remoção cirúrgica de tags HTML persistentes, sintaxes parciais de Markdown e caracteres de escape inválidos nos resumos e críticas através de Expressões Regulares (`re`) e análise estrutural com a biblioteca `BeautifulSoup`.
3. **Engenharia de Recursos & EDA:** Criação de métricas de suporte (como comprimento de texto de reviews) e mapeamento visual de comportamentos de mercado usando `Matplotlib`, `Seaborn` e gráficos interativos em `Plotly`.
4. **Machine Learning Avançado (Clustering):** Normalização paramétrica das variáveis de escala e clusterização com o algoritmo **K-Means**. O script calcula dinamicamente o número ótimo de agrupamentos via **Método do Cotovelo (WCSS)** cruzado com o **Silhouette Score**.
5. **Enriquecimento com IA Generativa (LLM):** Utilização do novo SDK da Google (`google-genai`) para instanciar o modelo `gemini-2.5-flash`. O modelo recebe um prompt altamente contextualizado contendo as características do cluster e amostras reais de críticas textuais para mapear o perfil mercadológico (*Persona*) de cada grupo.

---

## 📊 Funcionalidades e Visualizações Estatísticas

O script implementa cinco análises visuais avançadas para extrair valor dos dados coletados:

* **Gráfico de Haltere (Dumbbell Plot) - Joias Escondidas vs. Blockbusters:** Confronta diretamente a nota média (`averageScore`) contra o volume de popularidade (`popularity`). Permite isolar animes hiper-aclamados pela crítica especializada, mas que permanecem desconhecidos ou de nicho para o grande público.
* **Gráfico de Crista (Ridge Plot) - Evolução de Scores por Década:** Uma visualização de densidade facetada por épocas históricas do entretenimento japonês. Demonstra graficamente se as distribuições de notas estão a sofrer inflação ou se o padrão de qualidade percebido mudou nas últimas décadas.
* **Matriz de Coocorrência de Gêneros:** Um mapa de calor baseado em frequências emparelhadas que revela quais as misturas temáticas mais exploradas e bem-sucedidas pela indústria (ex: a forte correlação entre *Ação*, *Fantasia* e *Drama*).
* **Treemap Interativo de Domínio de Estúdios:** Desenvolvido com `Plotly`, este gráfico dinâmico permite navegar de forma hierárquica pelo volume de obras consagradas de estúdios líderes do mercado internacional (como *MAPPA, Wit Studio, Madhouse, ufotable*), cruzando com os seus géneros predominantes.
* **Performance por Temporada de Lançamento:** Agrupa e cruza dados temporais para aferir se os lançamentos nas épocas tradicionais (*Winter, Spring, Summer, Autumn*) possuem variações significativas de audiência acumulada ou classificação crítica.

---

## 🤖 Machine Learning Dinâmico & Integração GenAI

### Otimização Automática do K-Means
A função `optimize_and_run_kmeans` elimina escolhas arbitrárias de hiperparâmetros. Ela isola as variáveis numéricas cruciais:
* `averageScore`
* `popularity`
* `episodes`

Aplica o `StandardScaler` para balancear a influência de escalas díspares e avalia um intervalo de $K$ (de 2 a 8 grupos). Ao traçar em paralelo a curva de Inércia e a linha de coeficiente Silhouette, o pipeline extrai via código (`np.argmax`) o índice ideal que maximiza a separação e coesão dos clusters, aplicando o modelo definitivo com base nessa decisão matemática autónoma.

### Relatórios Cognitivos com Gemini
Com os agrupamentos validados, a rotina `generate_cluster_personas` extrai os animes mais populares de cada cluster e extrai as reviews reais de utilizadores mais representativas daquele nicho. Esse bloco textual denso serve como base contextual estruturada enviada ao **Gemini 2.5 Flash**, parametrizado para responder três pilares analíticos de mercado por grupo:
1.  **Nome Criativo do Cluster:** Uma identidade estilística marcante para o grupo.
2.  **Público-Alvo (Persona):** O arquétipo do consumidor padrão que engaja com aquele nicho.
3.  **Gatilho de Engajamento:** O principal fator técnico ou emocional que gera retenção e paixão pela obra.

---

## 🛠️ Stack Tecnológica

* **Manipulação e Computação Numérica:** `pandas`, `numpy`
* **Conexão e Parsing Web:** `requests`, `beautifulsoup4`
* **Visualização de Dados:** `matplotlib`, `seaborn`, `plotly`
* **Modelagem Estatística e ML:** `scikit-learn`
* **Motor de Inteligência Artificial:** `google-genai`
* **Configuração e Segurança:** `python-dotenv`

---

## ⚙️ Instalação e Execução Local

Siga as diretrizes abaixo para clonar, configurar e correr o projeto no seu ambiente local:

1.  **Clonar o Repositório:**
    ```bash
    git clone https://github.com/SEU_USUARIO/NOME_DO_REPOSITORIO.git
    cd NOME_DO_REPOSITORIO
    ```

2.  **Inicializar e Ativar o Ambiente Virtual:**
    ```bash
    python -m venv venv
    # No Windows (PowerShell):
    .\venv\Scripts\Activate.ps1
    # No Linux / macOS:
    source venv/bin/activate
    ```

3.  **Instalar as Dependências Requeridas:**
    ```bash
    pip install requests pandas numpy matplotlib seaborn plotly beautifulsoup4 scikit-learn google-genai python-dotenv
    ```

4.  **Configurar Chaves de Acesso:**
    Crie um ficheiro `.env` na raiz do diretório e insira o seu token obtido na Google AI Studio:
    ```env
    GEMINI_API_KEY=insira_aqui_a_sua_chave_api_do_gemini
    ```

5.  **Executar o Pipeline:**
    Inicie o Jupyter e abra o ficheiro `analysis_top100_anilist.ipynb` para executar o fluxo de ponta a ponta:
    ```bash
    jupyter notebook
    ```

---
*Desenvolvido como projeto de portfólio para demonstração de competências integradas em Data Engineering, Data Science e Generative AI.*
