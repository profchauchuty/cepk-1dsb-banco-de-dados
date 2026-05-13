# Banco de Dados — Entidades, Atributos e Cardinalidade

# O que é uma entidade?

Entidade é qualquer objeto do mundo real que pode ser representado dentro do banco de dados.

Normalmente, uma entidade se transforma em uma tabela.

---

## Exemplos de entidades

| Entidade | Representa |
|---|---|
| FUNCIONARIO | Funcionários da empresa |
| CLIENTE | Clientes do sistema |
| PRODUTO | Produtos cadastrados |
| ALUNO | Alunos da escola |
| DISCIPLINA | Disciplinas do curso |
| EMPRESA | Empresas cadastradas |

---

# O que é um atributo?

Atributo é uma característica da entidade.

Normalmente, um atributo se transforma em uma coluna da tabela.

---

## Exemplos de atributos

| Atributo | Representa |
|---|---|
| ID | Identificador |
| NOME | Nome da pessoa |
| CPF | Documento |
| EMAIL | E-mail |
| TELEFONE | Número telefônico |
| DATA_NASCIMENTO | Data de nascimento |

---

## Representação de atributos

```text
[FUNCIONARIO]

- ID
- NOME
- CPF
- EMAIL
```

---

# Exemplo completo

## Entidade

```text
[ALUNO]
```

## Atributos

```text
- ID
- NOME
- CPF
- DATA_NASCIMENTO
```

---

## Código SQL

```sql
CREATE TABLE ALUNO (
    ID INT PRIMARY KEY,
    NOME VARCHAR(100),
    CPF VARCHAR(14),
    DATA_NASCIMENTO DATE
);
```

---

# O que é cardinalidade?

Cardinalidade é a regra que define como duas entidades se relacionam.

Cardinalidade determina:

- quantos registros podem se relacionar
- se o relacionamento é obrigatório
- se os registros podem se repetir

---

## Exemplos

> - Um funcionário POSSUI quantos armários?
> - Um armário PERTENCE a quantos funcionários?
> - Um aluno CURSA quantas disciplinas?
> - Uma disciplina POSSUI quantos alunos?
> - Um cliente POSSUI quantos pedidos?
> - Um pedido PERTENCE a quantos clientes?

---

# Entendendo `(1:1)`, `(1:N)` e `(0:N)`

A estrutura funciona assim:

```text
(MÍNIMO:MÁXIMO)
```

| Parte | Significado |
|---|---|
| Primeiro valor | Quantidade mínima |
| Segundo valor | Quantidade máxima |

---

# Primeiro valor — mínimo

| Valor | Significado |
|---|---|
| `0` | Relacionamento opcional |
| `1` | Relacionamento obrigatório |

---

# Segundo valor — máximo

| Valor | Significado |
|---|---|
| `1` | Apenas um relacionamento |
| `N` | Vários relacionamentos |

---

# Exemplos rápidos

| Representação | Significado |
|---|---|
| `(1:1)` | Obrigatório e único |
| `(0:1)` | Opcional e único |
| `(1:N)` | Obrigatório e múltiplo |
| `(0:N)` | Opcional e múltiplo |

---

# Tipos de cardinalidade

| Cardinalidade | Significado | Exemplo |
|---|---|---|
| `1:1` | Um para um | Um funcionário POSSUI um armário |
| `1:N` | Um para muitos | Uma empresa POSSUI vários funcionários |
| `N:N` | Muitos para muitos | Um aluno CURSA várias disciplinas |
| `0:1` | Zero ou um | Um usuário POSSUI uma foto ou nenhuma |
| `0:N` | Zero ou muitos | Um cliente POSSUI vários pedidos ou nenhum |

---

# 1:1 — Um para um

| Campo | Descrição |
|---|---|
| Explicação | Um funcionário POSSUI apenas um armário. Um armário PERTENCE a apenas um funcionário. |
| Representação | `(FUNCIONARIO) (1:1) ---------- (1:1) (ARMARIO)` |

---

## Código SQL

```sql
CREATE TABLE FUNCIONARIO (
    ID INT PRIMARY KEY,
    NOME VARCHAR(100)
);

CREATE TABLE ARMARIO (
    ID INT PRIMARY KEY,
    NUMERO VARCHAR(10),
    FK_FUNCIONARIO INT UNIQUE,

    FOREIGN KEY (FK_FUNCIONARIO)
        REFERENCES FUNCIONARIO(ID)
);
```

