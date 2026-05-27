# Exercícios: JOIN e Tabela Associativa

## Objetivo
Criar consultas SQL utilizando:
- `INNER JOIN`
- `LEFT JOIN`
- `RIGHT JOIN`
- `FULL JOIN`
- Tabela Associativa

---

# INNER JOIN

## 1. Clientes e Pedidos

### Tabela: CLIENTES

| ID | NOME     |
|----|-----------|
| 1  | CARLOS    |
| 2  | MARIANA   |
| 3  | FERNANDA  |
| 4  | ROBERTO   |
| 5  | JULIANA   |

### Tabela: PEDIDOS

| ID  | VALOR | FK_CLIENTE |
|-----|-------|-------------|
| 101 | 250   | 1           |
| 102 | 500   | 2           |
| 103 | 120   | 1           |
| 104 | 900   | 4           |
| 105 | 300   | 5           |

### Atividade
- Criar uma consulta utilizando:
  - `INNER JOIN`
- Exibir:
  - Nome do cliente
  - Valor do pedido

---

## 2. Alunos e Notas

### Tabela: ALUNOS

| ID | ALUNO    |
|----|-----------|
| 1  | PEDRO     |
| 2  | LARISSA   |
| 3  | MATEUS    |
| 4  | BIANCA    |
| 5  | RAFAEL    |

### Tabela: NOTAS

| ID | NOTA | FK_ALUNO |
|----|------|-----------|
| 1  | 8.5  | 1         |
| 2  | 9.0  | 2         |
| 3  | 7.5  | 3         |
| 4  | 6.0  | 4         |
| 5  | 10.0 | 1         |

### Atividade
- Criar uma consulta utilizando:
  - `INNER JOIN`
- Exibir:
  - Nome do aluno
  - Nota

---

# LEFT JOIN

## 3. Produtos e Categorias

### Tabela: CATEGORIAS

| ID | CATEGORIA   |
|----|--------------|
| 1  | INFORMÁTICA  |
| 2  | ESCRITÓRIO   |
| 3  | GAMER        |

### Tabela: PRODUTOS

| ID | PRODUTO     | FK_CATEGORIA |
|----|--------------|---------------|
| 1  | MOUSE        | 1             |
| 2  | TECLADO      | 1             |
| 3  | CADEIRA      | 2             |
| 4  | HEADSET      | 3             |
| 5  | CABO USB     | NULL          |

### Atividade
- Criar uma consulta utilizando:
  - `LEFT JOIN`
- Exibir:
  - Produto
  - Categoria

---

## 4. Funcionários e Projetos

### Tabela: PROJETOS

| ID | PROJETO      |
|----|---------------|
| 1  | SISTEMA A     |
| 2  | SISTEMA B     |
| 3  | APLICATIVO X  |

### Tabela: FUNCIONARIOS

| ID | FUNCIONARIO | FK_PROJETO |
|----|--------------|-------------|
| 1  | MARCOS       | 1           |
| 2  | JULIANA      | NULL        |
| 3  | RICARDO      | 2           |
| 4  | AMANDA       | 3           |
| 5  | PAULA        | NULL        |

### Atividade
- Criar uma consulta utilizando:
  - `LEFT JOIN`
- Exibir:
  - Funcionário
  - Projeto

---

# RIGHT JOIN

## 5. Funcionários e Departamentos

### Tabela: DEPARTAMENTOS

| ID | DEPARTAMENTO |
|----|---------------|
| 1  | RH            |
| 2  | TI            |
| 3  | FINANCEIRO    |
| 4  | MARKETING     |

### Tabela: FUNCIONARIOS

| ID | FUNCIONARIO | FK_DEPARTAMENTO |
|----|--------------|-----------------|
| 1  | JOÃO         | 1               |
| 2  | ANA          | 2               |
| 3  | ROBERTO      | 2               |

### Atividade
- Criar uma consulta utilizando:
  - `RIGHT JOIN`
- Exibir:
  - Funcionário
  - Departamento

---

