# Explorando JOINs no SQL

---

# 1. JOIN vs WHERE

## 1.1 O que é JOIN?

O `JOIN` é utilizado para relacionar tabelas.

Ele conecta informações que estão separadas em diferentes tabelas do banco de dados.

---

## 1.2 O que é WHERE?

O `WHERE` é utilizado para filtrar registros.

Ele define quais dados devem aparecer no resultado da consulta.

---

## 1.3 Diferença Principal

| JOIN | WHERE |
|---|---|
| Relaciona tabelas | Filtra registros |
| Conecta dados | Seleciona resultados |
| Usa `ON` | Usa condições |
| Une informações | Restringe informações |

---

## 1.4 Exemplo

### Tabela: `TURMA`

| ID | NOME |
|---|---|
| 1 | A |
| 2 | B |

---

### Tabela: `ALUNO`

| ID | NOME | FK_TURMA |
|---|---|---|
| 1 | Ana | 1 |
| 2 | João | 2 |
| 3 | Maria | 1 |

---

A tabela `ALUNO` possui apenas o código da turma.

O nome da turma está armazenado na tabela `TURMA`.

---

## 1.5 JOIN na Prática

```sql
SELECT ALUNO.NOME, TURMA.NOME
FROM ALUNO
JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID;
```

---

### Resultado

| NOME | TURMA |
|---|---|
| Ana | A |
| João | B |
| Maria | A |

---

### O que aconteceu?

O `JOIN`:
- conectou as tabelas
- relacionou:
  - `ALUNO.FK_TURMA`
  - `TURMA.ID`

Assim, foi possível exibir o nome da turma junto do nome do aluno.

---

## 1.6 WHERE na Prática

```sql
SELECT ALUNO.NOME, TURMA.NOME
FROM ALUNO
JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID
WHERE TURMA.NOME = 'A';
```

---

### Resultado

| NOME | TURMA |
|---|---|
| Ana | A |
| Maria | A |

---

### O que aconteceu?

- o `JOIN` conectou as tabelas
- o `WHERE` filtrou apenas alunos da turma A

---

## 1.7 Ordem Simplificada do SQL

O SQL funciona, de forma simplificada, nesta ordem:

1. `FROM`
2. `JOIN`
3. `ON`
4. `WHERE`
5. `SELECT`

---

### Interpretação

Primeiro:
- as tabelas são conectadas

Depois:
- os registros são filtrados

Por fim:
- as colunas são exibidas

---

## 1.8 Sintaxe Antiga vs Moderna

### Sintaxe Antiga

```sql
SELECT ALUNO.NOME, TURMA.NOME
FROM ALUNO, TURMA
WHERE ALUNO.FK_TURMA = TURMA.ID;
```

---

### Problemas

- mistura relacionamento com filtro
- reduz legibilidade
- dificulta manutenção
- aumenta chance de erro

---

### Sintaxe Moderna

```sql
SELECT ALUNO.NOME, TURMA.NOME
FROM ALUNO
JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID;
```

---

### Vantagens

- mais organizada
- mais legível
- separa relacionamento de filtro
- facilita manutenção
- padrão utilizado atualmente

---

# 2. Estrutura dos Comandos

## 2.1 Estrutura do JOIN

```sql
SELECT COLUNAS
FROM TABELA1
JOIN TABELA2
  ON TABELA1.COLUNA = TABELA2.COLUNA;
```

---

## 2.2 Exemplo Real

### Tabela: `CLIENTE`

| ID | NOME |
|---|---|
| 1 | Carlos |
| 2 | Fernanda |

---

### Tabela: `PEDIDO`

| ID | VALOR | FK_CLIENTE |
|---|---|---|
| 1 | 500 | 1 |
| 2 | 300 | 2 |
| 3 | 900 | 1 |

---

### Consulta

```sql
SELECT CLIENTE.NOME, PEDIDO.VALOR
FROM PEDIDO
JOIN CLIENTE
  ON PEDIDO.FK_CLIENTE = CLIENTE.ID;
```

---

### Resultado

| CLIENTE | VALOR |
|---|---|
| Carlos | 500 |
| Fernanda | 300 |
| Carlos | 900 |

---

### Explicação

| Parte | Função |
|---|---|
| `FROM PEDIDO` | tabela principal |
| `JOIN CLIENTE` | conecta CLIENTE |
| `ON PEDIDO.FK_CLIENTE = CLIENTE.ID` | cria relacionamento |
| `SELECT` | escolhe colunas |

---

## 2.3 Estrutura do WHERE

```sql
SELECT COLUNAS
FROM TABELA
WHERE CONDICAO;
```

---

## 2.4 Exemplo Real

### Tabela: `PRODUTO`

| ID | NOME | PRECO |
|---|---|---|
| 1 | Notebook | 3500 |
| 2 | Mouse | 80 |
| 3 | Monitor | 1200 |

---

