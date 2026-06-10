# Explorando Cláusulas SQL

---

# 1. O que são Cláusulas?

## 1.1 Definição

Uma cláusula é uma parte de uma instrução SQL.

Cada cláusula possui uma responsabilidade específica dentro da consulta.

As cláusulas trabalham juntas para informar ao banco de dados:

- quais dados devem ser buscados
- de onde os dados serão obtidos
- quais registros devem ser filtrados
- como os dados devem ser agrupados
- como os resultados devem ser ordenados
- quantos registros devem ser retornados

---

## 1.2 Exemplo

```sql
SELECT NOME
FROM ALUNO
WHERE IDADE >= 16
ORDER BY NOME;
```

---

## 1.3 Identificando as Cláusulas

| Cláusula | Função |
|---|---|
| SELECT | escolhe colunas |
| FROM | define a tabela |
| WHERE | filtra registros |
| ORDER BY | ordena resultados |

---

## 1.4 Estrutura Geral

```sql
SELECT COLUNAS
FROM TABELA
WHERE CONDICAO
GROUP BY COLUNA
HAVING CONDICAO
ORDER BY COLUNA
LIMIT QUANTIDADE;
```

---

## 1.5 Cláusulas Mais Utilizadas

| Cláusula | Objetivo |
|---|---|
| SELECT | selecionar colunas |
| FROM | definir tabela |
| WHERE | filtrar registros |
| DISTINCT | remover duplicados |
| GROUP BY | agrupar registros |
| HAVING | filtrar grupos |
| ORDER BY | ordenar resultados |
| LIMIT | limitar registros |
| OFFSET | ignorar registros |
| JOIN | relacionar tabelas |
| ON | definir relacionamento |
| AS | criar apelidos |
| UNION | unir consultas |
| UNION ALL | unir consultas mantendo duplicados |
| EXISTS | verificar existência |

---

## 1.6 Cláusulas vs Funções

Nem tudo em SQL é uma cláusula.

Existem também funções.

---

### Cláusulas

```sql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
```

---

### Funções

```sql
COUNT()
SUM()
AVG()
MAX()
MIN()
```

---

## Diferença

| Tipo | Exemplo |
|---|---|
| Cláusula | WHERE |
| Cláusula | GROUP BY |
| Cláusula | ORDER BY |
| Função | COUNT() |
| Função | SUM() |
| Função | AVG() |

---

# 2. SELECT

## 2.1 O que é SELECT?

O `SELECT` é utilizado para escolher quais colunas serão exibidas no resultado.

---

### Exemplo

```sql
SELECT NOME
FROM ALUNO;
```

---

### Resultado

| NOME |
|---|
| Ana |
| João |
| Maria |

---

## 2.2 Selecionando Múltiplas Colunas

```sql
SELECT NOME, IDADE
FROM ALUNO;
```

---

## 2.3 Selecionando Todas as Colunas

```sql
SELECT *
FROM ALUNO;
```

---

# 3. FROM

## 3.1 O que é FROM?

O `FROM` define de qual tabela os dados serão obtidos.

---

### Exemplo

```sql
SELECT *
FROM ALUNO;
```

---

### Explicação

O banco procura os dados dentro da tabela:

```text
ALUNO
```

---

# 4. WHERE

## 4.1 O que é WHERE?

O `WHERE` é utilizado para filtrar registros.

---

### Exemplo

```sql
SELECT *
FROM ALUNO
WHERE IDADE >= 16;
```

---

### Resultado

Retorna apenas alunos com idade maior ou igual a 16.

---

## 4.2 Operadores

| Operador | Significado |
|---|---|
| = | igual |
| != | diferente |
| > | maior |
| < | menor |
| >= | maior ou igual |
| <= | menor ou igual |

---

## 4.3 AND

```sql
SELECT *
FROM ALUNO
WHERE IDADE >= 16
AND FK_TURMA = 1;
```

---

## 4.4 OR

```sql
SELECT *
FROM ALUNO
WHERE FK_TURMA = 1
OR FK_TURMA = 2;
```

---

## 4.5 NOT

```sql
SELECT *
FROM ALUNO
WHERE NOT FK_TURMA = 1;
```

---

## 4.6 LIKE

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

### Contém AR

```sql
SELECT *
FROM ALUNO
WHERE NOME LIKE '%AR%';
```

---

## 4.7 IN

```sql
SELECT *
FROM ALUNO
WHERE FK_TURMA IN (1,2,3);
```

---

## 4.8 BETWEEN

```sql
SELECT *
FROM ALUNO
WHERE IDADE BETWEEN 15 AND 18;
```

---

## 4.9 IS NULL

```sql
SELECT *
FROM ALUNO
WHERE FK_TURMA IS NULL;
```

---

# 5. DISTINCT

## 5.1 O que é DISTINCT?

Remove registros duplicados.

---

### Tabela

| FK_TURMA |
|---|
| 1 |
| 1 |
| 1 |
| 2 |
| 2 |

---

### Consulta

```sql
SELECT DISTINCT FK_TURMA
FROM ALUNO;
```

---

### Resultado

| FK_TURMA |
|---|
| 1 |
| 2 |

---

# 6. ORDER BY

## 6.1 O que é ORDER BY?

O `ORDER BY` é utilizado para ordenar resultados.

---

## Ordem Crescente

```sql
SELECT *
FROM ALUNO
ORDER BY NOME ASC;
```

