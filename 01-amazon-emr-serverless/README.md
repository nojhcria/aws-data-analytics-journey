# Projeto 01 - Processamento de Avaliações com Amazon EMR Serverless

## Contexto

O objetivo deste projeto foi executar um processamento de dados utilizando Apache Spark na AWS sem precisar criar ou gerenciar servidores manualmente.

Para isso, utilizei o Amazon EMR Serverless, que permite executar jobs Spark de forma totalmente gerenciada pela AWS.

---

## Problema

Uma base de dados contendo avaliações de produtos precisava ser processada.

A solução deveria:

* Armazenar os arquivos na AWS
* Executar um processamento utilizando Spark
* Gerar arquivos de saída
* Armazenar os resultados novamente no Amazon S3

---

## Arquitetura Utilizada

```text
dataset_en_dev.json
        ↓
Amazon S3
        ↓
Amazon EMR Serverless
        ↓
Apache Spark (reviews.py)
        ↓
Amazon S3 (resultado)
```

---

## Serviços Utilizados

### Amazon S3

Responsável pelo armazenamento dos arquivos.

Bucket criado:

```text
emrtutorial0826
```

Estrutura utilizada:

```text
emrtutorial0826
└── input
    ├── dataset_en_dev.json
    └── reviews.py
```

---

### IAM

Responsável pelo controle de permissões.

Role criada:

```text
emrstudio
```

Também foi configurada uma Trust Policy para permitir que o Amazon EMR utilizasse a Role durante a execução dos jobs.

---

### Amazon VPC

Foi criada uma VPC para utilização futura com Amazon EMR em EC2.

Configuração utilizada:

* 1 Availability Zone
* 1 Public Subnet
* 0 Private Subnets
* Sem NAT Gateway
* Sem Endpoints

Nome:

```text
emrtutorial
```

---

### Apache Spark

Framework responsável pelo processamento dos dados.

Script utilizado:

```text
reviews.py
```

---

### Amazon EMR Serverless

Serviço utilizado para executar aplicações Spark sem necessidade de criar ou gerenciar servidores.

Aplicação criada:

```text
emrtutorialnojh
```

---

## Implementação

### Etapa 1 - Criação do Bucket S3

Foi criado um bucket para armazenar os arquivos do projeto.

Bucket:

```text
emrtutorial0826
```

---

### Etapa 2 - Upload dos Arquivos

Foram enviados os arquivos:

```text
dataset_en_dev.json
reviews.py
```

para a pasta:

```text
input
```

---

### Etapa 3 - Configuração IAM

Foi criada a Role:

```text
emrstudio
```

Permissões adicionadas:

* AmazonS3FullAccess
* Policy Inline personalizada (Studio_inlinepolicy)

Também foi configurada a Trust Policy utilizando:

Região:

```text
sa-east-1
```

---

### Etapa 4 - Execução do Job Spark

Script executado:

```text
reviews.py
```

Arquivo de entrada:

```text
dataset_en_dev.json
```

Resultado:

Job executado com sucesso.

---

## Problemas Encontrados

### Erro AccessDenied

Durante a primeira execução do Job foi retornado:

```text
AccessDenied
```

### Causa

A Role utilizada pelo EMR não possuía permissões suficientes para acessar os arquivos armazenados no Amazon S3.

### Correção

Foram adicionadas permissões:

```text
s3:GetObject
s3:PutObject
s3:ListBucket
s3:DeleteObject
```

### Resultado

Após a correção das permissões o Job foi executado com sucesso.

Status:

```text
SUCCESS
```

---

## Tentativa com Amazon EMR em EC2

Após concluir o ambiente Serverless, foi iniciada a criação de um cluster Amazon EMR em EC2.

Durante a criação ocorreu o erro:

```text
Service role emrstudio has insufficient EC2 permissions
```

Também foi apresentado erro relacionado à criação de Security Groups:

```text
ec2:CreateSecurityGroup
```

Ações realizadas:

* Revisão da Trust Policy
* Revisão das Policies
* Revisão da VPC
* Revisão do Instance Profile
* Comparação com as instruções do laboratório

Resultado:

Problema não resolvido até o momento.

Status:

```text
Em análise
```

---

## Principais Aprendizados

Durante este projeto aprendi:

* Como criar buckets Amazon S3
* Como armazenar datasets e scripts na AWS
* Como IAM controla permissões
* Como Roles e Policies funcionam
* Como Trust Policies funcionam
* Como o Amazon EMR Serverless executa aplicações Spark
* Como investigar erros de permissão na AWS
* Diferença entre EMR Serverless e EMR executando sobre EC2

---

## Resultado Final

Foi possível:

✅ Criar bucket Amazon S3

✅ Fazer upload dos arquivos

✅ Configurar IAM

✅ Configurar Trust Policy

✅ Criar aplicação EMR Serverless

✅ Executar Job Spark

✅ Resolver erro AccessDenied

⚠️ Amazon EMR em EC2 ainda pendente para investigação futura