### Consulta

```sql
SELECT *
FROM PRODUTO
WHERE PRECO > 1000;
```

---

### Resultado

| ID | NOME | PRECO |
|---|---|---|
| 1 | Notebook | 3500 |
| 3 | Monitor | 1200 |

---

### Explicação

O `WHERE` filtrou apenas produtos com preço acima de 1000.

---

# 3. Operadores do WHERE

## 3.1 Igual (`=`)

```sql
SELECT *
FROM ALUNO
WHERE NOME = 'ANA';
```

---

## 3.2 Diferente (`!=`)

```sql
SELECT *
FROM ALUNO
WHERE NOME != 'ANA';
```

---

## 3.3 Maior que (`>`)

```sql
SELECT *
FROM ALUNO
WHERE IDADE > 15;
```

---

## 3.4 Menor que (`<`)

```sql
SELECT *
FROM ALUNO
WHERE IDADE < 17;
```

---

## 3.5 Maior ou igual (`>=`)

```sql
SELECT *
FROM ALUNO
WHERE IDADE >= 16;
```

---

## 3.6 Menor ou igual (`<=`)

```sql
SELECT *
FROM ALUNO
WHERE IDADE <= 15;
```

---

## 3.7 AND

```sql
SELECT *
FROM ALUNO
WHERE IDADE >= 15
AND FK_TURMA = 1;
```

---

## 3.8 OR

```sql
SELECT *
FROM ALUNO
WHERE FK_TURMA = 1
OR FK_TURMA = 2;
```

---

## 3.9 NOT

```sql
SELECT *
FROM ALUNO
WHERE NOT FK_TURMA = 1;
```

---

## 3.10 LIKE

### Começa com A

```sql
SELECT *
FROM ALUNO
WHERE NOME LIKE 'A%';
```

---

### Termina com A

```sql
SELECT *
FROM ALUNO
WHERE NOME LIKE '%A';
```

---

### Contém "AR"

```sql
SELECT *
FROM ALUNO
WHERE NOME LIKE '%AR%';
```

---

## 3.11 IN

```sql
SELECT *
FROM ALUNO
WHERE FK_TURMA IN (1, 2);
```

---

## 3.12 BETWEEN

```sql
SELECT *
FROM ALUNO
WHERE IDADE BETWEEN 15 AND 16;
```

---

## 3.13 IS NULL

```sql
SELECT *
FROM ALUNO
WHERE FK_TURMA IS NULL;
```

---

# 4. Tipos de JOIN

Para entender os JOINs, utilizaremos sempre as mesmas tabelas.

---

## Tabela: `ALUNO`

| ID | NOME | FK_TURMA |
|---|---|---|
| 1 | Ana | 1 |
| 2 | João | 2 |
| 3 | Carlos | NULL |

---

## Tabela: `TURMA`

| ID | NOME |
|---|---|
| 1 | A |
| 2 | B |
| 3 | C |

---

# 4.1 INNER JOIN

## Definição

Retorna apenas registros que possuem relacionamento nas duas tabelas.

---

## Consulta

```sql
SELECT ALUNO.NOME, TURMA.NOME
FROM ALUNO
INNER JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID;
```

---

## Relacionamento

| ALUNO.FK_TURMA | TURMA.ID | Resultado |
|---|---|---|
| 1 | 1 | Ana → A |
| 2 | 2 | João → B |
| NULL | ? | não aparece |

---

## Resultado

| ALUNO | TURMA |
|---|---|
| Ana | A |
| João | B |

---

## Resumo

Mostra apenas registros relacionados.

---

> `INNER JOIN` e apenas `JOIN` são equivalentes.
>
> Os dois comandos produzem o mesmo resultado.
>
> Exemplo:
>
> ```sql
> JOIN
> ```
>
> é a forma reduzida de:
>
> ```sql
> INNER JOIN
> ```

---

# 4.2 LEFT JOIN

## Definição

Retorna:
- todos os registros da esquerda
- apenas os relacionados da direita

---

## Consulta

```sql
SELECT ALUNO.NOME, TURMA.NOME
FROM ALUNO
LEFT JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID;
```

---

## Relacionamento

| ALUNO.NOME | ALUNO.FK_TURMA | TURMA.ID | TURMA.NOME |
|---|---|---|---|
| Ana | 1 | 1 | A |
| João | 2 | 2 | B |
| Carlos | NULL | NULL | NULL |

---

## Resultado

| ALUNO | TURMA |
|---|---|
| Ana | A |
| João | B |
| Carlos | NULL |

---

## Resumo

Mostra tudo da tabela da esquerda (`ALUNO`).

---

# 4.3 RIGHT JOIN

## Definição

Retorna:
- todos os registros da direita
- apenas os relacionados da esquerda

---

## Consulta

```sql
SELECT ALUNO.NOME, TURMA.NOME
FROM ALUNO
RIGHT JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID;
```