---

## Ordem Decrescente

```sql
SELECT *
FROM ALUNO
ORDER BY NOME DESC;
```

---

## Ordenando por Mais de Uma Coluna

```sql
SELECT *
FROM ALUNO
ORDER BY FK_TURMA, NOME;
```

---

# 7. LIMIT

## 7.1 O que é LIMIT?

O `LIMIT` restringe a quantidade de registros retornados.

---

### Exemplo

```sql
SELECT *
FROM ALUNO
LIMIT 5;
```

---

### Resultado

Retorna apenas os 5 primeiros registros.

---

# 8. OFFSET

## 8.1 O que é OFFSET?

O `OFFSET` ignora registros antes de retornar os resultados.

---

### Exemplo

```sql
SELECT *
FROM ALUNO
LIMIT 10 OFFSET 20;
```

---

### Explicação

Ignora:

```text
20 registros
```

Depois retorna:

```text
10 registros
```

---

# 9. GROUP BY

## 9.1 O que é GROUP BY?

O `GROUP BY` agrupa registros que possuem valores iguais.

---

### Tabela

| ID | NOME | FK_TURMA |
|---|---|---|
| 1 | Ana | 1 |
| 2 | João | 1 |
| 3 | Maria | 2 |
| 4 | Carlos | 2 |
| 5 | Pedro | 2 |

---

### Consulta

```sql
SELECT FK_TURMA, COUNT(*)
FROM ALUNO
GROUP BY FK_TURMA;
```

---

### Resultado

| FK_TURMA | TOTAL |
|---|---|
| 1 | 2 |
| 2 | 3 |

---

### O que aconteceu?

O banco criou grupos com base na coluna:

```text
FK_TURMA
```

Depois contou quantos registros existem em cada grupo.

---

# 10. HAVING

## 10.1 O que é HAVING?

O `HAVING` é utilizado para filtrar grupos criados pelo `GROUP BY`.

---

## Diferença Entre WHERE e HAVING

| WHERE | HAVING |
|---|---|
| Filtra registros | Filtra grupos |
| Antes do GROUP BY | Depois do GROUP BY |

---

### Exemplo

```sql
SELECT FK_TURMA, COUNT(*)
FROM ALUNO
GROUP BY FK_TURMA
HAVING COUNT(*) >= 3;
```

---

### Resultado

| FK_TURMA | TOTAL |
|---|---|
| 2 | 3 |

---

### O que aconteceu?

Primeiro:

```sql
GROUP BY FK_TURMA
```

Cria os grupos.

---

Depois:

```sql
HAVING COUNT(*) >= 3
```

Filtra os grupos.

---

# 11. AS (Alias)

## 11.1 O que é AS?

O `AS` cria apelidos para colunas ou tabelas.

---

### Exemplo com Coluna

```sql
SELECT NOME AS ALUNO
FROM ALUNO;
```

---

### Resultado

| ALUNO |
|---|
| Ana |
| João |

---

### Exemplo com Tabela

```sql
SELECT A.NOME
FROM ALUNO AS A;
```

---

# 12. JOIN

## 12.1 O que é JOIN?

O `JOIN` relaciona tabelas.

---

### Exemplo

```sql
SELECT A.NOME, T.NOME
FROM ALUNO A
INNER JOIN TURMA T
ON A.FK_TURMA = T.ID;
```

---

### Resultado

| ALUNO | TURMA |
|---|---|
| Ana | A |
| João | B |

---

# 13. ON

## 13.1 O que é ON?

O `ON` define a condição de relacionamento utilizada pelo JOIN.

---

### Exemplo

```sql
ON ALUNO.FK_TURMA = TURMA.ID
```

---

### Explicação

Relaciona:

```text
ALUNO.FK_TURMA
```

com

```text
TURMA.ID
```

---

# 14. UNION

## 14.1 O que é UNION?

Une resultados de consultas.

---

### Exemplo

```sql
SELECT NOME
FROM ALUNO

UNION

SELECT NOME
FROM PROFESSOR;
```

---

### Importante

Remove registros duplicados.

---

# 15. UNION ALL

## 15.1 O que é UNION ALL?

Também une consultas.

---

### Exemplo

```sql
SELECT NOME
FROM ALUNO

UNION ALL

SELECT NOME
FROM PROFESSOR;
```

---

### Diferença

| UNION | UNION ALL |
|---|---|
| Remove duplicados | Mantém duplicados |

---

# 16. EXISTS

## 16.1 O que é EXISTS?

Verifica se uma subconsulta retorna registros.

---

### Exemplo

```sql
SELECT *
FROM CLIENTE C
WHERE EXISTS
(
    SELECT 1
    FROM PEDIDO P
    WHERE P.FK_CLIENTE = C.ID
);
```

---

### Explicação

Retorna apenas clientes que possuem pedidos.

---

# 17. Ordem de Execução do SQL

Considere a consulta:

```sql
SELECT FK_TURMA, COUNT(*)
FROM ALUNO
WHERE IDADE >= 15
GROUP BY FK_TURMA
HAVING COUNT(*) >= 2
ORDER BY FK_TURMA
LIMIT 5;
```

---

## Ordem Real

```text
1. FROM
2. JOIN
3. ON
4. WHERE
5. GROUP BY
6. HAVING
7. SELECT
8. ORDER BY
9. LIMIT
```

---

- Cláusulas trabalham juntas para construir consultas SQL

---
