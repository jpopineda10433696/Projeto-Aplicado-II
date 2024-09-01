# Análise e Compreensão dos Metadados

  Os dados escolhidos estão disponíveis no portal de transparência da SSP e refere-se aos registros de homicídios dolosos do estado de São Paulo entre os períodos de 2017 a 2022.

  A Secretaria de Segurança Pública de São Paulo divulga bases de registros de crimes contra a vida e as divide entre homicídio doloso, feminicídio, latrocínio, morte suspeita e lesão corporal seguida de morte, para o projeto em questão, foi escolhida a base ‘Homicídio Doloso’ diante da robustez dos dados.

  A partir da análise dos dados disponibilizados, buscamos identificar padrões, aferir tendências ao longo do tempo, relacionar informações de gênero, profissão e faixa etária para compreender os grupos mais vulneráveis à condição de vítima desses crimes.

  Abaixo, buscamos definir cada uma das colunas do conjunto de dados a fim de compreender, na integralidade, o conjunto de dados.

| Nome                                             | Definição                                           | Tipo      |
|--------------------------------------------------|----------------------------------------------------|-----------|
| Departamento da Circunscrição                    | Departamento responsável pelo registro do homicídio.| String    |
| Município da Circunscrição                       | Município Responsável pelo B.O.                     | String    |
| Número de Vítima de Homicídio Doloso             | Quantidade de vitimados pelo ato registrado.        | Int       |
| Mês da Estatística                               | Mês de registro da ocorrência.                      | Int       |
| Data e Hora do Registro do Boletim de Ocorrência | Data completa do registro.                          | Date      |
| Ano do Boletim de Ocorrência                     | Ano de registro do Boletim de Ocorrência            | Int       |
| Departamento de Elaboração                       | Nome da delegacia responsável pela elaboração       | String    |
| Departamento de Elaboração                       | Departamento responsável pelo registro do homicídio.| String    |
| Hora do Fato                                     | Hora relatada no B.O.                               | Timestamp |
| Logradouro                                       | Endereço do ocorrido.                               | String    |
| Latitude                                         | Coordenadas do local onde o fato ocorreu.           | Float     |
| Tipo de Pessoa                                   | Qualificação do indivíduo - focaremos em vítimas.   | String    |
| Idade da Pessoa                                  | Anos completos de idade.                            | Int       |
| Cor da Pele                                      | Raça do indivíduo.                                  | String    |
| Natureza Apurada                                 | Homicídio doloso.                                   | String    |