---

## Relacionamento

| ALUNO.NOME | ALUNO.FK_TURMA | TURMA.ID | TURMA.NOME |
|---|---|---|---|
| Ana | 1 | 1 | A |
| João | 2 | 2 | B |
| NULL | NULL | 3 | C |

---

## Resultado

| ALUNO | TURMA |
|---|---|
| Ana | A |
| João | B |
| NULL | C |

---

## Resumo

Mostra tudo da tabela da direita (`TURMA`).

---

# 4.4 FULL JOIN

## Definição

Retorna:
- todos os registros das duas tabelas
- relacionados ou não

---

## Consulta

```sql
SELECT ALUNO.NOME, TURMA.NOME
FROM ALUNO
FULL JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID;
```

---

## Relacionamento

| ALUNO.NOME | ALUNO.FK_TURMA | TURMA.ID | TURMA.NOME |
|---|---|---|---|
| Ana | 1 | 1 | A |
| João | 2 | 2 | B |
| Carlos | NULL | NULL | NULL |
| NULL | NULL | 3 | C |

---

## Resultado

| ALUNO | TURMA |
|---|---|
| Ana | A |
| João | B |
| Carlos | NULL |
| NULL | C |

---

## Resumo

Mostra tudo das duas tabelas.

---

# 4.5 Comparação Geral

| JOIN | O que retorna? |
|---|---|
| INNER JOIN | apenas registros relacionados |
| LEFT JOIN | tudo da esquerda |
| RIGHT JOIN | tudo da direita |
| FULL JOIN | tudo das duas tabelas |

---

# 5. Alias

## 5.1 O que são Alias?

Alias são apelidos para tabelas.

Eles tornam consultas maiores mais organizadas e legíveis.

---

## 5.2 Exemplo

```sql
SELECT A.NOME, T.NOME
FROM ALUNO A
INNER JOIN TURMA T
  ON A.FK_TURMA = T.ID;
```

---

## 5.3 Interpretação

| Alias | Tabela |
|---|---|
| `A` | ALUNO |
| `T` | TURMA |

---

# 6. Exemplo Completo

## 6.1 Criando Tabelas

```sql
CREATE TABLE TURMA (
  ID INT,
  NOME VARCHAR(10)
);

CREATE TABLE ALUNO (
  ID INT,
  NOME VARCHAR(100),
  IDADE INT,
  FK_TURMA INT
);
```

---

## 6.2 Inserindo Dados

```sql
INSERT INTO TURMA VALUES (1, 'A');
INSERT INTO TURMA VALUES (2, 'B');

INSERT INTO ALUNO VALUES (1, 'ANA', 15, 1);
INSERT INTO ALUNO VALUES (2, 'JOAO', 16, 2);
INSERT INTO ALUNO VALUES (3, 'MARIA', 17, 1);
```

---

## 6.3 Realizando JOIN

```sql
SELECT ALUNO.NOME, TURMA.NOME
FROM ALUNO
INNER JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID;
```

---

### Resultado

| NOME | TURMA |
|---|---|
| ANA | A |
| JOAO | B |
| MARIA | A |

---

## 6.4 Utilizando WHERE

```sql
SELECT ALUNO.NOME, TURMA.NOME
FROM ALUNO
INNER JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID
WHERE ALUNO.IDADE >= 16;
```

---

### Resultado

| NOME | TURMA |
|---|---|
| JOAO | B |
| MARIA | A |

---

# 7. Erros Comuns

## 7.1 Esquecer o ON

```sql
SELECT *
FROM ALUNO
JOIN TURMA;
```

---

### Problema

Sem `ON`, o banco pode gerar combinações incorretas entre registros.

Isso é chamado de produto cartesiano.

---

## 7.2 Filtrar no Lugar Errado

```sql
SELECT *
FROM ALUNO
JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID
WHERE TURMA.NOME = 'A';
```

---

### Correto

- `ON` → relacionamento
- `WHERE` → filtro

---

## 7.3 Ambiguidade de Colunas

```sql
SELECT ID
FROM ALUNO
JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID;
```

---

### Problema

As duas tabelas possuem a coluna `ID`.

O banco não sabe qual delas deve retornar.

---

### Correto

```sql
SELECT ALUNO.ID
FROM ALUNO
JOIN TURMA
  ON ALUNO.FK_TURMA = TURMA.ID;
```

---

# 8. Resumo

- `JOIN` relaciona tabelas
- `WHERE` filtra registros
- `ON` define relacionamento
- `JOIN` é equivalente a `INNER JOIN`
- `INNER JOIN` mostra apenas correspondências
- `LEFT JOIN` mostra tudo da esquerda
- `RIGHT JOIN` mostra tudo da direita
- `FULL JOIN` mostra tudo das duas tabelas
- Alias melhoram legibilidade
- `JOIN` e `WHERE` normalmente trabalham juntos

---