---

# 1:N — Um para muitos

| Campo | Descrição |
|---|---|
| Explicação | Uma empresa POSSUI vários funcionários. Um funcionário PERTENCE a apenas uma empresa. |
| Representação | `(EMPRESA) (1:1) ---------- (1:N) (FUNCIONARIO)` |

---

## Código SQL

```sql
CREATE TABLE EMPRESA (
    ID INT PRIMARY KEY,
    NOME VARCHAR(100)
);

CREATE TABLE FUNCIONARIO (
    ID INT PRIMARY KEY,
    NOME VARCHAR(100),
    FK_EMPRESA INT NOT NULL,

    FOREIGN KEY (FK_EMPRESA)
        REFERENCES EMPRESA(ID)
);
```

---

# N:N — Muitos para muitos

| Campo | Descrição |
|---|---|
| Explicação | Um aluno CURSA várias disciplinas. Uma disciplina POSSUI vários alunos. |
| Representação | `(ALUNO) (1:N) ---------- (N:1) (DISCIPLINA)` |

---

## Código SQL

```sql
CREATE TABLE ALUNO (
    ID INT PRIMARY KEY,
    NOME VARCHAR(100)
);

CREATE TABLE DISCIPLINA (
    ID INT PRIMARY KEY,
    NOME VARCHAR(100)
);

CREATE TABLE MATRICULA (
    FK_ALUNO INT,
    FK_DISCIPLINA INT,

    PRIMARY KEY (FK_ALUNO, FK_DISCIPLINA),

    FOREIGN KEY (FK_ALUNO)
        REFERENCES ALUNO(ID),

    FOREIGN KEY (FK_DISCIPLINA)
        REFERENCES DISCIPLINA(ID)
);
```

---

## Importante

> Relacionamentos `N:N` precisam de uma tabela intermediária.

```text
(ALUNO) (1:N) ---------- (N:1) (MATRICULA) (1:N) ---------- (N:1) (DISCIPLINA)
```

---

# 0:1 — Zero ou um

| Campo | Descrição |
|---|---|
| Explicação | Um usuário POSSUI uma foto ou nenhuma. Uma foto PERTENCE a apenas um usuário. |
| Representação | `(USUARIO) (0:1) ---------- (1:1) (FOTO)` |

---

## Código SQL

```sql
CREATE TABLE USUARIO (
    ID INT PRIMARY KEY,
    NOME VARCHAR(100)
);

CREATE TABLE FOTO (
    ID INT PRIMARY KEY,
    ARQUIVO VARCHAR(100),
    FK_USUARIO INT UNIQUE,

    FOREIGN KEY (FK_USUARIO)
        REFERENCES USUARIO(ID)
);
```

---

# 0:N — Zero ou muitos

| Campo | Descrição |
|---|---|
| Explicação | Um cliente POSSUI vários pedidos ou nenhum. Um pedido PERTENCE a apenas um cliente. |
| Representação | `(CLIENTE) (0:1) ---------- (1:N) (PEDIDO)` |

---

## Código SQL

```sql
CREATE TABLE CLIENTE (
    ID INT PRIMARY KEY,
    NOME VARCHAR(100)
);

CREATE TABLE PEDIDO (
    ID INT PRIMARY KEY,
    VALOR FLOAT,
    FK_CLIENTE INT,

    FOREIGN KEY (FK_CLIENTE)
        REFERENCES CLIENTE(ID)
);
```

---

# Comparação geral

| Cardinalidade | Representação | Exemplo |
|---|---|---|
| `1:1` | `(1:1) ---------- (1:1)` | Funcionário → Armário |
| `1:N` | `(1:1) ---------- (1:N)` | Empresa → Funcionário |
| `N:N` | `(1:N) ---------- (N:1)` | Aluno → Disciplina |
| `0:1` | `(0:1) ---------- (1:1)` | Usuário → Foto |
| `0:N` | `(0:1) ---------- (1:N)` | Cliente → Pedido |

---

# Como identificar a cardinalidade

## Faça estas perguntas

> - Quantos registros podem se relacionar?
> - O relacionamento é obrigatório?
> - Os registros podem se repetir?
> - Ambos os lados podem possuir vários relacionamentos?

---

# Resumo

| Conceito | Definição |
|---|---|
| Entidade | Objeto do mundo real |
| Atributo | Característica da entidade |
| Cardinalidade | Forma como entidades se relacionam |
