Prova prática - Analista de Big Data

Olá candidato! :)

Parabéns por ter chegado até essa etapa!

Leia atentamente as informações abaixo para realização da prova.

Boa prova!!
Contextualização

Você recebeu alguns conjuntos de dados sobre estudantes, contendo diversos dados sobre sua trajetória acadêmica.

Seu objetivo é criar os pipelines de extração, transformação, refinamento e carga desses dados no banco de dados Postgresql no padrão de Data Warehouse (modelagem dimensional).

Você deverá utilizar Python + Spark na execução das etapas.
Fontes de dados

Os dados estão localizados em três fontes.
Banco de dados PostgreSQL

Alguns dos dados estão contidos nas tabelas abaixo do banco de dados prova:

    alunos
    matriculas

Dados de conexão:

    Host: localhost
    User: postgres
    Password: password
    Port: 5432
    Dabase: prova

Google Sheets

Alguns dos dados estão contidos em uma planilha Google, nas abas:

    Cursos
    Disciplinas
    Turmas

Link da planilha

Esses dados devem ser extraídos utilizando API Google.
Diretório local

Alguns dos dados estão contidos em formato CSV dentro da pasta notebooks/dados:

    matricula_aulas.csv
    matricula_disciplinas.csv
    matricula_notas.csv
    turma_avaliacoes.csv

Dicionário de dados

O dicionário encontra-se dentro da pasta docs: Dicionário.md
Regras de negócio

    A nota de aprovação para os cursos é de 7,0.
    A porcentagem de frequência para aprovação é 70%.
    A data das aulas e avaliações deve estar entre o início e término das turmas, e no ocorrer no dia da semana da disciplina.
    A idade mínima dos alunos deve ser 18 anos e a máximo 90 anos.

Subindo o ambiente com Docker

Para facilitar a configuração do seu ambiente de trabalho, está sendo disponibilizado um arquivo docker-compose.yml com os containeres referentes aos serviços:

    Minio: Esse é um serviço de Data Lake, onde você deverá armazenar os arquivos extraídos, transformados e refinados.
    Jupyter: Esse é um serviço de notebooks onde você deverá desenvolver seus notebooks para cada fase do ETL.
    PostgreSQL: Esse é um serviço de banco de dados onde você deverá armazenar as tabelas do Data Warehouse.

Para iniciar os containers, com o Docker e Docker Compose instalados em seu computador, execute: docker-compose up ou docker compose up.
Subindo o ambiente sem Docker

Nesse caso, você precisará instalar os serviços de forma independe no seu computador.

    Minio: https://min.io/docs/minio/linux/operations/installation.html
    Jupyter: https://jupyter.org/install
    PostgreSQL: https://www.postgresql.org/download

Obs.: Recomendamos usar o ambiente em Docker, devido já estar todo configurado e pronto para utilizar, bastando a instalação do Docker em seu computador.
Acessando os serviços

Abaixo os endereços e portas dos serviços.
Minio

Acessar através do endereço http://localhost:9009

Access Key: minioaccesskey Secret Key: miniosecretkey

Para criar novos buckets, acesse: http://localhost:9009/buckets

Você precisará criar os buckets seguindo a arquitetura medalhão.
Jupyter

Acessar através do endereço: http://localhost:8888

Token: token

Os notebooks devem ser criados no diretório notebooks: http://localhost:8888/lab/tree/notebooks

Obs: No arquivo notebooks/HelloWorld.ipynb consta um exemplo de configuração do Spark com PostgreSQL.
PostgreSQL

Configurar em seu software de gerenciamento de bancos de dados favorito (dica Dbeaver).

    Host: localhost
    User: postgres
    Password: password
    Port: 5432

Deve existir uma base de dados de nome prova nas quais contém algumas tabelas que vão ser extraídas.

Você deve criar uma nova base de dados chamada dw que vai conter o modelo dimensional do DW.
Execução da Prova

Etapas a seguir deevem ser executadas pelo candidato.
Questão 1: Extração

Crie os notebooks responsáveis pela extração dos dados conforme as fontes citadas anteriormente.

