# Manipulação de Registros no BD
Para realizar o CRUD no BD, utilizamos os comandos CREATE, SELECT, UPDATE, DELETE.

## Inserindo Registros
```sql
--- Tabela base:
CREATE TABLE livros 
(
    id_livro BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    titulo VARCHAR(200) NOT NULL,
    autor_id BIGINT UNSIGNED,
    ano_publicado INT,
    CONSTRAINT fk_autor_livro
    FOREIGN KEY (autor_id) REFERENCES autores(id_autor)
    ON DELETE SET NULL
    ON UPDATE CASCADE -- no update OK, evite ao máximo no DELETE
);

--- Sem informar os campos (Informar todos os campo e na mesma ordem) -> Muito pouco usado
INSERT INTO livros VALUE ('O Livro', 1, 2025);

--- Informando a ordem dos campos (Mais usado)
INSERT INTO livros (autor_id, ano, titulo) VALUE (1, 2025, 'O livro');

---Inserindo vários registros de 1 vez
INSERT INTO livros (titulo, autor_id, ano) VALUE ('O livro', 1, 2025) ('Pior Livro', 2, NULL);
´´´
