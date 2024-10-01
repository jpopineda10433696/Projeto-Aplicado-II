# Kick Off

## Introdução

  Este trabalho foi realizado por um grupo de estudantes com o objetivo de analisar dados de 
homicídios dolosos no estado de São Paulo. Os dados utilizados estão disponíveis no portal de 
transparência da Secretaria de Segurança Pública do Estado de São Paulo (SSP-SP) e 
abrangem o período de 2017 a 2022.

  O projeto busca entender os padrões e tendências dos homicídios dolosos no estado, 
utilizando uma base de dados extensa que inclui informações sobre localização, data e hora 
dos crimes, características das vítimas (como idade, gênero, raça e profissão) e detalhes 
específicos dos incidentes. A análise desses dados permite identificar grupos mais vulneráveis 
e compreender melhor as dinâmicas da criminalidade.

  Ao realizar essa análise, o grupo espera contribuir para uma compreensão mais aprofundada 
dos fatores que influenciam os homicídios dolosos em São Paulo, fornecendo insights que 
possam ser utilizados para o desenvolvimento de políticas de segurança pública mais eficazes 
e estratégias de prevenção.

## Cronograma

![image](https://github.com/user-attachments/assets/26e0ff97-220c-4258-8e8c-a1c1bd1c096c)

## Organização

  Para desenvolver o projeto foi escolhida a Secretaria da Segurança Pública do Estado de São 
Paulo (SSP-SP) que é um órgão do governo estadual responsável pela coordenação e 
administração das políticas de segurança pública. A SSP-SP tem como principais funções a 
prevenção, controle e combate à criminalidade, além de garantir a segurança das pessoas, 
propriedades e comunidades.

  A SSP-SP também coordena programas de segurança comunitária e trabalha em conjunto com 
outras instituições para promover a segurança e o bem-estar da população.
Essa secretaria conta com uma base de dados extensa que contém informações sobre 
diversos setores onde a organização atua.

## Apresentação dos Dados

  Os dados escolhidos estão disponíveis no portal de transparência da SSP1 e refere-se aos 
registros de homicídios dolosos do estado de São Paulo entre os períodos de 2017 a 2022.

  A Secretaria de Segurança Pública de São Paulo divulga bases de registros de crimes contra a 
vida e as divide entre homicídio doloso, feminicídio, latrocínio, morte suspeita e lesão corporal 
seguida de morte, para o projeto em questão, foi escolhida a base ‘Homicídio Doloso’ diante da 
robustez dos dados – tendo em vista que, nesta base, as informações à respeito das vítimas e 
do fato ocorrido são mais completas, além de contar com mais registros que as demais dada a 
natureza generalista do crime.

## Proposta Analítica e Resultados pretendidos

  A partir da análise dos dados disponibilizados, buscamos identificar padrões, aferir tendências 
ao longo do tempo, relacionar informações de gênero, profissão e faixa etária para 
compreender os grupos mais vulneráveis à condição de vítima desses crimes.

  Abaixo, buscamos definir cada uma das colunas do conjunto de dados a fim de compreender, 
na integralidade, o conjunto de dados.

| Nome do Campo | Definição                                                                     | Tipo    |
|--------------|------------------------------------------------------------------------------|---------|
| Departamento da Circunscrição | Departamento responsável pelo registro do homicídio                | String  |
| Seccional da Circunscrição  | Divisão da seccional do departamento responsável.                   | String  |
| Municipio da Circunscrição  | Municipio Responsável pelo B.O                                         | String  |
| Departamento da Circunscrição  | Subdivisão do municipio geralmente, bairro.                       | String  |
| Número de Vitimas de Homicidio Doloso | Quantidade de vitimados pelo ato registrado.                    | Int     |
| Identificação da Delegacia | ID único de delegacia responsável pela apuração.                       | String  |
| Mês da Estatistica | Mês de registro da ocorrência.                                            | Int     |
| Ano da Estatistica | Ano de registro da ocorrência.                                                 | Int.    |
| Data e Hora do Registro do Boletim de Ocorrência | Data completa do registro                                                       | Date    |
| Número do Boletim de Ocorrência | ID único do boletim de ocorrência                                               | String  |
| Ano do Boletim de Ocorrência | Ano de registro do Boletim de Ocorrência                                                   | Int.    |
| Municipio de Elaboração | Municipio Responsável pelo B.O                                                   | String  |
| Departamento de Elaboração | Nome da delegacia responsável pela elaboração                       | String  |
| Seccional de Elaboração | Divisão da seccional do departamento responsável pelo B.O.              | String  |
| Departamento de Elaboração | Departamento responsável pelo registro do homicídio                | String  |
| Data do Fato | Dia do ocorrido presente no B.O                                                 | Date    |
| Hora do Fato | Hora relatada no B.0.                                                           | Timestamp|
| Descrição do Tipo de Local | Local relatado da ocorrência.                                                  | String  |
| Logradouro | Endereço do ocorrido                                                           | String  |
| Número do Logradouro | Número do local caso trate-se de uma edificação                | int     |
| Latitude | Coordenadas do local onde o fato ocorreu.                                               | Float   |
| Longitude | Coordenadas do local onde o fato ocorreu.                                               | Float   |
| Tipo de Pessoa | Qualificação do indivíduo, focaremos em vitimas                          | String  |
| Sexo da Pessoa | Gênero do indivíduo                                                            | String  |
| Idade da Pessco | Anos completos de idade.                                                        | Int     |
| Data de Nascimento da Pessoa | Data de nascimento completa do indivíduo                                                | Date    |
| Cor da Pele | Raça do indivíduo.                                                             | String  |
| Profissão | Ocupação profissional do indivíduo.                                           | String  |
| Natureza Apurada | Homicídio dolosso                                                                | String  |



