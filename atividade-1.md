# ATIVIDADE — BANCO DE DADOS (JOGOS ESCOLARES)

## Objetivo

A Escola Estadual Presidente Kennedy está organizando os Jogos Escolares e precisa de um sistema para controle das informações de esportes, equipes e alunos participantes.

Atualmente, esses registros são feitos manualmente, o que dificulta a organização e a consulta dos dados.

Você deverá desenvolver um banco de dados relacional para organizar essas informações de forma estruturada.

---

## Estrutura do Banco de Dados

Crie as seguintes tabelas:

### esporte
(id, nome, categoria)

### equipe
(id, nome, sigla, fk_esporte)

### aluno
(id, nome, turma, fk_equipe)

---

## Requisitos da atividade

- ☐ Acessar o OneCompiler  
- ☐ Criar as tabelas conforme a estrutura definida  
- ☐ Definir chave primária (id) e chave estrangeira (fk) em todas as tabelas  
- ☐ Inserir dados de teste:
  - 3 esportes  
  - 6 equipes  
  - 30 alunos (Futsal: 4×5 = 20, Tênis de mesa: 6×2 = 12, Vôlei: 4×4 = 16)  

- ☐ Realizar consultas SQL  

---

## Consultas obrigatórias

- Listar todos os esportes cadastrados  
- Listar equipes com seus respectivos esportes  
- Listar alunos com turma e equipe  
- Listar alunos com equipe e esporte  
- Listar quantidade de alunos por equipe  

---

## Apresentação

O aluno deverá apenas apresentar:

- Script SQL completo (criação das tabelas, inserção e consultas)  
- Código executando corretamente no OneCompiler com os resultados visíveis  

---

## ATIVIDADE PLUS (DESAFIO)

Para elevar o nível do sistema, refaça a estrutura substituindo o atributo turma da tabela aluno por uma nova tabela.

### Nova estrutura

- Criar a tabela turma (id, nome)  
- Alterar a tabela aluno para conter fk_turma no lugar do atributo turma  

---

## Objetivo do desafio

- Criar relacionamento entre aluno e turma  
- Ajustar inserções de dados  
- Atualizar consultas para exibir a turma corretamente  

---

## Dicas de funções SQL que podem ser usadas

- SELECT → para consultar dados das tabelas  
- INSERT INTO → para inserir registros nas tabelas  
- CREATE TABLE → para criar as tabelas  
- PRIMARY KEY → para identificar registros únicos  
- FOREIGN KEY → para relacionar tabelas  
- JOIN → para unir tabelas e mostrar dados relacionados  
- COUNT() → para contar registros (ex: quantidade de alunos por equipe)  
- GROUP BY → para agrupar resultados (ex: por equipe ou esporte)  
- WHERE → para filtrar dados específicos  
- ORDER BY → para ordenar resultados  

---