Você deve extrair os dados e armazenar no Minio seguindo as boas práticas e seguindo a arquitetura medalhão
Questão 2: Transformação

Crie os notebooks responsáveis pelas transformações dos dados brutos extraídos anteriormente.

Aplique técnicas de padronização nos dados, nomes de campos, tipagens de campos, criação de metadados, e o que mais julgar importante nessa fase. Justifique cada umas das técnicas que forem aplicadas.

O resultado deve ser salvo no bucket apropriado dentro do Minio.
Questão 3: Refinamento

Crie os notebooks responsáveis pelo refinamento dos dados transformados anteriormente.

Nessa etapa você deve enriquecer os dados, conforme julgar necessário, e transformá-los no formato adequado para o modelo dimensional (dimensões e fatos). Explique as atividades que forem executadas nessa etapa.

O resultado deve ser salvo no bucket apropriado dentro do Minio.
Questão 4: Carga

Crie os notebooks responsáveis pela carga dos dados refinados no banco de dados do Data Warehouse.

Você deve avaliar a estratégia de acordo com cada fonte de dado, se faz sentido usar overwrite ou append dos dados no banco de dados. Explique suas motivações.
Questão 4: Execução dos pipelines

Aplique alguma técnica para orquestrar a execução dos pipelines, executando-os de forma encadeada.

A execução devem ser de acordo com cada etapa: extração > transformação > refinamento > carga.

Os logs de execução e/ou falhas dos pipelines devem ser monitorados de alguma maneira.
Entregáveis

    Jupyter notebook com os códigos desenvolvidos utilizando linguagem Python + Spark.
    O código deve ser bem comentado e de fácil de entendimento/manutenção.
    Diagrama contendo os serviços envolvidos, pipelines, buckets e banco de dados do DW.

Critérios de avaliação

    Habilidades com Python, incluindo proficiência em bibliotecas como PySpark
    Habilidades em engenharia de dados, incluindo a capacidade de extração, transformação e carga de dados
    Organização das etapas e códigos.
    Explicação do que foi desenvolvido.

Observações

    Todo o desenvolvimento realizado deve ser versionado no repositório liberado para o candidato.
    Lembre-se de documentar o seu desenvolvimento, de forma que seja de fácil entendimento para outras pessoas.
    Lembre-se de adicionar as instruções, através de comentários, caso precise instalar alguma biblioteca específica para executar os notebooks.

FLUXO DE TRABALHO DESENVOLVIDO PARA CRIAÇÃO DO PIPELINE

O notebook pipeline.ipynb serve para rodar todo o fluxo de trabalho desde a extração dos dados necessários contidos dentro do DB prova no Postgres e da planilha no Google Sheets até a criação do Data Warehouse com as tabelas dimensão e fato refinadas.

O fluxo de aplicação segue a seguinte ordem: 1- data_extractio.ipynb para extrair os dados das fontes comentadas, criar um bucket nível bronze no Data Lake usando o MinIO e salvar os arquivos RAW. 2- data_transformation onde os dados da camada bronze são limpos e transformados para então serem armazenados na camada prata do Data Lake. Foi garantido que apenas dados não existentes fossem gravados de modo a evitar duplicidade. 3 - data_refinement.ipynb usado para refinar os dados, criando tabelas dimensão e fato a serem armazenadas na camada ouro do Data Lake. Estas tabelas tem a função de serem usadas para BI ou outros serviços. Foi garantido que apenas dados inexistentes fossem gravados de modo a evitar duplicidade. A criação de uma coluna de particionamento garante o maior controle dos dados. 4 - data_warehouse_load serve para fazer a carga das tabelas tratadas na camada ouro do Data Lake para o Data Warehouse. Foi garantida Foi garantido que apenas dados inexistentes fossem gravados de modo a evitar duplicidade. Critérios como uso de append ou overwrite foram observados para evitar perda dos dados históricos. 5 - o notebook deletar_DW.ipynb serve para deletar os buckets e tabelas do Data Lake e do Data Warehouse facilitando os testes de carga full e incremental.
