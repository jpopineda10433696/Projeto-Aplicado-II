# Apresentação de Produtos e Storytelling

Essa etapa tem o objetivo de trabalhar a consolidação dos resultados para os métodos analíticos, usando métricas de desempenho. Além disso, será realizada a perspectiva de apresentação com a teoria de Data Storytelling.

## Aplicação do Método Analítico

Para o projeto, foi definido que o método analítico a ser utilizado será a de construção de um modelo de classificação que identifique o sentimento do usuário da plataforma X durante a postagem do tweet sobre a saúde pública no Brasil.

Utilizando como base os diversos projetos de PLN (Processamento de Linguagem Natural) criados em Python, foram escolhidos 4 algoritmos de
aprendizado de máquina, divididos em pipelines que apresentam melhores
resultados para análises de textos diversificados, como aqueles apresentados nos
tweets. São eles:

  **- Pipeline 1: TF-IDF + Regressão Logística:**
    
  Utiliza o TF-IDF (Term Frequency-Inverse Document Frequency) para
  converter textos em números considerando a frequência das palavras,
  atribuindo pesos para as maiores frequências.
  
  Além disso, a Regressão Logística é um modelo linear muito utilizado
  para classificação binária, além de possuir boa performance com textos.
  
    
  **- Pipeline 2: Count Vectorizer + Naive Bayes:**
    
  Count Vectorizer é um algoritmo que converte texto e matriz para
  facilitar a contagem de palavras, além de ser mais simples que o anterior
  (TF-IDF).
  
  Já o Naive Beayes é um algoritmo de classificação que leva em
  consideração, com alta probabilidade, a independência das variáveis do
  modelo, funcionando melhor em dados esparsos (palavras muito distintas).
    
**- Pipeline 3: TF-IDF + SVM (Support Vector Machine)**

  Além de utilizar o TF-IDF, como o primeiro pipeline, a escolha do
  algoritmo SVM (Support Vector Machine) se suporta por utilizar a divisão dos
  dados em diversas dimensões (hiperplanos), maximizando a distância entre
  as classes.
  
  Com isso, o SVM consegue trabalhar melhor com textos, pois são
  espaços de alta dimensão (dados esparsos e em grandes conjuntos).
  
      
  **- Pipeline 4: TF-IDF + Random Forest**
  
  Nesse pipeline, foi utilizado o Random Forest, que é um modelo que
  trabalha com um conjunto de árvores de decisão. Possui boa capacidade de
  generalização e lida bem com não-linearidades, trabalhando com
  probabilidades em cada ramo da "árvore" de decisão.
  
  Todos os modelos acima estão disponíveis dentro da biblioteca scikit-learn
  do Python.
      
      import numpy as np
      from sklearn.model_selection import train_test_split
      from sklearn.feature_extraction.text import TfidfVectorizer, CountVectorizer
      from sklearn.pipeline import Pipeline
      from sklearn.metrics import accuracy_score, confusion_matrix,
      classification_report
      from sklearn.linear_model import LogisticRegression
      from sklearn.naive_bayes import MultinomialNB
      16
      from sklearn.ensemble import RandomForestClassifier
      from sklearn.svm import LinearSVC
      from imblearn.over_sampling import SMOTE
      
Devido ao número baixo de amostras na base de dados, ocasionado por
impeditivos da extração em massa de dados do X (políticas de LGPD), foi decidido
que somente serão trabalhados dois tipos de tweets: positivos e negativos.
Isso facilitará o cálculo de métricas, aplicação de testes e identificação de
desbalanceamento de classes.

      # Filtro de rótulos negativos ou positivos, somente
      df_binary = df[df['sentimento'].isin([-1, 1])].copy()
      X = df_binary['texto_filtrado']
      y = df_binary['sentimento']
      Após essa etapa, o modelo foi dividido em treino e teste e foram criadas as 4
      pipelines conforme descrito acima.
      X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2,
      random_state=42)
      
