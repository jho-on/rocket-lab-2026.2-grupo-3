# Rocket Lab 2026.2 — Grupo 3

Pipeline de dados de crédito desenvolvida no Databricks, utilizando a arquitetura Medallion.

## Sumário

- [Estrutura do repositório](#estrutura-do-repositório)
- [Pré-requisitos](#pré-requisitos)
- [Parâmetros](#parâmetros)
- [Análise exploratória](#análise-exploratória)
- [Fluxo de desenvolvimento e deploy](#fluxo-de-desenvolvimento-e-deploy)

## Estrutura do repositório

- `notebooks/ingest_to_bronze.ipynb`: realiza a ingestão dos arquivos CSV para a camada Bronze.
- `notebooks/bronze_to_silver.ipynb`: realiza o tratamento e a preparação dos dados na camada Silver.
- `notebooks/silver_to_gold.ipynb`: prepara os dados da camada Gold para análise.
- `notebooks/csv_ingestion_diagnostics.ipynb`: investiga características das fontes de dados, como *encoding*, duplicatas e chaves candidatas, auxiliando na avaliação de estratégias de carga.
- `resources/v_credit_medallion_pipeline.job.yml`: define o Job, incluindo tarefas, dependências e parâmetros.
- `databricks.yml`: configura o Databricks Asset Bundle.

## Pré-requisitos

### Arquivos de origem

Antes de executar a pipeline, os arquivos CSV devem estar disponíveis no volume de landing do Databricks:

`/Volumes/v_credit_medallion/bronze/landing/`

O diretório deve conter todos os arquivos esperados pelo notebook `ingest_to_bronze.ipynb`, com os nomes definidos na configuração de ingestão.

### Infraestrutura e permissões

O workspace de destino deve possuir o catálogo, os schemas, o volume e as permissões necessários para a execução da pipeline.

## Parâmetros

O Job possui o parâmetro `ambiente`, com valor padrão `prod`, utilizado pelos notebooks para identificar o ambiente de execução.

## Análise exploratória

O notebook `csv_ingestion_diagnostics.ipynb` reúne os códigos utilizados na investigação inicial das fontes de dados. Ele auxilia na identificação dos *encodings* dos arquivos e na avaliação de características como duplicatas e chaves candidatas, que podem influenciar a escolha da estratégia de carga.

Esse notebook tem finalidade diagnóstica e não faz parte da sequência regular de execução do Job.

## Fluxo de desenvolvimento e deploy

O projeto utiliza **Databricks Asset Bundles** para versionar e gerenciar a configuração do Job.

### Fluxo de trabalho

1. **Sincronizar o repositório:** antes de começar, execute `pull` para obter as alterações mais recentes do GitHub.
2. **Desenvolver:** edite os notebooks e os arquivos de configuração na pasta do projeto.
3. **Versionar e compartilhar:** faça `commit` e `push` para registrar e compartilhar suas alterações.
4. **Aplicar alterações de configuração:** execute o `deploy` do Bundle no target `dev` quando modificar os arquivos de configuração dos recursos gerenciados pelo Bundle.

### Observações

- Os notebooks e arquivos de configuração são versionados no GitHub.
- Alterações na configuração do Job, como tarefas, dependências e parâmetros, exigem um novo `deploy` para serem aplicadas ao recurso implantado.
- Alterações feitas diretamente na interface do Databricks podem não estar refletidas no repositório e podem ser sobrescritas por um deploy posterior.
- Os arquivos CSV de origem não são incluídos automaticamente no repositório e devem estar disponíveis no volume de landing.
