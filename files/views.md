# Banco de Dados - Views

## O que é uma View?

Uma **View** é uma consulta SQL armazenada no banco de dados que pode ser utilizada como se fosse uma tabela.

Ela é criada a partir de um comando `SELECT` e tem como objetivo simplificar consultas, reutilizar código e facilitar o acesso às informações.

Uma View é frequentemente chamada de **tabela virtual**, pois não armazena os dados da consulta. Ela armazena apenas a instrução SQL utilizada para gerar os resultados.

---

## Para que serve uma View?

As Views são utilizadas para:

- Simplificar consultas complexas;
- Reutilizar consultas frequentes;
- Facilitar a manutenção do banco de dados;
- Controlar quais informações serão exibidas aos usuários;
- Ocultar detalhes da estrutura das tabelas.

Imagine uma consulta que utiliza várias tabelas e relacionamentos. Em vez de escrever toda a consulta novamente, podemos criar uma View e utilizá-la sempre que necessário.

---

## Sintaxe

```sql
CREATE VIEW nome_view AS
SELECT colunas
FROM tabela;
```

### Exemplo

```sql
CREATE VIEW alunos_maiores AS
SELECT
    id,
    nome,
    idade
FROM alunos
WHERE idade >= 18;
```

---

## Consultando uma View

Após ser criada, uma View pode ser consultada da mesma forma que uma tabela.

```sql
SELECT *
FROM alunos_maiores;
```

Também é possível aplicar filtros:

```sql
SELECT *
FROM alunos_maiores
WHERE nome LIKE 'C%';
```

---

## Exemplo com JOIN

Uma das aplicações mais comuns das Views é simplificar consultas com relacionamentos.

### Tabelas

```sql
CREATE TABLE alunos (
    id INT PRIMARY KEY,
    nome VARCHAR(100)
);
```

```sql
CREATE TABLE cursos (
    id INT PRIMARY KEY,
    nome VARCHAR(100)
);
```

```sql
CREATE TABLE matriculas (
    aluno_id INT,
    curso_id INT
);
```

### View

```sql
CREATE VIEW alunos_matriculados AS
SELECT
    alunos.nome AS aluno,
    cursos.nome AS curso
FROM alunos
INNER JOIN matriculas
    ON matriculas.aluno_id = alunos.id
INNER JOIN cursos
    ON matriculas.curso_id = cursos.id;
```

### Consulta

```sql
SELECT *
FROM alunos_matriculados;
```

---

## Alterando uma View

Para modificar uma View existente:

```sql
CREATE OR REPLACE VIEW alunos_maiores AS
SELECT
    id,
    nome,
    idade
FROM alunos
WHERE idade >= 16;
```

---

## Excluindo uma View

```sql
DROP VIEW alunos_maiores;
```

A exclusão da View não remove os dados das tabelas utilizadas.

---

## Vantagens das Views

- Reduzem a complexidade das consultas;
- Evitam repetição de código SQL;
- Facilitam a manutenção do sistema;
- Aumentam a segurança ao restringir colunas visíveis;
- Melhoram a organização do banco de dados.

---

## View x Tabela

| View | Tabela |
|--------|--------|
| Consulta armazenada | Armazena dados |
| Tabela virtual | Tabela física |
| Baseada em SELECT | Contém registros |
| Não duplica dados | Armazena dados permanentemente |

---

## Resumo

Uma **View** é uma consulta SQL armazenada no banco de dados que funciona como uma tabela virtual.

Principais comandos:

```sql
CREATE VIEW
```

Cria uma View.

```sql
CREATE OR REPLACE VIEW
```

Cria ou altera uma View.

```sql
SELECT
```

Consulta uma View.

```sql
DROP VIEW
```

Remove uma View.

---
