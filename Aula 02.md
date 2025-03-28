# Aula 02 - 28/03/2025

Comandos para manipulação do Banco de Dados

```sql
--- Exibe as bases existentes se possuir permissões
SHOW DATABASES;
```

# Criação de Bases de Dados
```sql
--- Criação da base com as consfigurações padrões doSGBD
CREATE DATABASE nomedobancodedados;

---Confere se o banco existe antes de criar
CREATE DATABASE IF NOT EXISTS nomedobanco;

---Cria o banco definido o conjunto de caracteres 
CREATE DATABASE meubanco CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci; --- IMPORTANTE DECORAR !!!
```
OUTF8MB4 é recomendado por suportar caracteres especiais e emojis, enquanto UTF8 não suporta todos os caracteres Unicode.

### Character Set:
-> Define o conjunto de caracteres que pode ser armazenado (ex.: utf8mb4 suporta caracteres Unicode completos).

### Collation:
-> Determina as regras de comparação e ordenação dos dados armazenados. Por exemplo, a collation define se a compaaração de "a" e "á" é considerada igual ou diferente, se a ordenação será case-sentive (sensível a maiúsculas/minúsculas) ou case-insensitive.

utf8mb4_general_ci -> NÃO É CASE SENSITIVE! usado apenas para Aulas
general_ci -> A = a
Unicode -> A != a

``` sql
---Alterar um banco de dados
---IMPORTATE: Não tem como mudar o nome do banco de dados do SQL
ALTER DATABASE nomebanco CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;

---Excluir um banco de dados
DROP DATABASE nomebanco;

DROP DATABASE IF EXISTS nomebanco;

--- Marcar um Banco de Dados ser utilizado como padrão para todos os demais comandos
USE nomebanco;
```
-------------------------------------------------------------------------------------------------------

### TABELAS
```sql
---Criando tabelas :
CREATE TABLE nometabela (
    nome_campo tipo atributo atributo ...,
    nome_campo tipo atributo,
    nome_campo tipo(valor),
    ...
);

---Escolhendo em qual banco vamos criar a tabela
CREATE TABLE nomebanco.nometabela (...);

---Exemplo de uma tabela
CREATE TABLE IF NOT EXISTS usuários(
    id_usuario BIGINT AUTO_INCREMENT NOT NULL UNSIGNED PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    data_nascimento DATE NULL,
    tipo CHAR(1) DEFAULT 'U',
    senha VARCHAR(255) NOT NULL,
    salario DECIMAL (14,2) DEFAULT 0.00,
    ultimo_acesso DATETIME,
    data_cadastro TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    data_exclusao TIMESTAMP
);

Nesta tabela temos os tipos de dados:
`BIGINT`: Maior numero inteiro possivel no MySQL
`VARCHAR`: Texto de tamanho variável, espaço vazios não ocupam espaço no disco
`DATE`: Armazena data no padrão AAAA-MM-DD
`CHAR`: Armazena testo ocupando todo espaço em disco mesmo não preenchido
`DECIMAL`: Numeros com esses decimais, definindo a precisão (qtdo_digitos, casas_decimais)
`DATETIME`: Armazena data e hora no padão AAAA-MM-DD hh:mm:ss
`TIMESTAMP`: Armazena data e hora como numero inteiro

Nesta tabela temos os atributos (constraints):
`AUTO_INCREMENT`: Utilizado em chaves primaria para incrementar +1 a cada novo insert autmaticamente
`NOT NULL`: Impede o campo de ficar null, obrigando seu preenchimento
`NULL`: Permite que o campo de ficar nulo (mais que vazio)
`PRIMARY KEY`: Define o campo como chave primaria
`UNIQUE`: Define que os valores do campo devem ser únicos na tablea
`DEFAULT`: Define o valor padrão para o campo se ele não for preenchido
`CURRENT_TIMESTAMP`: Preenche o campo com o dia e hora do servidor no momento da inserção
`UNSIGNED`: Não permite números negativos. "Dobra" a capacidade de números
```