# Estudos de SQL com Python

Repositório com notebooks de estudo sobre SQL aplicado à análise de dados usando Python, SQLite e Pandas. O material segue uma evolução prática: começa com consultas simples em uma única tabela, passa por filtros, agregações e condicionais, e termina com junções, `UNION` e simulação de `FULL JOIN`.

## Descrição para o GitHub

Notebooks de SQL com Python, SQLite e Pandas usando bases fictícias de alunos para praticar consultas, filtros, agregações, subqueries, joins, union e full join.

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
├── 3. group by.ipynb
├── 4. having.ipynb
├── 5. case.ipynb
├── 6. subquery.ipynb
├── 7. in and like.ipynb
├── 8. join.ipynb
├── 9. join2.ipynb
├── 10. union e fulljoin.ipynb
├── README.md
└── data
    ├── BaseDados.db
    ├── BaseDados.xlsx
    ├── BaseDados2.db
    ├── BaseDados2.xlsx
    └── BaseDados3.db
```

## Bases de dados

As bases são fictícias e foram usadas apenas para fins de estudo.

### `data/BaseDados.db`

Banco usado nos notebooks iniciais. Possui a tabela `dados`, com informações consolidadas de alunos, acesso à plataforma e provas.

Campos principais:

- Identificação do aluno: `id_aluno`, `nome_aluno`, `cod_matricula`
- Contato: `e-mail`
- Acesso à plataforma: `acesso_plataforma`, `acesso_liberado`, `dias_ultimo_acesso`
- Provas: `nr_prova`, `prova_feita`, `nota_prova`

### `data/BaseDados2.db`

Banco usado nos notebooks de `JOIN`. Possui dados separados em tabelas relacionais:

- `alunos`: dados cadastrais dos alunos
- `plataforma`: informações de acesso à plataforma
- `provas`: provas liberadas e notas dos alunos
- `base`: tabela consolidada de apoio

### `data/BaseDados3.db`

Banco usado no notebook de `UNION` e `FULL JOIN`. Possui:

- `alunos`: alunos já existentes na base
- `alunos_novos`: alunos adicionados após o início do mês
- `plataforma`: informações de acesso à plataforma

## Assuntos estudados

### 1. Introdução ao SQL com SQLite

Arquivo: `1. sql.ipynb`

Apresenta a conexão com SQLite usando Python, execução de consultas SQL e conversão dos resultados para DataFrames do Pandas.

Principais pontos:

- Conexão com `sqlite3`
- Criação de cursor
- Consulta com `SELECT *`
- Seleção de colunas específicas
- Uso de alias com `AS`
- Remoção de duplicidades com `DISTINCT`
- Criação da função `executa_sql()` para reutilizar consultas

### 2. Filtros com `WHERE`

Arquivo: `2. where.ipynb`

Trabalha filtros em registros com base em condições de acesso, liberação da plataforma e tempo desde o último acesso.

Principais pontos:

- Filtros com igualdade
- Condições com `AND` e `OR`
- Operadores numéricos como `<=` e `>`
- Seleção de registros únicos
- Alias para colunas com caracteres especiais

### 3. Agregações e agrupamentos

Arquivo: `3. group by.ipynb`

Aplica funções agregadoras para gerar indicadores a partir dos dados.

Principais pontos:

- `MIN`, `MAX` e `AVG`
- `COUNT`
- `GROUP BY`
- `ORDER BY`
- Filtro de valores nulos com `IS NOT NULL`

### 4. Filtros em agregações com `HAVING`

Arquivo: `4. having.ipynb`

Mostra a diferença entre filtrar linhas antes da agregação com `WHERE` e filtrar resultados agregados com `HAVING`.

Principais pontos:

- Ordem lógica de execução de uma consulta SQL
- Agrupamento com filtros agregados
- Limitação de resultados com `LIMIT`
- Ordenação com `ASC` e `DESC`

### 5. Condicionais com `CASE`

Arquivo: `5. case.ipynb`

Cria regras condicionais diretamente na consulta SQL para classificar alunos conforme notas e quantidade de provas.

Principais pontos:

- Estrutura `CASE WHEN THEN END`
- Criação de colunas calculadas
- Classificação de alunos por regra de negócio
- Combinação de condições numéricas e agregações

### 6. Subqueries

Arquivo: `6. subquery.ipynb`

Usa consultas internas para resolver problemas que dependem de resultados intermediários.

Principais pontos:

- Query dentro de query
- Subqueries para cálculos e filtros
- Reaproveitamento de lógicas criadas em consultas anteriores

### 7. Filtros com `IN` e `LIKE`

Arquivo: `7. in and like.ipynb`

Apresenta filtros para listas de valores e padrões de texto.

Principais pontos:

- `IN` para verificar valores dentro de uma lista
- `LIKE` para buscar padrões em strings
- Filtros textuais com curingas

### 8. Introdução a `JOIN`

Arquivo: `8. join.ipynb`

Introduz junções entre tabelas relacionais usando a base `BaseDados2.db`.

Principais pontos:

- Relação entre `alunos`, `plataforma` e `provas`
- Comparação entre `JOIN` no SQL e `merge()` no Pandas
- Junção por chave com `cod_matricula`
- Uso de `INNER JOIN`, `LEFT JOIN` e variações

### 9. Prática com `JOIN`

Arquivo: `9. join2.ipynb`

Aprofunda o uso de junções com consultas mais completas entre as tabelas da base relacional.

Principais pontos:

- Junções com múltiplas tabelas
- Seleção de campos de tabelas diferentes
- Uso de aliases para tabelas
- Consolidação de dados cadastrais, acesso e provas

### 10. `UNION` e `FULL JOIN`

Arquivo: `10. union e fulljoin.ipynb`

Trabalha união vertical de bases e simulação de `FULL JOIN` no SQLite.

Principais pontos:

- `UNION`
- `UNION ALL`
- Comparação entre união de linhas e junção de colunas
- Simulação de `FULL JOIN` com combinação de `LEFT JOIN` e `UNION`

## Como executar

1. Abra a pasta do projeto em um ambiente com Python e Jupyter Notebook.
2. Instale as dependências principais, se necessário:

```bash
pip install pandas jupyter
```

3. Execute os notebooks na ordem:

```text
1. sql.ipynb
2. where.ipynb
3. group by.ipynb
4. having.ipynb
5. case.ipynb
6. subquery.ipynb
7. in and like.ipynb
8. join.ipynb
9. join2.ipynb
10. union e fulljoin.ipynb
```

## Observações importantes

- Os bancos SQLite ficam na pasta `data`.
- Os notebooks 8 e 9 usam `data/BaseDados2.db`.
- O notebook 10 usa `data/BaseDados3.db`.
- Caso um notebook antigo use um caminho sem `data/`, ajuste a conexão para o caminho correto.

Exemplo:

```python
import sqlite3

con = sqlite3.connect("data/BaseDados3.db")
```

## Objetivo do projeto

Organizar os estudos iniciais de SQL aplicado à análise de dados com Python. As bases fictícias permitem praticar consultas de forma progressiva, mantendo o foco na lógica SQL, na modelagem relacional simples e na integração com Pandas.
