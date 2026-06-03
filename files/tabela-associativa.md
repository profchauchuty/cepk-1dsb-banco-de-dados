# Tabela Associativa

## O que é uma Tabela Associativa?

Uma Tabela Associativa é uma tabela criada para representar relacionamentos do tipo **Muitos para Muitos (N:N)** entre duas tabelas.

Ela armazena apenas as chaves que ligam os registros das tabelas envolvidas.

---

## Por que ela existe?

Imagine uma escola.

Um aluno pode cursar várias disciplinas.

Uma disciplina pode possuir vários alunos.

Temos então o seguinte relacionamento:

```text
ALUNOS ↔ DISCIPLINAS
```

Como ambos os lados podem possuir vários registros relacionados, não é possível resolver o problema utilizando apenas uma chave estrangeira.

Nesse caso é necessário criar uma terceira tabela.

---

## Exemplo Prático

### Tabela: ALUNOS

| ID | ALUNO |
|----|--------|
| 1 | PEDRO |
| 2 | LARISSA |
| 3 | MATEUS |
| 4 | BIANCA |

### Tabela: DISCIPLINAS

| ID | DISCIPLINA |
|----|-------------|
| 1 | MATEMÁTICA |
| 2 | FÍSICA |
| 3 | HISTÓRIA |

### Tabela: ALUNOS_DISCIPLINAS

| FK_ALUNO | FK_DISCIPLINA |
|-----------|----------------|
| 1 | 1 |
| 1 | 2 |
| 2 | 1 |
| 3 | 3 |
| 4 | 2 |
| 4 | 3 |

---

## Como interpretar os dados?

Observe o registro:

| FK_ALUNO | FK_DISCIPLINA |
|-----------|----------------|
| 1 | 1 |

Significa:

```text
PEDRO → MATEMÁTICA
```

Observe o registro:

| FK_ALUNO | FK_DISCIPLINA |
|-----------|----------------|
| 1 | 2 |

Significa:

```text
PEDRO → FÍSICA
```

Observe o registro:

| FK_ALUNO | FK_DISCIPLINA |
|-----------|----------------|
| 4 | 3 |

Significa:

```text
BIANCA → HISTÓRIA
```

---

## Estrutura Geral

Uma tabela associativa sempre fica entre duas tabelas.

```text
ALUNOS
    ↓
ALUNOS_DISCIPLINAS
    ↑
DISCIPLINAS
```

Ela é responsável por armazenar os relacionamentos existentes.

---

## Como Consultar os Dados

Para visualizar os nomes dos alunos e das disciplinas, é necessário utilizar JOIN.

Exemplo:

```sql
SELECT
    A.ALUNO,
    D.DISCIPLINA
FROM ALUNOS A
INNER JOIN ALUNOS_DISCIPLINAS AD
    ON A.ID = AD.FK_ALUNO
INNER JOIN DISCIPLINAS D
    ON D.ID = AD.FK_DISCIPLINA;
```

Resultado esperado:

| ALUNO | DISCIPLINA |
|--------|-------------|
| PEDRO | MATEMÁTICA |
| PEDRO | FÍSICA |
| LARISSA | MATEMÁTICA |
| MATEUS | HISTÓRIA |
| BIANCA | FÍSICA |
| BIANCA | HISTÓRIA |

---

## Outro Exemplo

### Tabela: FILMES

| ID | FILME |
|----|--------|
| 1 | MATRIX |
| 2 | INTERESTELAR |
| 3 | AVATAR |
| 4 | GLADIADOR |

### Tabela: ATORES

| ID | ATOR |
|----|--------|
| 1 | KEANU REEVES |
| 2 | MATTHEW MCCONAUGHEY |
| 3 | SAM WORTHINGTON |
| 4 | RUSSELL CROWE |

### Tabela: FILMES_ATORES

| FK_FILME | FK_ATOR |
|-----------|----------|
| 1 | 1 |
| 2 | 2 |
| 3 | 3 |
| 4 | 4 |
| 1 | 3 |

---

## Quando Utilizar?

Utilize uma tabela associativa quando existir um relacionamento:

### Muitos para Muitos (N:N)

Exemplos:

- ALUNOS ↔ DISCIPLINAS
- FILMES ↔ ATORES
- CLIENTES ↔ PRODUTOS
- PROFESSORES ↔ DISCIPLINAS
- JOGADORES ↔ TIMES

---

## Regra de Ouro

| Relacionamento | Solução |
|---------------|----------|
| 1:1 | Normalmente não utiliza tabela associativa |
| 1:N | Utiliza chave estrangeira |
| N:N | Utiliza tabela associativa |

---

## Exercício

### Situação-Problema

Uma escola deseja emitir um relatório contendo os alunos e as disciplinas em que estão matriculados.

Os dados estão armazenados nas seguintes tabelas:

### Tabela: ALUNOS

| ID | ALUNO |
|----|--------|
| 1 | PEDRO |
| 2 | LARISSA |
| 3 | MATEUS |
| 4 | BIANCA |

### Tabela: DISCIPLINAS

| ID | DISCIPLINA |
|----|-------------|
| 1 | MATEMÁTICA |
| 2 | FÍSICA |
| 3 | HISTÓRIA |

### Tabela: ALUNOS_DISCIPLINAS

| FK_ALUNO | FK_DISCIPLINA |
|-----------|----------------|
| 1 | 1 |
| 1 | 2 |
| 2 | 1 |
| 3 | 3 |
| 4 | 2 |
| 4 | 3 |

### Questionamento

O diretor da escola precisa visualizar:

- Nome do aluno
- Nome da disciplina

Considerando que os dados estão distribuídos em três tabelas:

- Como obter essas informações utilizando SQL?
- Qual é a função da tabela ALUNOS_DISCIPLINAS?
- Qual tipo de relacionamento existe entre ALUNOS e DISCIPLINAS?
- Quais tabelas precisam participar da consulta?
