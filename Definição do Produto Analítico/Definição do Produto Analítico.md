# Definição da linguagem de programação
A principal linguagem de programação a ser utilizada será a linguagem Python, devido ao alto suporte de bibliotecas de análise de dados, modelagem de NLP (Processamento de Linguagem Natural).
As bibliotecas utilizadas, com suas respectivas justificativas são as seguintes:

- **Pandas**: Manipulação e análise de dados.
- **NumPy**: Operações matemáticas e manipulação de arrays.
- **Matplotlib/Seaborn**: Visualização de dados.
- **NLTK (Natural Language Toolkit)**: Processamento de linguagem natural (tokenização, remoção de stopwords, etc.).
- **Unicodedata**: Normalização de textos com base no código ASCII.
- **WordCloud**: Geração de nuvens de palavras.
- **Scikit-learn**: Algoritmos de aprendizado de máquina e métricas de avaliação.
- **TextBlob**: Análise de sentimento.


# Análise exploratória da base de dados

A análise exploratória inicial realizada busca trazer informações sobre as estruturas dos dados, identificar padrões, valores ausentes e distribuição dos tweets. 
Devido a recentes restrições na API da rede social X para extração dos tweets, somado com mudanças nos termos de uso da plataforma que impedem extração direta dos dados via HTML com as principais bibliotecas python (Snscrape, BeautifulSoup etc.), os tweets mais recentes foram extraídos manualmente.
Para fins de rotulagem dos dados para garantir métricas que garantam a acuracidade do modelo, foi criada uma coluna na base de dados nomeada de sentimentos, onde o grupo classificou os tweets em três parâmetros:

  -  *1* : Sentimento Positivo
  -  *0* : Sentimento Neutro - Sem críticas ou elogios
  - *-1*: Sentimento Negativo
    
O dataframe foi criado via biblioteca pandas e abaixo estão as principais características obtidas.

