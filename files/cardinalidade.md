# Entidades, Atributos e Cardinalidade

---

## 1. Entidade

Uma **Entidade** representa um objeto do mundo real — físico ou abstrato — sobre o qual se deseja armazenar informações.

### Classificação

| Tipo | Exemplos |
|------|----------|
| Física | CLIENTE, PRODUTO, PESSOA |
| Abstrata | VENDA, MATRÍCULA, TRANSAÇÃO |

> **Regra:** Toda entidade é mapeada para uma **tabela** no banco de dados relacional.

---

## 2. Entidade Forte e Entidade Fraca

### 2.1 — Entidade Forte

Uma **Entidade Forte** é aquela que possui **identidade própria** — ou seja, sua existência **não depende** de nenhuma outra entidade. Ela possui um atributo identificador (PK) que a distingue unicamente no banco de dados.

**Exemplo:** A entidade `CLIENTE` existe por si só — um cliente pode ser cadastrado mesmo sem ter feito nenhum pedido.

```sql
CREATE TABLE CLIENTE (
    ID   INT          PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL,
    CPF  CHAR(11)     UNIQUE NOT NULL
);
```

---

### 2.2 — Entidade Fraca

Uma **Entidade Fraca** é aquela que **não possui identidade própria** — sua existência depende obrigatoriamente de uma **Entidade Forte** (chamada de entidade proprietária ou dominante). Sem a entidade forte associada, a entidade fraca não faz sentido.

**Exemplo:** A entidade `DEPENDENTE` depende de `FUNCIONARIO` — um dependente só existe vinculado a um funcionário.

```sql
CREATE TABLE FUNCIONARIO (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE DEPENDENTE (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL,
    FK_FUNCIONARIO INT NOT NULL,
    FOREIGN KEY (FK_FUNCIONARIO) REFERENCES FUNCIONARIO(ID) ON DELETE CASCADE
);
```

> **`ON DELETE CASCADE`:** Como a entidade fraca depende da forte, ao excluir um funcionário, seus dependentes são automaticamente removidos — mantendo a consistência do banco de dados.

---

## 3. Atributo

Atributos são as **propriedades** que descrevem uma entidade. Cada atributo corresponde a uma coluna da tabela.

### Tipos de Atributo

| Tipo                   | Descrição                            | Exemplo                             | Definição (SQL melhorada)                                                 |
| ---------------------- | ------------------------------------ | ----------------------------------- | ------------------------------------------------------------------------- |
| **Identificador (PK)** | Identifica unicamente cada registro  | `ID`, `CPF`                         | `ID INT PRIMARY KEY AUTO_INCREMENT` ou `CPF CHAR(11) PRIMARY KEY`         |
| **Simples**            | Valor atômico, não divisível         | `NOME`, `PREÇO`                     | `NOME VARCHAR(100) NOT NULL`, `PRECO DECIMAL(10,2) NOT NULL`              |
| **Composto**           | Formado por sub-atributos            | `ENDEREÇO` → `RUA`, `CEP`, `NÚMERO` | `RUA VARCHAR(100), CEP CHAR(8), NUMERO INT`                               |
| **Multivalorado**      | Pode ter múltiplos valores           | `TELEFONE`                          | `TELEFONE VARCHAR(20) ARRAY` ou `["11999999999", "1133333333"]`           |
| **Derivado**           | Calculado a partir de outro atributo | `IDADE` ← `DATA_NASCIMENTO`         | Não armazenado: `IDADE = TIMESTAMPDIFF(YEAR, DATA_NASCIMENTO, CURDATE())` |

---

## 4. Cardinalidade

A cardinalidade define **quantas instâncias** de uma entidade podem se associar às instâncias de outra entidade em um relacionamento.

### Notação: 
```
[Entidade A] --- (min_a, max_a) --- <VERBO> --- (min_b, max_b) --- [Entidade B]
```