## 6. Produtos e Fornecedores

### Tabela: FORNECEDORES

| ID | FORNECEDOR |
|----|-------------|
| 1  | TECHPLUS    |
| 2  | MEGAINFO    |
| 3  | DIGITALNET  |
| 4  | ALPHATECH   |

### Tabela: PRODUTOS

| ID | PRODUTO    | FK_FORNECEDOR |
|----|-------------|----------------|
| 1  | NOTEBOOK    | 1              |
| 2  | MOUSE       | 2              |
| 3  | MONITOR     | 1              |
| 4  | IMPRESSORA  | 3              |

### Atividade
- Criar uma consulta utilizando:
  - `RIGHT JOIN`
- Exibir:
  - Produto
  - Fornecedor

---

# FULL JOIN

## 7. Alunos e Turmas

### Tabela: TURMAS

| ID | TURMA |
|----|--------|
| 1  | 1ºA    |
| 2  | 2ºB    |
| 3  | 3ºC    |
| 4  | 1ºD    |

### Tabela: ALUNOS

| ID | ALUNO    | FK_TURMA |
|----|-----------|-----------|
| 1  | LUCAS     | 1         |
| 2  | BEATRIZ   | NULL      |
| 3  | RAFAEL    | 2         |
| 4  | JULIA     | 1         |
| 5  | AMANDA    | NULL      |

### Atividade
- Criar uma consulta utilizando:
  - `FULL JOIN`
- Exibir:
  - Aluno
  - Turma

---

## 8. Professores e Disciplinas

### Tabela: PROFESSORES

| ID | PROFESSOR |
|----|------------|
| 1  | MARCELO    |
| 2  | DENISE     |
| 3  | RICARDO    |

### Tabela: DISCIPLINAS

| ID | DISCIPLINA  | FK_PROFESSOR |
|----|--------------|---------------|
| 1  | MATEMÁTICA   | 1             |
| 2  | HISTÓRIA     | NULL          |
| 3  | FÍSICA       | 3             |
| 4  | GEOGRAFIA    | NULL          |

### Atividade
- Criar uma consulta utilizando:
  - `FULL JOIN`
- Exibir:
  - Professor
  - Disciplina

---

# TABELA ASSOCIATIVA

## 9. Alunos e Disciplinas

### Tabela: ALUNOS

| ID | ALUNO    |
|----|-----------|
| 1  | PEDRO     |
| 2  | LARISSA   |
| 3  | MATEUS    |
| 4  | BIANCA    |

### Tabela: DISCIPLINAS

| ID | DISCIPLINA |
|----|-------------|
| 1  | MATEMÁTICA  |
| 2  | FÍSICA      |
| 3  | HISTÓRIA    |

### Tabela: ALUNOS_DISCIPLINAS

| FK_ALUNO | FK_DISCIPLINA |
|-----------|----------------|
| 1         | 1              |
| 1         | 2              |
| 2         | 1              |
| 3         | 3              |
| 4         | 2              |
| 4         | 3              |

### Atividade
- Criar uma consulta utilizando:
  - Tabela associativa
- Exibir:
  - Aluno
  - Disciplina

---

## 10. Filmes e Atores

### Tabela: FILMES

| ID | FILME         |
|----|----------------|
| 1  | MATRIX         |
| 2  | INTERESTELAR   |
| 3  | AVATAR         |
| 4  | GLADIADOR      |

### Tabela: ATORES

| ID | ATOR                |
|----|----------------------|
| 1  | KEANU REEVES         |
| 2  | MATTHEW MCCONAUGHEY |
| 3  | SAM WORTHINGTON      |
| 4  | RUSSELL CROWE        |

### Tabela: FILMES_ATORES

| FK_FILME | FK_ATOR |
|-----------|----------|
| 1         | 1        |
| 2         | 2        |
| 3         | 3        |
| 4         | 4        |
| 1         | 3        |

### Atividade
- Criar uma consulta utilizando:
  - Tabela associativa
- Exibir:
  - Filme
  - Ator