Criação das pipelines:

      # Pipeline 1: TF-IDF + Regressão Logística
      pipeline1 = Pipeline([
      17
      ('tfidf', TfidfVectorizer(ngram_range=(1, 2), max_features=1000)),
      ('clf', LogisticRegression(class_weight='balanced'))
      ])
      
      # Pipeline 2: Count Vectorizer + Naive Bayes
      pipeline2 = Pipeline([
      ('count', CountVectorizer(ngram_range=(1, 2))),
      ('clf', MultinomialNB())
      ])
      
      # Pipeline 3: TF-IDF + SVM
      pipeline3 = Pipeline([
      ('tfidf', TfidfVectorizer(ngram_range=(1, 2), max_features=1000)),
      ('clf', LinearSVC(class_weight='balanced'))
      ])
      
      # Pipeline 4: TF-IDF + Random Forest
      pipeline4 = Pipeline([
      ('tfidf', TfidfVectorizer(ngram_range=(1, 2), max_features=1000)),
      ('clf', RandomForestClassifier(n_estimators=100, class_weight='balanced'))
      ])

A partir desse ponto, foi criada uma lista com todos os pipelines acima,
permitindo com que possa ser utilizado um laço de treinamento para cada um dos
modelos e seus respectivos cálculos das métricas de desempenho.

      def avaliar_modelo(nome, pipeline, X_train, X_test, y_train, y_test):
      print(f"\nResultados para {nome}:")
      pipeline.fit(X_train, y_train)
      y_pred = pipeline.predict(X_test)
      print("Acurácia:", accuracy_score(y_test, y_pred))
      print("\nMatriz de Confusão:")
      print(confusion_matrix(y_test, y_pred))
      print("\nRelatório de Classificação:")
      print(classification_report(y_test, y_pred))
      for nome, pipeline in pipelines:
      avaliar_modelo(nome, pipeline, X_train, X_test, y_train, y_test)

## Medidas de Acurácia