| Valor | Significado |
|-------|-------------|
| `0` | Participação **opcional** — a entidade pode não se relacionar |
| `1` | Participação **obrigatória** — a entidade deve se relacionar com exatamente um |
| `N` | Participação **múltipla** — a entidade pode se relacionar com muitos |

### Como identificar a cardinalidade

**Exemplo:**
- *Uma turma possui quantos alunos?* → muitos → `N`
- *Um aluno pertence a quantas turmas?* → uma → `1`
- **Resultado:** relacionamento `1:N` entre TURMA e ALUNO
```
[TURMA] --- (1, N) --- <POSSUI> --- (1, 1) --- [ALUNO]
```
---

## 5. Tipos de Relacionamento

### 5.1 — 1:1 (Um para Um)

Todo professor DEVE POSSUIR exatamente um armário, e todo armário DEVE PERTENCER exatamente a um professor.

**Cardinalidade interpretada:**
- `PROFESSOR (1,1)` — um professor **DEVE POSSUIR** um armário.
- `ARMÁRIO (1,1)` — um armário **DEVE PERTENCER** a um professor.

> [PROFESSOR] --- (1,1) --- \<POSSUI\> --- (1,1) --- [ARMÁRIO]

**Implementação:** A FK recebe `UNIQUE` (garante máximo 1) e `NOT NULL` (garante mínimo 1).

```sql
CREATE TABLE PROFESSOR (
    ID   INT          PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE ARMARIO (
    ID           INT         PRIMARY KEY AUTO_INCREMENT,
    NUMERO       VARCHAR(30) NOT NULL,
    FK_PROFESSOR INT         UNIQUE NOT NULL,
    FOREIGN KEY (FK_PROFESSOR) REFERENCES PROFESSOR(ID)
);
```

---

### 5.2 — 0:1 (Zero ou Um)

Um aluno PODE OU NÃO POSSUIR um cartão de biblioteca. Um cartão, se existir, DEVE PERTENCER a um aluno.

**Cardinalidade interpretada:**
- `ALUNO (0,1)` — um aluno **PODE OU NÃO POSSUIR** um cartão.
- `CARTÃO (0,1)` — um cartão **DEVE PERTENCER** a um aluno.

> [ALUNO] --- (0,1) --- \<POSSUI\> --- (0,1) --- [CARTÃO]

**Implementação:** A FK recebe `UNIQUE` (garante máximo 1) sem `NOT NULL` (permite mínimo 0).

```sql
CREATE TABLE ALUNO (
    ID   INT          PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE CARTAO_BIBLIOTECA (
    ID       INT         PRIMARY KEY AUTO_INCREMENT,
    CODIGO   VARCHAR(30) NOT NULL,
    FK_ALUNO INT         UNIQUE,
    FOREIGN KEY (FK_ALUNO) REFERENCES ALUNO(ID)
);
```

---

### 5.3 — 1:N (Um para Muitos)

Uma turma DEVE POSSUIR um ou mais alunos. Cada aluno DEVE PERTENCER a uma turma.

**Cardinalidade interpretada:**
- `TURMA (1,N)` — uma turma **DEVE POSSUIR PELO MENOS** 1 aluno.
- `ALUNO (1,1)` — um aluno **DEVE PERTENCER** a 1 turma.

> [TURMA] --- (1,N) --- \<POSSUI\> --- (1,1) --- [ALUNO]

**Implementação:** A FK fica no lado N (ALUNO) com `NOT NULL` (garante mínimo 1). A ausência de `UNIQUE` permite que múltiplos alunos referenciem a mesma turma.

```sql
CREATE TABLE TURMA (
    ID         INT         PRIMARY KEY AUTO_INCREMENT,
    NOME       VARCHAR(50) NOT NULL,
    ANO_LETIVO INT         NOT NULL
);

CREATE TABLE ALUNO (
    ID       INT          PRIMARY KEY AUTO_INCREMENT,
    NOME     VARCHAR(100) NOT NULL,
    FK_TURMA INT          NOT NULL,
    FOREIGN KEY (FK_TURMA) REFERENCES TURMA(ID)
);
```

