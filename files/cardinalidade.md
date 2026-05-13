# Modelagem de Dados Relacional: Entidades, Atributos e Cardinalidade.

---

# 1. Entidade (O Objeto)

Uma Entidade representa um objeto do mundo real (físico ou abstrato).

## Classificação
- Físicas: CLIENTE, PRODUTO, CARRO
- Abstratas: VENDA, MATRÍCULA, EMPRÉSTIMO

> Regra: Toda entidade vira uma TABELA no banco de dados.

---

# 2. Atributo

São propriedades (características) que descrevem uma entidade.

## Tipos
- Identificador (PK): ID, CPF
- Simples: NOME, PREÇO
- Composto: ENDEREÇO (RUA, CEP, NÚMERO)
- Multivalorado: TELEFONE
- Derivado: IDADE (DATA_NASCIMENTO)

---

# 3. Cardinalidade

Define quantos registros podem se relacionar.

## Regras
| Valor | Significado |
|------|-------------|
| 0 | Opcional |
| 1 | Obrigatório |
| N | Muitos |

---

# 4. Tipos de Relacionamento

## 4.1 1:1 (Um para Um)
Um registro tem apenas um correspondente.

```sql
SQL:
CREATE TABLE USUARIO (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME VARCHAR(100) NOT NULL
);

CREATE TABLE PERFIL (
    ID INT PRIMARY KEY,
    BIO TEXT,
    FK_USUARIO INT UNIQUE,
    FOREIGN KEY (FK_USUARIO) REFERENCES USUARIO(ID)
);
```

---

## 4.2 1:N (Um para Muitos)
Um registro pai pode ter vários filhos.

```sql
SQL:
CREATE TABLE DEPARTAMENTO (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    NOME_DEP VARCHAR(50)
);

CREATE TABLE FUNCIONARIO (
    ID INT PRIMARY KEY,
    NOME VARCHAR(100),
    FK_DEPARTAMENTO INT NOT NULL,
    FOREIGN KEY (FK_DEPARTAMENTO) REFERENCES DEPARTAMENTO(ID)
);
```

---

## 4.3 N:N (Muitos para Muitos)
Precisa de tabela intermediária.

```sql
SQL:
CREATE TABLE MATRICULA (
    FK_ALUNO INT,
    FK_DISCIPLINA INT,
    DATA_MATRICULA DATE,
    PRIMARY KEY (FK_ALUNO, FK_DISCIPLINA),
    FOREIGN KEY (FK_ALUNO) REFERENCES ALUNO(ID),
    FOREIGN KEY (FK_DISCIPLINA) REFERENCES DISCIPLINA(ID)
);
```

---

# 5. Como identificar cardinalidade

- Cliente faz quantos pedidos? → muitos (N)
- Pedido pertence a quantos clientes? → um (1)

Resultado: 1:N

---

# 6. Resumo de Constraints

- PRIMARY KEY: identifica registro
- FOREIGN KEY: cria relacionamento
- NOT NULL: obrigatório
- UNIQUE: único
- DEFAULT: valor padrão