As medidas de acurácia definidas foram resumidas conforme tópicos abaixo
para facilitar a comparação entre os modelos.

  - Pipeline 1: Regressão Logística:

  ![image](https://github.com/user-attachments/assets/82c767d4-eec9-4474-ad25-9b71fe3fd247)

    
  - Pipeline 2: Naive Bayes:

  ![image](https://github.com/user-attachments/assets/0311c32c-40f3-4e64-b0bf-a34df92a1d15)

  
  - Pipeline 3: SVM:

  ![image](https://github.com/user-attachments/assets/4029c6ed-fa83-4850-bd3b-d35337bfc2d2)

  
  - Pipeline 4: Random Forest:

  ![image](https://github.com/user-attachments/assets/a8084939-344b-4ec3-b900-40d4f531e00d)


## Descrição dos Resultados Preliminares

Com o resultado acima, foi possível obter como produto um modelo de
aprendizado de máquina que identifica, por meio de tweets na rede social X, qual é
o principal sentimento dos brasileiros em relação à saúde pública no país.

A escolha do modelo ideal se baseou nas principais métricas de desempenho
conforme análise realizada abaixo.


Comparação de Acurácia Geral dos Modelos:

  - Pipeline 1: Regressão Logística: 84.21%
  - Pipeline 2: Naive Bayes: 84.21%
  - Pipeline 3: SVM: 89.47% **(MAIOR)**
  - Pipeline 4: Random Forest: 78.95%
    
Por classes, os modelos performaram da seguinte forma:


![image](https://github.com/user-attachments/assets/55caef6c-ae03-442c-bb61-e0033381f473)



Retirando uma média simples das métricas de desempenho.


![image](https://github.com/user-attachments/assets/fdca39ab-ee9e-4f7e-a4ae-0aadfca28a19)


Com os dados apresentados acima, é possível concluir que o melhor modelo
é o SVM (Pipeline 3), seguido por Naive Bayes (Pipeline 2), pois ambos apresentaram performance superior tanto nas métricas por classes, quanto na acurácia.


Uma outra conclusão a ser destacada é que todos os modelos tiveram
facilidade de classificar a classe de tweets negativos (-1) e dificuldade em tweets
positivos (+1). Isso pode ser explicado pelo desbalanceamento das classes, pois da
amostra de 100 tweets coletados, a grande maioria contém palavras e frases de
cunho negativo sobre a saúde no Brasil, adequando o modelo à classe com maior
frequência de rótulos (negativos).


Esse é um risco de que o modelo pode apresentar overfitting e/ou
underfitting, dependendo do número de dados nas bases de treinamento e
também pela dificuldade dos algoritmos em identificar tweets que contém ironia
ou sarcasmo, podendo resultar em falsos positivos e/ou falsos negativos.


Para escalabilidade, melhor acurácia e, principalmente, garantia de
classificação coerente com base na semântica utilizada em cada um dos tweets, é
importante que haja uma coleta constante da base de dados, a fim de evitar
problemas com desbalanceamento e baixa amostragem.


## Testes em Frases Aleatórias

Abaixo, foi feita a definição de frases de teste e seus respectivos rótulos com
o objetivo de aplicar as pipelines obtidas acima com frases aleatórias e identificar a
efetividade das métricas de desempenho.


Além disso, esses testes podem conter indicativos se os modelos
apresentam vieses devido a coleta de dados (overfitting e/ou underfitting).

As frases utilizadas para teste nos modelos foram:


1. "O atendimento no hospital foi péssimo, muito demorado",
2. "Excelente iniciativa do governo na campanha de vacinação",
3. "Falta de medicamentos básicos é um absurdo",
4. "O novo centro de saúde está muito bem equipado",
5. "Os médicos são muito atenciosos e competentes",
6. "O sistema de saúde está um caos total"
7. "Adorei o novo programa de prevenção à saúde",
8. "Péssimas condições de trabalho para os profissionais"
   
Aplicando cada um dos 4 algoritmos às frases acima obtemos os seguintes
resultados:

  - Pipeline 1
  
  ![image](https://github.com/user-attachments/assets/16adc5ac-9333-4e03-9856-8a5ae8e40908)
  
  - Pipeline 2

  ![image](https://github.com/user-attachments/assets/dbccf1cf-dd1f-49cf-806d-e7f10dd5757f)

  - Pipeline 3

  ![image](https://github.com/user-attachments/assets/545fe62d-3ca4-4d56-b1f0-b3d5ba50aa67)

  - Pipeline 4

  ![image](https://github.com/user-attachments/assets/d16fc7c8-aff6-41e0-8a02-7a35e4013ff2)


Como identificado na descrição de resultados acima, é possível verificar que
os modelos com maior taxa de acertos foram os Pipelines 2 e 3, ou seja, os dois com
melhores métricas de desempenho.

Mais detalhes sobre o código Python estão no repositório do GitHub (Análise_de_Sentimento_X.ipynb).

## Aplicação em um Modelo de Negócio

O modelo criado acima pode ser aplicado em diversos setores,
principalmente em organizações públicas que monitoram a performance de UBS’s
(Unidade Básica de Saúde) e Hospitais Públicos.


Como proposta de valor, o algoritmo de classificação pode ser utilizado em
dashboards de monitoramento em tempo real sobre problemas relacionados ao
sistema de saúde, com extração direta pela API de redes sociais e classificação pelo
modelo desenvolvido, podendo ser dividido por região, tema e horário.


Um dos principais diferenciais competitivos desse negócio seria o foco
específico no setor de saúde brasileiro, análises contextualizadas do cenário local,
integração com os dados oficiais de saúde e, principalmente, foco na avaliação dos
sentimentos dos usuários do sistema em tempo real, permitindo aplicação de
medidas imediatas de correção.


# Esboço do Storytelling

## Introdução ao Problema: Importância de entender a percepção pública sobre o SUS.

O Sistema Único de Saúde (SUS) é um dos maiores sistemas públicos de
saúde do mundo, sendo vital para milhões de brasileiros. Entender como a
população percebe o SUS é fundamental para aprimorar políticas públicas,
identificar pontos críticos e fortalecer a comunicação entre governo e
sociedade. Com o crescimento das redes sociais, especialmente a rede social
X (antigo Twitter), tornou-se possível captar opiniões, críticas e elogios sobre
o SUS, fornecendo um termômetro valioso da opinião pública. Essa
percepção, muitas vezes, influencia debates, pautas políticas e até mesmo
decisões de gestão em saúde.


## Coleta e Preparação de Dados: Descrição da coleta manual de tweets e preparação da base.

Devido a restrições recentes na API da rede social X e mudanças nos
termos de uso, a coleta de tweets precisou ser feita manualmente. Para essa
abordagem, foram coletados dados como data, usuário, link e texto do
tweet, o qual foi rotulado em uma coluna específica no dataframe,
classificando em sentimentos positivos, neutros e negativos. Essa
preparação inicial foi crucial para estabelecer métricas de acuracidade do
modelo.

A análise exploratória do dataset resultou em indicadores de
distribuição dos sentimentos. Cerca de 68% dos tweets coletados
expressaram insatisfação com a situação da saúde no Brasil, com
predominância de sentimentos negativos e palavras pejorativas, como
“péssimo”, “horrível”, etc. Esse panorama inicial indicou a necessidade de um
tratamento cuidadoso dos dados para uma compreensão mais profunda.

O tratamento dos dados envolveu várias etapas complexas, incluindo a
remoção de links, menções, caracteres especiais e stopwords. Utilizando
bibliotecas como “re”, “unicodedata” e “nltk”, o texto foi limpo e
normalizado. Após esse processo, foi desenvolvida uma nuvem de palavras,
onde foi possível destacar maior predominância de termos como “fila” e
“problema”, os quais corroboram a análise anterior sobre a distribuição de
reclamações.

Com os dados tratados, o próximo passo foi desenvolver algoritmos
de aprendizado de máquina. Esses algoritmos têm o objetivo analisar as
principais palavras e atribuir rótulos de classificação de sentimento,
validando a acuracidade da rotulagem inicial feita pelo grupo.


## Análise de Sentimentos: Aplicação de Machine Learning para classificar sentimentos.

Para a criação do modelo de aprendizado de máquina, foram
escolhidos 4 dos principais modelos utilizados em projetos de PLN
(Processamento de Linguagem Natural). Esses modelos foram divididos em
4 pipelines, sendo eles: Regressão Logística, Naive Bayes, SVM (Support
Vector Machine) e Random Forest.

A escolha desses modelos, além de algoritmos de tratamento
adjacentes, foi justificada pela facilidade dos mesmos de trabalhar com
dados de alta dimensionalidade, como textos, além de atribuir classificação
numérica e pesos para cada palavra (TF-IDF) com a finalidade de direcionar o
modelo para os termos semânticos com maior distribuição de probabilidade
por rótulo definido pelo grupo.

**Exemplo:** A alta presença de palavras como “horrível” e “péssimo” em
frases negativas, somada com a baixa presença de palavras como “bom” ou
“ótimo”, aumenta a acurácia do modelo para tweets classificados como
negativos sobre a saúde pública.

Após essa definição, foi realizada a filtragem da base de dados apenas
para tweets positivos ou negativos, para facilitar a obtenção dos resultados;
e em seguida o treinamento do modelo em cada um dos pipelines definidos
acima, obtendo as principais métricas de desempenho como saída.


## Resultados: Acurácia obtida, insights extraídos e tendências identificadas.

Os modelos de Machine Learning utilizados apresentaram índices
satisfatórios de acurácia na classificação dos sentimentos, validados por
métricas como precisão, recall e F1-score. A análise revelou padrões
interessantes, como a predominância de sentimentos negativos em períodos
de crise ou notícias polêmicas, e aumento de menções positivas após
campanhas de vacinação ou melhorias no atendimento. 

Também foram
identificados tópicos recorrentes, como tempo de espera, qualidade do
atendimento e acesso a medicamentos, permitindo mapear as principais
preocupações e expectativas dos usuários.


## Impacto Potencial: Aplicações para decisões públicas.

A análise contínua dos sentimentos expressos na rede social X pode
subsidiar gestores públicos na tomada de decisões mais informadas e
responsivas. Ao identificar rapidamente mudanças na percepção pública, é
possível ajustar campanhas de comunicação e implementar melhorias em
serviços. Além disso, os insights extraídos podem orientar pesquisas
acadêmicas, apoiar estratégias de combate à desinformação e fortalecer o
diálogo entre governo e sociedade, tornando a gestão do SUS mais
transparente e participativa.

No mais, a detecção precoce de tendências negativas ou insatisfação
recorrente permite respostas proativas, evitando a ampliação de crises
institucionais ou potenciais conflitos sociais.


## Conclusão: Benefícios de uso contínuo da metodologia.

A aplicação da análise de sentimentos em redes sociais representa uma
ferramenta poderosa para monitorar a opinião pública sobre o SUS.Além de
fornecer dados em tempo real, permite identificar tendências, avaliar o
impacto de políticas e aprimorar a comunicação institucional.

O uso contínuo
dessa metodologia contribui para uma gestão mais eficiente e alinhada às
demandas da população, promovendo um ciclo virtuoso de escuta, análise e
ação no âmbito da saúde pública.