---

### 5.4 — 0:N (Zero para Muitos)

Um projeto PODE OU NÃO POSSUIR tarefas. Uma tarefa PODE OU NÃO PERTENCER a um projeto.

**Cardinalidade interpretada:**
- `PROJETO (0,N)` — um projeto **PODE OU NÃO POSSUIR** tarefas.
- `TAREFA (0,1)` — uma tarefa **PODE OU NÃO PERTENCER** a um projeto.

> [PROJETO] --- (0,N) --- \<POSSUI\> --- (0,1) --- [TAREFA]

**Implementação:** A FK fica no lado N (TAREFA) sem `NOT NULL` (permite mínimo 0). A ausência de `UNIQUE` permite múltiplas tarefas por projeto.

```sql
CREATE TABLE PROJETO (
    ID   INT          PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE TAREFA (
    ID           INT          PRIMARY KEY AUTO_INCREMENT,
    DESCRICAO    VARCHAR(150) NOT NULL,
    FK_PROJETO   INT,
    FOREIGN KEY (FK_PROJETO) REFERENCES PROJETO(ID)
);
```

---

### 5.5 — N:N (Muitos para Muitos)

Um aluno PODE PERTENCER a eventos escolares, e um evento escolar PODE POSSUIR alunos.

**Cardinalidade interpretada:**
- `ALUNO (0,N)` — um aluno **PODE PERTENCER** a eventos escolares.
- `EVENTO_ESCOLAR (0,N)` — um evento escolar **PODE POSSUIR** alunos.

> [ALUNO] --- (0,N) --- \<PARTICIPA\> --- (0,N) --- [EVENTO_ESCOLAR]

**Implementação:** Não é possível representar N:N diretamente com FKs simples. É necessária uma **tabela associativa** (`ALUNO_EVENTO`) com chave primária composta `(FK_ALUNO, FK_EVENTO)`, que decompõe a relação em dois relacionamentos 1:N.

```sql
CREATE TABLE ALUNO (
    ID   INT          PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE EVENTO_ESCOLAR (
    ID   INT          PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL,
    DATA_EVENTO DATE
);

CREATE TABLE ALUNO_EVENTO (
    FK_ALUNO INT NOT NULL,
    FK_EVENTO INT NOT NULL,
    PRIMARY KEY (FK_ALUNO, FK_EVENTO),
    FOREIGN KEY (FK_ALUNO) REFERENCES ALUNO(ID),
    FOREIGN KEY (FK_EVENTO) REFERENCES EVENTO_ESCOLAR(ID)
);
```
---

## 6. Resumo dos Tipos de Relacionamento

| Tipo | Cardinalidade Formal | UNIQUE | NOT NULL | Tabela Associativa |
|------|---------------------|--------|----------|--------------------|
| 1:1  | (1,1) — (1,1)       | Sim    | Sim      | Não                |
| 0:1  | (0,1) — (0,1)       | Sim    | Não      | Não                |
| 1:N  | (1,1) — (1,N)       | Não    | Sim      | Não                |
| 0:N  | (0,1) — (0,N)       | Não    | Não      | Não                |
| N:N  | (0,N) — (0,N)       | Não    | Sim      | Sim                |

---

## 7. Resumo das Constraints

| Constraint | Função |
|------------|--------|
| `PRIMARY KEY` | Identifica unicamente cada registro da tabela |
| `FOREIGN KEY` | Cria o vínculo de relacionamento entre tabelas |
| `NOT NULL` | Torna o campo obrigatório (garante mínimo 1) |
| `UNIQUE` | Impede valores duplicados na coluna (garante máximo 1) |
| `DEFAULT` | Define um valor padrão quando nenhum é informado |
