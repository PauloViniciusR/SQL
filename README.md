# Estudos de SQL com Python

Repositório com notebooks de estudo sobre consultas SQL usando Python, SQLite e Pandas. O projeto utiliza uma base fictícia de alunos para praticar leitura de dados, filtros, seleção de colunas, remoção de duplicidades e agregações.

## Descrição para o GitHub

Notebooks de estudo de SQL com Python, SQLite e Pandas usando uma base fictícia de alunos para praticar consultas, filtros e agregações.

## Tecnologias utilizadas

- Python
- SQLite
- Pandas
- Jupyter Notebook
- Excel

## Estrutura do repositório

```text
.
├── 1. sql.ipynb
├── 2. where.ipynb
├── 3. groupyby.ipynb
├── BaseDados.db
└── BaseDados.xlsx
```

## Base de dados

Os arquivos `BaseDados.db` e `BaseDados.xlsx` representam uma base fictícia de alunos. Eles foram usados apenas para fins de estudo e prática de consultas SQL.

A tabela principal do banco SQLite é `dados`, com campos relacionados a:

- Identificação do aluno: `id_aluno`, `nome_aluno`, `cod_matricula`
- Contato: `e-mail`
- Acesso à plataforma: `acesso_plataforma`, `acesso_liberado`, `dias_ultimo_acesso`
- Provas: `nr_prova`, `prova_feita`, `nota_prova`

## Assuntos estudados

### 1. Introdução ao SQL com SQLite

Arquivo: `1. sql.ipynb`

Este notebook apresenta o uso básico do SQLite com Python. O foco está em conectar ao banco de dados, executar consultas SQL e transformar os resultados em DataFrames do Pandas.

Principais pontos:

- Criação de conexão com `sqlite3`
- Criação de cursor para executar comandos SQL
- Consulta completa com `SELECT *`
- Leitura dos nomes das colunas pelo `cur.description`
- Conversão dos resultados para `DataFrame`
- Seleção de colunas específicas
- Uso de alias para renomear colunas no resultado
- Remoção de registros duplicados com `DISTINCT`
- Criação da função `executa_sql()` para reutilizar consultas

Exemplos de consultas trabalhadas:

```sql
SELECT * FROM dados;
SELECT nome_aluno, cod_matricula FROM dados;
SELECT nome_aluno, [e-mail] AS email FROM dados;
SELECT DISTINCT nome_aluno, cod_matricula FROM dados;
```

### 2. Filtros com WHERE

Arquivo: `2. where.ipynb`

Este notebook aprofunda o uso da cláusula `WHERE` para filtrar registros com base em condições. O objetivo é localizar alunos conforme regras de acesso e tempo desde o último acesso à plataforma.

Principais pontos:

- Filtros com igualdade
- Combinação de condições com `AND`
- Combinação de condições com `OR`
- Filtros numéricos com operadores como `<=` e `>`
- Seleção de registros únicos com `DISTINCT`
- Aplicação de alias em colunas com nomes especiais

Exemplos de consultas trabalhadas:

```sql
SELECT *
FROM dados
WHERE acesso_plataforma = 0;

SELECT DISTINCT nome_aluno, cod_matricula, [e-mail] AS email
FROM dados
WHERE acesso_plataforma = 0
  AND dias_ultimo_acesso <= 2;

SELECT DISTINCT nome_aluno, cod_matricula, [e-mail] AS email,
       acesso_plataforma, dias_ultimo_acesso
FROM dados
WHERE acesso_plataforma = 0
   OR dias_ultimo_acesso > 10;
```

### 3. Agregações e agrupamentos

Arquivo: `3. groupyby.ipynb`

Este notebook trabalha consultas de resumo usando funções agregadoras e agrupamento de dados. O foco está em gerar indicadores a partir da tabela `dados`.

Principais pontos:

- Cálculo de mínimo, máximo e média com `MIN`, `MAX` e `AVG`
- Contagem de registros com `COUNT`
- Agrupamento com `GROUP BY`
- Ordenação de resultados com `ORDER BY`
- Filtro de valores nulos com `IS NOT NULL`
- Combinação de filtros, agregações e ordenação

Exemplos de consultas trabalhadas:

```sql
SELECT MIN(dias_ultimo_acesso) AS Minimo,
       MAX(dias_ultimo_acesso) AS Maximo,
       AVG(dias_ultimo_acesso) AS Media
FROM dados;

SELECT nome_aluno, COUNT(id_aluno)
FROM dados
GROUP BY nome_aluno;

SELECT nome_aluno, AVG(id_aluno) AS Media
FROM dados
WHERE nota_prova IS NOT NULL
GROUP BY nome_aluno
ORDER BY AVG(id_aluno) DESC;
```

## Como executar

1. Clone o repositório.
2. Abra a pasta em um ambiente com Python e Jupyter Notebook.
3. Instale as dependências principais, se necessário:

```bash
pip install pandas jupyter
```

4. Execute os notebooks na ordem:

```text
1. sql.ipynb
2. where.ipynb
3. groupyby.ipynb
```

## Objetivo do projeto

O objetivo deste repositório é organizar os estudos iniciais de SQL aplicado à análise de dados com Python. A base fictícia permite praticar consultas de forma simples, mantendo o foco na lógica SQL e na integração com Pandas.

