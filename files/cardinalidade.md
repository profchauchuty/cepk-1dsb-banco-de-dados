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

Uma **Entidade Forte** é aquela que possui **identidade própria**, ou seja, sua existência não depende de nenhuma outra entidade.

**Exemplo:** A entidade `CLIENTE` existe independentemente de outras entidades.

```sql
CREATE TABLE CLIENTE (
    ID   INT          PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL,
    CPF  CHAR(11)     UNIQUE NOT NULL
);
```

---

### 2.2 — Entidade Fraca

Uma **Entidade Fraca** depende obrigatoriamente de uma entidade forte para existir.

**Exemplo:** Um `DEPENDENTE` só existe vinculado a um `FUNCIONARIO`.

```sql
CREATE TABLE FUNCIONARIO (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE DEPENDENTE (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL,
    FK_FUNCIONARIO INT NOT NULL,
    FOREIGN KEY (FK_FUNCIONARIO)
        REFERENCES FUNCIONARIO(ID)
        ON DELETE CASCADE
);
```

> `ON DELETE CASCADE` remove automaticamente os dependentes ao excluir o funcionário.

---

## 3. Atributo

Atributos são as propriedades que descrevem uma entidade.

| Tipo | Descrição | Exemplo |
|------|------------|----------|
| Identificador | Identifica unicamente o registro | ID, CPF |
| Simples | Valor indivisível | NOME |
| Composto | Possui subatributos | ENDEREÇO |
| Multivalorado | Possui múltiplos valores | TELEFONE |
| Derivado | Calculado a partir de outro atributo | IDADE |

---

## 4. Cardinalidade

A cardinalidade define quantas instâncias de uma entidade podem se relacionar com outra.

### Notação

```text
[ENTIDADE A] --- (min,max) --- <VERBO> --- (min,max) --- [ENTIDADE B]
```

| Valor | Significado |
|------|--------------|
| 0 | participação opcional |
| 1 | participação obrigatória |
| N | múltiplas ocorrências |

---

### Como interpretar

A cardinalidade escrita próxima de uma entidade representa quantas ocorrências DAQUELA entidade o outro lado pode possuir.

---

### Exemplo

Perguntas:

- Uma TURMA possui quantos ALUNOS? → muitos → `N`
- Um ALUNO pertence a quantas TURMAS? → uma → `1`

Resultado:

```text
[TURMA] --- (1,1) --- <POSSUI> --- (1,N) --- [ALUNO]
```

Leitura:

- Um ALUNO pertence obrigatoriamente a uma TURMA.
- Uma TURMA possui um ou vários ALUNOS.

---

## 5. Tipos de Relacionamento

### 5.1 — 1:1 (Um para Um)

Todo professor deve possuir exatamente um armário.

```text
[PROFESSOR] --- (1,1) --- <POSSUI> --- (1,1) --- [ARMÁRIO]
```

```sql
CREATE TABLE PROFESSOR (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE ARMARIO (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NUMERO VARCHAR(30) NOT NULL,
    FK_PROFESSOR INT UNIQUE NOT NULL,
    FOREIGN KEY (FK_PROFESSOR) REFERENCES PROFESSOR(ID)
);
```

---

### 5.2 — 0:1 (Zero ou Um)

Um aluno pode ou não possuir um cartão.

```text
[ALUNO] --- (1,1) --- <POSSUI> --- (0,1) --- [CARTÃO]
```

Leitura:

- Um CARTÃO pertence obrigatoriamente a um ALUNO.
- Um ALUNO pode possuir zero ou um CARTÃO.

```sql
CREATE TABLE ALUNO (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE CARTAO_BIBLIOTECA (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    CODIGO VARCHAR(30) NOT NULL,
    FK_ALUNO INT UNIQUE,
    FOREIGN KEY (FK_ALUNO) REFERENCES ALUNO(ID)
);
```

---

### 5.3 — 1:N (Um para Muitos)

Uma turma possui vários alunos.

```text
[TURMA] --- (1,N) --- <POSSUI> --- (1,1) --- [ALUNO]
```

Leitura:

- Um ALUNO pertence obrigatoriamente a uma TURMA.
- Uma TURMA possui um ou vários ALUNOS.

```sql
CREATE TABLE TURMA (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(50) NOT NULL,
    ANO_LETIVO INT NOT NULL
);

CREATE TABLE ALUNO (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL,
    FK_TURMA INT NOT NULL,
    FOREIGN KEY (FK_TURMA) REFERENCES TURMA(ID)
);
```

---

### 5.4 — 0:N (Zero para Muitos)

Um projeto pode possuir várias tarefas.

```text
[PROJETO] --- (0,1) --- <POSSUI> --- (0,N) --- [TAREFA]
```

Leitura:

- Uma TAREFA pode ou não pertencer a um PROJETO.
- Um PROJETO pode possuir nenhuma ou várias TAREFAS.

```sql
CREATE TABLE PROJETO (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE TAREFA (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    DESCRICAO VARCHAR(150) NOT NULL,
    FK_PROJETO INT,
    FOREIGN KEY (FK_PROJETO) REFERENCES PROJETO(ID)
);
```

---

### 5.5 — N:N (Muitos para Muitos)

Um aluno pode participar de vários eventos.

```text
[ALUNO] --- (0,N) --- <PARTICIPA> --- (0,N) --- [EVENTO_ESCOLAR]
```

Leitura:

- Um ALUNO pode participar de vários EVENTOS.
- Um EVENTO pode possuir vários ALUNOS.

```sql
CREATE TABLE ALUNO (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE EVENTO_ESCOLAR (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL,
    DATA_EVENTO DATE
);

CREATE TABLE ALUNO_EVENTO (
    FK_ALUNO INT NOT NULL,
    FK_EVENTO INT NOT NULL,

    PRIMARY KEY (FK_ALUNO, FK_EVENTO),

    FOREIGN KEY (FK_ALUNO)
        REFERENCES ALUNO(ID),

    FOREIGN KEY (FK_EVENTO)
        REFERENCES EVENTO_ESCOLAR(ID)
);
```

---

## 6. Resumo dos Relacionamentos

| Tipo | Cardinalidade |
|------|----------------|
| 1:1 | (1,1) — (1,1) |
| 0:1 | (1,1) — (0,1) |
| 1:N | (1,1) — (1,N) |
| 0:N | (0,1) — (0,N) |
| N:N | (0,N) — (0,N) |

---

## 7. Constraints

| Constraint | Função |
|------------|--------|
| PRIMARY KEY | Identifica unicamente cada registro |
| FOREIGN KEY | Cria relacionamento entre tabelas |
| NOT NULL | Campo obrigatório |
| UNIQUE | Impede valores duplicados |
| DEFAULT | Define valor padrão |
