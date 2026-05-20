# Banco de Dados — Copa do Mundo 2026

---

# Objetivo

Desenvolver um banco de dados relacional para armazenar informações da Copa do Mundo 2026, incluindo seleções, partidas, jogadores, estádios e eventos.

O sistema pode ser expandido com funcionalidades adicionais.

---

# Tecnologias

- MySQL  
- MariaDB  
- SQLite  

---

# Entregas

- Modelo Conceitual (com cardinalidade)
- Script DDL (estrutura)
- Script DML (dados)
- Consultas SQL

> Pesquisar: O que é o **Modelo Conceitual**?

Ferramentas de Modelagem:

- https://www.brmodeloweb.com  
- https://excalidraw.com  

---

# Tabelas Obrigatórias

## Modelagem corrigida (sem casa/visitante e com gols estruturados)

| Tabela | Campos |
|---|---|
| PAIS | id, nome, sigla, continente, ranking_fifa |
| TECNICO | id, nome, data_nasc, nacionalidade |
| ESTADIO | id, nome, cidade, capacidade, fk_pais |
| SELECAO | id, nome, fk_tecnico, fk_pais |
| FASE | id, nome_fase |
| JOGADOR | id, nome, nome_camisa, numero_camisa, posicao, data_nasc, altura, fk_selecao |
| ARBITRO | id, nome, nacionalidade, categoria |
| PARTIDA | id, data_hora, fk_fase, fk_estadio |
| PARTIDA_SELECAO | fk_partida, fk_selecao |
| PARTIDA_ARBITRO | fk_partida, fk_arbitro, funcao |
| EVENTO_PARTIDA | id, fk_partida, minuto, tipo_evento, fk_jogador, fk_selecao |
| GOL_PARTIDA | id, fk_partida, fk_jogador, fk_selecao, minuto |
| RESULTADO_PARTIDA | id, fk_partida, fk_selecao_vencedora, fk_selecao_perdedora, tipo_resultado |
| SUBSTITUICAO | id, fk_partida, fk_jogador_sai, fk_jogador_entra, minuto |

---

# Requisitos do Banco

- Chave primária em todas as tabelas  
- Chaves estrangeiras corretamente definidas  
- Relacionamentos consistentes  
- Tipos de dados adequados  

---

# Consultas SQL (mínimo)

- Jogadores por seleção  
- Partidas por estádio  
- Partidas de uma seleção específica  
- Gols por partida  
- Resultado de cada partida  
- Artilheiros (jogadores com mais gols)  
- Árbitros por partida  
- Jogadores mais experientes  
- Média de gols por partida  
- Seleções por continente  

---

# Avaliação

| Critério | Peso |
|---|---|
| Modelagem | 2,0 |
| Relacionamentos | 2,0 |
| Estrutura SQL | 2,0 |
| Consultas SQL | 1,5 |
| Organização | 1,0 |
| Criatividade | 1,5 |

---

# Entrega

- Scripts SQL (DDL e DML)  
- Diagrama do banco  
- Consultas SQL  
- Apresentação  

---

# Desafio Extra

O objetivo deste desafio é demonstrar que o banco de dados pode ser melhorado, expandido e otimizado.

Boa sorte.
