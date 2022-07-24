<img src="https://cdn.eleflow.com.br/ef-web/wp-content/uploads/2016/08/21181642/Eleflow.png" alt="Eleflow BigData" width="200"/>

# Data engineering capstone

## BigData Airlines

A Eleflow irá atender um novo cliente, a _BigData Airlines_, e você será o engenheiro de dados responsável por fazer a ingestão de dados e preparar algumas tabelas para os cientistas de dados e analistas de dados. 

### Capstone

- Carregar os dados de VRA
  - Normalizar o cabeçalho para snake case
  - Salvar estes dados
- Carregar dos dados de AIR_CIA
  - Normalizar o cabeçalho para snake case
  - Separar a coluna 'ICAO IATA' em duas colunas, seu conteúdo está separado por espaço e pode não conter o código IATA, caso não contenha o código IATA, deixe o valor nulo.
  - Salvar estes dados
- Criar nova tabela aerodromos
  - Através da API [https://rapidapi.com/Active-api/api/airport-info/]() trazer os aeródramos através do código ICAO presente nos dados de VRA.
  - Salvar estes dados
- Criar as seguintes views (Priorize o uso de SQL para esta parte):
  - Para cada companhia aérea trazer a rota mais utilizada com as seguintes informações:
    - Razão social da companhia aérea
    - Nome Aeroporto de Origem
    - ICAO do aeroporto de origem
    - Estado/UF do aeroporto de origem
    - Nome do Aeroporto de Destino
    - ICAO do Aeroporto de destino
    - Estado/UF do aeroporto de destino
  - Para cada aeroporto trazer a companhia aérea com maior atuação no ano com as seguintes informações:
    - Nome do Aeroporto
    - ICAO do Aeroporto
    - Razão social da Companhia Aérea
    - Quantidade de Rotas à partir daquele aeroporto
    - Quantidade de Rotas com destino àquele aeroporto
    - Quantidade total de pousos e decolagens naquele aeroporto

#### Extras:
  - Descrever qual estratégia você usaria para ingerir estes dados de forma incremental caso precise capturar esses dados a cada mes?
  - Justifique em cada etapa sobre a escalabilidade da tecnologia utilizada.
  - Justifique as camadas utilizadas durante o processo de ingestão até a disponibilização dos dados.

#### Observações:
   - Você pode utilizar a tecnologia de sua preferência ou seguir a recomendação:
     - Notebooks Jupyter
     - Google Colab
     - Databricks Community
   - Pode incluir comentários sobre a abordagem de extração/transformação que você está fazendo
   - Pode disponibilizar o projeto via Git, URL ou .zip

# Respostas
- Descrever qual estratégia você usaria para ingerir estes dados de forma incremental caso precise capturar esses dados a cada mes?
  - Para o GET dos dados, normalmente é passado parametro de data no request da API, ou o cliente pode estar enviando diretamente os dados novos.
  - Para a inserção no banco, pode ser feito de duas maneiras, criando um id único para cada linha de dado, sendo assim, sempre que vier dados novos, irá subir, e para o caso da rotina ser rodado com dados já existentes, apenas dar UPDATE. Também pode ser feito realizando um delete por periodo, pegando start_date e end_date dos dados de entrada, ou seja, deletar os dados referente ao mês e subir de novo, evitando duplicação na base.
- Justifique em cada etapa sobre a escalabilidade da tecnologia utilizada.
  - Foi utilizado python com a biblioteca requests para realizar os requests na API por conta de sua simplicidade e performance.
  - Também foi utilizado python e pandas para a manipulação dos dados, sendo que a biblioteca pandas foi feita para suportar grandes datasets e por ser muito utilizada atualmente no mercado, possui uma grande comunidade, com isso possui muito conteúdo disponível.
  - O sqlite foi utilizado por comodidade, para poder roder tudo em um lugar só, sendo o colab, porém o ideal seria utilizar outro banco relacional como o PostgreSQL para ter uma maior escalabilidade.
- Justifique as camadas utilizadas durante o processo de ingestão até a disponibilização dos dados.
  - Primeiramente foi feito a ingestão dos dados disponibilizados por arquivo csv e json.
  - Feito um tratamento e normalizado o header para snake_case, para então salvar os dados em um dataFrame (Estrutura de dados do pandas).
  - A partir dos dados do VRA, foi pego os códigos ICAO dos aeroportos, para então realizar o request na API e retornar os dados dos aeroportos.
  - Com isso, foi criado os bancos de dados no SQLITE, inserido cada dado em sua respectiva tabela, para poder montar as views por SQL.
  - Com as tabelas preenchidas, foi feito as queries para visualização das rotas mais utilizadas por cada companhia aérea e a companhia aérea que mais utiliza cada aeroporto. Realizando um JOIN entre as tabelas para pegar as informações desejadas.

OBS: Foi feito tudo no colab para facilitar a visualização do que foi feito.

Link para o Colab: [https://colab.research.google.com/drive/1s9gNtyw8Y9GFTZZLHAUAVPJI3RLEib9m?usp=sharing]()
Arquivo .ipynb também disponibilizado no repositório.