- **Cabeçalho**

	O dataset é composto por 5 colunas, que representam características do tweet, como data de publicação, link para acesso, usuário, conteúdo e rótulo de sentimento, conforme mencionado anteriormente.

  ![image](https://github.com/user-attachments/assets/9285eda4-faaa-4998-a836-b26b7cb27bc1)


- **Tipos de dados**

	Os tipos de dados são, em sua maior parte, do tipo “object” devido a alta variação do tipo de conteúdo de cada atributo. Já a coluna “sentimento” é do tipo “int64” pois os rótulos de sentimento foram escolhidos com base em números inteiros (-1,0,1).
	
	![image](https://github.com/user-attachments/assets/8714d884-4afd-4ae9-929d-79c2a67aa4da)

- **Distribuição dos sentimentos (rótulos)**

  De acordo com a distribuição abaixo sobre os sentimentos, é possível verificar que cerca de 40 dos 59 tweets são negativos em relação à situação da saúde no Brasil (aproximadamente 68% do total).
  Em relação aos sentimentos negativos ou neutros, não existe nenhuma variação perceptível dentro da amostra de tweets mais recentes.

	![image](https://github.com/user-attachments/assets/68892ca5-796b-4414-95af-e9636d252e27)

  
- **Nuvem de palavras**

  É possível verificar que, sem um tratamento adequado da base de dados, não é possível distinguir termos relacionados à classificação de sentimentos dentro da nuvem de palavras, pois as mesmas trazem uma densidade maior de termos com maior repetição, em sua maioria artigos, preposições e conjunções (ou stopwords, conceito que será explorado na próxima etapa).

  ![image](https://github.com/user-attachments/assets/637df20a-ef20-40fb-a314-ce7f1c381ecc)


# Tratamento da base de dados

Devido a complexidade da manipulação de linguagem natural, o tratamento da base de dados foi realizado em várias etapas, sendo elas:

- **Remoção de links, menções e caracteres especiais.**
  
  Para isso, foi utilizada a biblioteca “re” que realiza manipulação de textos retirando caracteres especiais, além da “unicodedata” que foi utilizada para normalizar os tweets e remover acentos.

      def limpar_texto(texto):
          texto = re.sub(r"http\S+|www\S+|https\S+", '', texto, flags=re.MULTILINE)
          texto = re.sub(r'\@\w+|\#', '', texto)
          texto = unicodedata.normalize('NFKD', texto).encode('ASCII', 'ignore').decode('ASCII')
          texto = re.sub(r'[^A-Za-z0-9\s]', '', texto)
          texto = texto.lower()
          return texto

- **Remoção de stopwords**
  
  Para a remoção de stopwords, ou seja, caracteres irrelevantes para a análise textual, foi utilizada a biblioteca “nltk”.

      stop_words = set(stopwords.words('portuguese'))
      df['texto_limpo'] = df['texto_limpo'].apply(lambda x: ' '.join([word for word in word_tokenize(x) if word not in stop_words]))

Por fim, com a aplicação das funções acima de tratamento do dataset e realizando um filtro de palavras-chave que foram utilizados para pesquisa do tweet, obtemos a nova nuvem de palavras abaixo.

![image](https://github.com/user-attachments/assets/40dfc268-a30a-4d1c-b9b7-4c0705ad8ab8)


É possível perceber que as palavras maiores como “fila”, “atendimento”, “falta” e “morrem” (e derivados), corrobora a distribuição anterior dos rótulos de dados de que grande parte dos tweets se refere a fatores negativos ou reclamações específicas sobre o sistema público de saúde no Brasil.

Nas próximas etapas, serão construídos algoritmos de aprendizado de máquina para criar um modelo que realizará a análise dessas principais palavras e irá atribuir rótulos de classificação de sentimento para identificar a acurácia do modelo em relação à rotulagem atribuída pelo grupo.

O código detalhado está disponível no projeto do GitHub na pasta “Definição do Produto Analítico”.

# Definição e descrição das bases teóricas dos métodos

A construção do modelo de classificação de sentimentos utilizará os conceitos teóricos mais relevantes com base nos projetos existentes desse tipo de abordagem.
Dentre as principais base teóricas estudadas no curso e suas definições estão:

  - **Processamento de Linguagem Natural (NLP)**: Utilização de linguagem computacional para compreender e gerar linguagem humana de forma efetiva e coesa.
As principais aplicações no projeto de NLP são a identificação de caracteres especiais, manipulação de stopwords, normalização de palavras e identificação de padrões linguísticos para classificação de sentimento.

  - **Aprendizado de Máquina**: Descoberta de padrões via algoritmos computacionais para realizar previsões ou classificações.
Nas próximas etapas, será utilizada a biblioteca “sklearn” para criação de algoritmos de classificação, regressão logística e criação de métricas de desempenho.

  - **Aquisição e Preparação de Dados (ETL)**: Extração, Transformação, Limpeza e Carga de dados para os modelos de aprendizado de máquina.
No item anterior, foi realizado o carregamento das bases de dados para o Google Colab e posterior transformação dos dados com pandas para manipular o dataset, retirar termos indesejados, calcular as principais estatísticas, etc.


# Definição e descrição de como será calculada a acurácia
	
A escolha das métricas a serem utilizadas é de extrema importância pois definem se o nosso modelo de análise de sentimentos é suficiente para prever outros casos com acuracidade, entender (em tempo real) qual é o sentimento do brasileiro em relação à saúde pública do país sem vieses e também garantir com que o modelo se assemelha à visão humana sobre o sentimento de outros indivíduos com assertividade.
Para essa garantia, utilizaremos as seguintes métricas de avaliação:

 - **Acurácia**:

       Acurácia = Previsões Corretas/Total de Previsões

   A definição de “Previsões Corretas” será garantida pela comparação entre o sentimento do tweet identificado pelo modelo e o rotulado pelo grupo.

  - **Matriz de Confusão**:

    |                   | Previsto Positivo | Previsto Negativo |
    |-------------------|-------------------|-------------------|
    | **Real Positivo** | VP                | FN                |
    | **Real Negativo** | FP                | VN                |


    A matriz de confusão compara as frequências de classificação para cada classe do modelo, ou seja, mostrando as classificações corretas e incorretas para cada classe. Esse tipo de métrica é importante pois permite identificar onde exatamente está a maior taxa de erro do modelo.

  - **AUC-ROC**
    
  	É uma métrica de desempenho utilizada para representar o desempenho de um modelo de classificação e mostra a capacidade de um modelo em distinguir entre duas classes (positivo e negativo, em nosso caso).
  	Essa métrica é composta pela curva ROC, que é uma ferramenta visual que define a taxa de verdadeiros positivos contra falsos positivos. Além disso, também contém o AUC, que determina a área sob a curva ROC. Quanto mais próxima de 1, melhor o desempenho do modelo.
