# MER e DER

## O que é MER?

MER significa **Modelo Entidade-Relacionamento**.

É uma forma de planejar um banco de dados antes de criá-lo.

O MER descreve:

- Quais informações serão armazenadas
- Como essas informações se relacionam
- Quais atributos pertencem a cada entidade

Ele funciona como uma planta de uma casa.

Antes de construir o banco de dados, é necessário planejar sua estrutura.

---

## O que é DER?

DER significa **Diagrama Entidade-Relacionamento**.

É a representação gráfica do MER.

Enquanto o MER é o modelo conceitual, o DER é o desenho desse modelo.

Exemplo:

```text
ALUNO
   |
   |
MATRICULA
   |
   |
DISCIPLINA
```

O DER facilita a visualização da estrutura do banco de dados.

---

# Componentes do MER

Um MER é composto por:

- Entidades
- Atributos
- Relacionamentos

---

# Entidade

Uma entidade representa algo do mundo real que será armazenado no banco de dados.

Exemplos:

- ALUNO
- PROFESSOR
- DISCIPLINA
- PRODUTO
- CLIENTE

Uma entidade normalmente se transforma em uma tabela.

---

## Exemplo

### Entidade: ALUNO

| ID | NOME | IDADE |
|----|------|--------|
| 1 | PEDRO | 16 |
| 2 | ANA | 15 |

ALUNO é uma entidade.

---

# Atributo

Atributos são as características de uma entidade.

Exemplo:

### Entidade: ALUNO

| ATRIBUTO |
|-----------|
| ID |
| NOME |
| IDADE |

Nesse caso:

- ID é um atributo
- NOME é um atributo
- IDADE é um atributo

---

# Relacionamento

Relacionamento é a ligação entre duas entidades.

Exemplo:

```text
ALUNO ---- CURSA ---- DISCIPLINA
```

Nesse caso:

- ALUNO é uma entidade
- DISCIPLINA é uma entidade
- CURSA é o relacionamento

---

# Exemplo de MER

Imagine um sistema escolar.

Necessidades:

- Cadastrar alunos
- Cadastrar turmas
- Saber em qual turma cada aluno estuda

Temos as entidades:

### ALUNO

- ID
- NOME

### TURMA

- ID
- TURMA

Relacionamento:

```text
ALUNO ---- PERTENCE ---- TURMA
```

---

# DER do Exemplo

```text
+-------------+           +-------------+
|   ALUNO     |           |   TURMA     |
+-------------+           +-------------+
| ID          |           | ID          |
| NOME        |           | TURMA       |
+-------------+           +-------------+
       \                     /
        \                   /
         \                 /
          \   PERTENCE    /
           \             /
            \           /
             +---------+
```

---

# Cardinalidade

Cardinalidade define quantos registros podem participar de um relacionamento.

Os principais tipos são:

- 1:1
- 1:N
- N:N

---

# Relacionamento 1:1

Um registro está relacionado a apenas um outro registro.

Exemplo:

```text
PESSOA ↔ CPF
```

Uma pessoa possui apenas um CPF.

Um CPF pertence a apenas uma pessoa.

---

# Relacionamento 1:N

Um registro pode estar relacionado a vários registros.

Exemplo:

```text
TURMA ↔ ALUNO
```

Uma turma possui vários alunos.

Um aluno pertence a apenas uma turma.

---

## DER

```text
TURMA (1)
    |
    |
    |-----< ALUNO (N)
```

---

# Relacionamento N:N

Vários registros podem se relacionar com vários registros.

Exemplo:

```text
ALUNO ↔ DISCIPLINA
```

Um aluno pode cursar várias disciplinas.

Uma disciplina pode possuir vários alunos.

---

## DER

```text
ALUNO (N)
    |
    |
    |
DISCIPLINA (N)
```

---

# Como Resolver um N:N?

Utilizando uma tabela associativa.

Exemplo:

```text
ALUNO
   |
   |
ALUNOS_DISCIPLINAS
   |
   |
DISCIPLINA
```

---

# Exemplo Completo

## Entidade: ALUNO

| ATRIBUTO |
|-----------|
| ID |
| NOME |

## Entidade: DISCIPLINA

| ATRIBUTO |
|-----------|
| ID |
| DISCIPLINA |

## Relacionamento

```text
ALUNO ---- CURSA ---- DISCIPLINA
```

## DER

```text
+-------------+
|   ALUNO     |
+-------------+
| ID          |
| NOME        |
+-------------+
       |
       | CURSA
       |
+-------------+
| DISCIPLINA  |
+-------------+
| ID          |
| NOME        |
+-------------+
```

---

# Do MER para o Banco de Dados

Após criar o MER e o DER, eles são transformados em tabelas.

Exemplo:

MER:

```text
ALUNO
- ID
- NOME

TURMA
- ID
- NOME
```

Transformação:

```sql
CREATE TABLE TURMAS (
    ID INT PRIMARY KEY,
    TURMA VARCHAR(50)
);

CREATE TABLE ALUNOS (
    ID INT PRIMARY KEY,
    NOME VARCHAR(100),
    FK_TURMA INT
);
```

---

# Vantagens do MER e DER

- Facilita o planejamento do banco
- Evita erros de modelagem
- Permite visualizar relacionamentos
- Reduz redundância de dados
- Melhora a organização do projeto

---

# Resumo

| Conceito | Definição |
|-----------|-----------|
| MER | Modelo Entidade-Relacionamento |
| DER | Diagrama Entidade-Relacionamento |
| Entidade | Objeto que será armazenado |
| Atributo | Característica da entidade |
| Relacionamento | Ligação entre entidades |
| Cardinalidade | Quantidade de registros relacionados |

---

# Exercícios

## 1. Conceito

Explique a diferença entre MER e DER.

---

## 2. Identificação

Em um sistema de biblioteca existem as entidades:

- LIVRO
- AUTOR
- EDITORA

Identifique quais são as entidades.

---

## 3. Atributos

Para a entidade ALUNO, sugira três atributos.

---

## 4. Cardinalidade

Classifique os relacionamentos abaixo:

- PESSOA ↔ CPF
- TURMA ↔ ALUNO
- ALUNO ↔ DISCIPLINA

Utilize:

- 1:1
- 1:N
- N:N

---

## 5. Modelagem

Uma escola deseja armazenar:

- Professores
- Disciplinas
- Turmas

Crie um MER contendo:

- As entidades
- Pelo menos três atributos para cada entidade
- Os relacionamentos entre elas

---

## Atividade Prática

### Situação-Problema

Uma academia deseja desenvolver um sistema para controlar seus alunos e os planos contratados.

Regras:

- Um aluno possui apenas um plano.
- Um plano pode ser contratado por vários alunos.

### Atividade

1. Identifique as entidades.
2. Defina os atributos de cada entidade.
3. Identifique o relacionamento.
4. Determine a cardinalidade.
5. Desenhe o DER.
6. Transforme o modelo em tabelas relacionais.
