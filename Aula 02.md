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

### Chave Estrangueira (FOREING KEYS - FK)
Em nossa bases vamos utilizar o padrão table_id para criação de uma chave estrangueira.
A FK representa o campo que faz a ligação entre uma tabela com outra com outra que desejamos obter dados adicionais
A FK de uma tabela de ser SEMPRE apontar e ter as mesma especificações da PK da tabela com quem vai se relacionar.


### Exemplos Práticos
```sql
---Ao criar a tabela:
FOREING KEY (coluna_fk) REFERENCES tabela_pai(id_tabela);

---EXEMPLO
CREATE TABLE telefone
(
    id_tlefone BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT;
    numero VARCHAR(20) NOT NULL,
    descricao VARCHAR(255),
    pessoa_id BIGINT UNSIGNED NOT NULL,
    FOREING KEY (pessoa_id) REFERENCES pessoas(id_pessoa)
);

```
### TIPOS DE MySQL 
--------------------------------------------------------------------------------------
### Tipo Numérico 

`TINYINT`: valor minimo -128 e valor maximo 127; 
            1bytes; minimo 0, maximo 255;

`SMALLINT`: valor minimo -32768 e valor max 32767;
            2 bytes; minimo 0, maximo 65535;

`MEDIUMINT`: valor minimo -8388608 valor max 8388607;
            3 bytes; minimo 0, maximo 16777215;

`INT`: valor minimo -2147483648 e valor maximo 	2147483647;
        4 bytes; minimo 0, maximo 4294967295;

`BIGINT`: valor minimo -2^63 e valor maximo 2^63-1
            8 bytes; minimo 0, maximo 2^64-1

-----------------------------------------------------------------------------------

### Tipo Tempo (Data e Hora)

`DATE`: "zero" value -> '0000-00-00';

`TIME`: "zero" value -> '00:00:00';

`DATETIME`: "zero" value -> '0000-00-00
                            00:00:00';

`TIMESTAMP`: "zero" value -> '0000-00-00
                            00:00:00';

`YEAR`: "zero" value -> 0000

----------------------------------------------------------------------------------

### Tipo de Texto (STRING)

`CHAR`(M): M x W bytes <= M <= 255;

`BINARY`(M): M bytes, 0 <= M <= 255;

`VARCHAR`(M), `VARBINARY` (M): L + 1 bytes se o valor da coluna 
                                0 - 255 bytes, L + 2 bytes;

`TINYBLOB`, `TINYTEXT`: L + 1 bytes onde L < 2^8;

`BLOB`, `TEXT`: L + 2 bytes, onde L < 2^16;

`MEDIUMBLOB`, `MEDIUMTEXT`: L + 3 bytes, onde L < 2^24;

`LONGBLOB`, `LONGTEXT`: L + 4 bytes, onde L < 2^32;

`ENUM` ('value1','value2',...): 1 ou 2 bytes dependem de um 
                                número com valor (65,535 values maximo);

`SET`('value1','value2',...): 1, 2, 3, 4 ou 8 dependem de um numero
                                com o tenham a enumeração (64 membros maximos);

--------------------------------------------------------------------------------------------------------

### Tipo Binário (BLOB) e outros

`BLOB`: é um objeto binário grande que pode segurar uma grande variedade
de datas;

`VARBINARY`: uma coluna que pode ser grande o bastante para sua criação
de preferencia;

`LONGVARBINARY`: 

``

``