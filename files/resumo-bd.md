# Banco de Dados

## O que é um Banco de Dados?

Um Banco de Dados (BD) é um conjunto organizado de dados armazenados de forma estruturada, permitindo acesso, manipulação e análise eficiente.

Diferente de uma lista simples, um banco de dados é projetado para:
- evitar redundância (dados repetidos)
- garantir consistência
- permitir consultas rápidas
- suportar múltiplos usuários

Exemplo prático:
Um sistema escolar pode armazenar:
- alunos
- professores
- notas
- disciplinas

---

## Estrutura x Dados

### Estrutura (Schema)

A estrutura define como os dados são organizados.

Ela inclui:
- tabelas
- colunas (campos)
- tipos de dados
- regras (restrições)

Exemplo de estrutura:

| id | nome        | idade |
|----|-------------|------|
|    |             |      |

- id → número inteiro  
- nome → texto  
- idade → número inteiro  

A estrutura funciona como um molde.

---

### Dados

Os dados são os valores armazenados dentro da estrutura.

Exemplo:

| id | nome  | idade |
|----|------|------|
| 1  | Ana  | 15   |
| 2  | João | 16   |

Os dados representam a informação real.

---

## O que é um SGBD?

O SGBD (Sistema Gerenciador de Banco de Dados) é o software responsável por gerenciar o banco de dados.

Ele funciona como uma ponte entre o usuário e os dados.

Principais funções:
- criar e modificar estruturas
- inserir, consultar, atualizar e excluir dados
- controlar acesso de usuários
- garantir integridade dos dados

Exemplos:
- MySQL  
- PostgreSQL  
- MongoDB  

---

## Linguagem SQL

A SQL (Structured Query Language) é a linguagem usada para interagir com bancos de dados relacionais.

Com SQL é possível:
- criar estruturas
- inserir dados
- consultar informações
- atualizar registros
- excluir dados

---

## Divisão dos Comandos: DDL e DML

Os comandos SQL são divididos principalmente em:

- DDL → define a estrutura
- DML → manipula os dados

---

## DDL — Data Definition Language

A DDL define e altera a estrutura do banco.

### CREATE (criar)

    CREATE TABLE alunos (
      id INT,
      nome VARCHAR(100),
      idade INT
    );

Cria a tabela alunos.

---

### ALTER (alterar)

    ALTER TABLE alunos ADD nota FLOAT;

Adiciona uma nova coluna.

---

### DROP (remover)

    DROP TABLE alunos;

Remove a tabela e todos os dados.

---

## DML — Data Manipulation Language

A DML manipula os dados dentro das tabelas.

---

### INSERT (inserir)

    INSERT INTO alunos (id, nome, idade)
    VALUES (1, 'Ana', 15);

Insere um novo registro.

---

### SELECT (consultar)

    SELECT * FROM alunos;

Retorna todos os dados.

---

### UPDATE (atualizar)

    UPDATE alunos
    SET idade = 16
    WHERE id = 1;

Atualiza um registro existente.

---

### DELETE (remover)

    DELETE FROM alunos
    WHERE id = 1;

Remove um registro.

---

## Exemplo Completo (Passo a Passo)

### 1. Criar estrutura (DDL)

    CREATE TABLE alunos (
      id INT,
      nome VARCHAR(100),
      idade INT
    );

---

### 2. Inserir dados (DML)

    INSERT INTO alunos VALUES (1, 'João', 16);
    INSERT INTO alunos VALUES (2, 'Maria', 15);

---

### 3. Consultar dados

    SELECT * FROM alunos;

Resultado esperado:

| id | nome  | idade |
|----|-------|------|
| 1  | João  | 16   |
| 2  | Maria | 15   |

---

### 4. Atualizar dados

    UPDATE alunos
    SET idade = 17
    WHERE id = 1;

---

### 5. Remover dados

    DELETE FROM alunos
    WHERE id = 2;

---

## Tipos de Banco de Dados

### Banco Relacional

- Dados organizados em tabelas
- Usa SQL
- Estrutura fixa

Exemplos:
- MySQL  
- PostgreSQL  

---

### Banco Não Relacional (NoSQL)

- Estrutura flexível
- Dados em formato de documentos (JSON)

Exemplo:

    {
      "nome": "Ana",
      "idade": 15
    }

Exemplo de banco:
- MongoDB  

---

## Pontos Importantes

- Banco de Dados ≠ SGBD  
- SQL ≠ Banco de Dados  
- DDL → estrutura  
- DML → dados  

---

## Exercícios

### 1. Conceito
Explique o que é um banco de dados.

---

### 2. Classificação
Classifique como DDL ou DML:

- CREATE  
- INSERT  
- DROP  
- UPDATE  

---

### 3. Prática
Escreva comandos para:
- criar uma tabela chamada produtos
- inserir um produto
- consultar os dados

---

## Atividade Prática

Situação:
Criar um sistema simples de produtos.

Passos:

1. Criar tabela produtos
2. Definir colunas:
   - id
   - nome
   - preco
3. Inserir 3 produtos
4. Fazer um SELECT
5. Atualizar um preço
6. Deletar um produto
