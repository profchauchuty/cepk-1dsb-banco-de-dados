# Aula — Banco de Dados NoSQL

## 1. Contexto Histórico

Os bancos de dados relacionais, popularizados a partir da década de 1970, organizam informações em tabelas e utilizam SQL para consultas.

Com o crescimento das aplicações Web, redes sociais e sistemas distribuídos, surgiram novas necessidades: armazenar grandes volumes de dados, trabalhar com estruturas flexíveis e distribuir informações entre servidores.

Nesse contexto, o movimento **NoSQL** ganhou força nos anos 2000.

## 2. O que é NoSQL?

**NoSQL (Not Only SQL)** representa uma categoria de bancos de dados que não depende exclusivamente do modelo relacional.

Características comuns:

- Armazenamento flexível de dados.
- Diferentes modelos de organização.
- Suporte a grandes volumes de informações.
- Possibilidade de escalabilidade horizontal.
- Aplicação em sistemas Web, Mobile e distribuídos.

**Importante:** NoSQL não significa ausência de relacionamentos, consultas estruturadas ou transações.

## 3. Banco Relacional × NoSQL

| Característica | Relacional (SQL) | Não Relacional (NoSQL) |
|---|---|---|
| Organização | Tabelas | Documentos, chave-valor, grafos etc. |
| Estrutura | Esquema geralmente definido | Esquema frequentemente flexível |
| Relacionamentos | Chaves estrangeiras e JOIN | Referências, documentos incorporados ou arestas |
| Consultas | SQL | Linguagem ou API específica |
| Escalabilidade | Vertical e horizontal | Frequentemente orientado à distribuição horizontal |
| Transações | ACID | Depende do banco |
| Exemplos | MySQL, PostgreSQL | MongoDB, Firestore, Redis, Neo4j |

### Exemplo: armazenamento de alunos

**Banco Relacional — SQL**

```sql
CREATE TABLE alunos (
    id INT PRIMARY KEY,
    nome VARCHAR(100),
    turma VARCHAR(10)
);

INSERT INTO alunos VALUES
(1, 'Ana', '3DSB');
```

**Banco NoSQL — Documento JSON**

```json
{
  "id": 1,
  "nome": "Ana",
  "turma": "3DSB"
}
```

No modelo relacional, os dados são armazenados em linhas e colunas. No modelo documental, são organizados em documentos com campos e valores.

## 4. ACID × BASE

ACID e BASE representam conceitos utilizados para discutir propriedades de transações, consistência e disponibilidade em sistemas de dados.

### ACID — Propriedades das transações

| Propriedade | Significado |
|---|---|
| **A — Atomicidade** | A transação é concluída integralmente ou desfeita. |
| **C — Consistência** | A transação preserva as regras de integridade do banco. |
| **I — Isolamento** | Transações simultâneas não devem interferir indevidamente umas nas outras. |
| **D — Durabilidade** | Dados confirmados permanecem armazenados mesmo após falhas. |

**Exemplo:** transferência bancária.

Se R$ 100 forem retirados de uma conta, mas não puderem ser adicionados à outra, a transação deve ser desfeita.

### BASE — Modelo associado a sistemas distribuídos

| Propriedade | Significado |
|---|---|
| **BA — Basically Available** | O sistema procura manter a disponibilidade, mesmo diante de determinadas falhas. |
| **S — Soft State** | O estado dos dados pode mudar devido à sincronização, mesmo sem novas operações do usuário. |
| **E — Eventual Consistency** | As réplicas tendem a convergir para um estado consistente quando deixam de ocorrer atualizações. |

**Exemplo:** uma aplicação distribuída pode apresentar temporariamente informações diferentes em servidores distintos, até que a sincronização seja concluída.

### Comparação

| ACID | BASE |
|---|---|
| Propriedades de transações | Modelo de comportamento distribuído |
| Ênfase na integridade transacional | Ênfase na disponibilidade e convergência |
| Consistência definida por regras transacionais | Pode admitir consistência eventual |
| Exemplo: transação bancária | Exemplo: sistema distribuído com réplicas |

**Atenção:** SQL não significa obrigatoriamente ACID, assim como NoSQL não significa obrigatoriamente BASE. MongoDB e Firestore, por exemplo, oferecem transações com propriedades ACID.

## 5. Tipos de Bancos de Dados NoSQL

### 5.1. Orientado a documentos

Armazena informações em documentos, geralmente com estruturas semelhantes a JSON.

**Exemplos:** MongoDB e Firebase Cloud Firestore.

```json
{
  "nome": "Carlos",
  "turma": "3DSB",
  "disciplinas": ["Banco de Dados", "Programação Mobile"]
}
```

Aplicações: sistemas Web, aplicativos móveis e cadastros.

### 5.2. Chave-valor

Cada informação é identificada por uma chave.

**Exemplo:** Redis.

```text
usuario:1:nome = "Carlos"
usuario:1:turma = "3DSB"
```

Aplicações: cache, sessões de usuários e armazenamento de dados de acesso rápido.

### 5.3. Orientado a colunas amplas

Organiza dados em famílias de colunas, permitindo estruturas distribuídas e grandes volumes de registros.

**Exemplos:** Apache Cassandra e HBase.

Aplicações: telemetria, eventos e registros em larga escala.

### 5.4. Orientado a grafos

Representa informações utilizando nós e relacionamentos.

**Exemplo:** Neo4j.

```text
(Ana) ──[AMIGA_DE]──> (Carlos)
```

Aplicações: redes sociais, recomendações e análise de relacionamentos.

### Comparação dos principais bancos

| Banco | Modelo | Característica |
|---|---|---|
| MongoDB | Documentos | Documentos BSON e consultas flexíveis |
| Firestore | Documentos | Integração com Firebase e sincronização em tempo real |
| Redis | Chave-valor | Operações rápidas e estruturas em memória |
| Cassandra | Colunas amplas | Distribuição e alta disponibilidade |
| Neo4j | Grafos | Consultas sobre relacionamentos |

## 6. Firebase Cloud Firestore

O **Cloud Firestore** é um banco de dados NoSQL orientado a documentos, oferecido pelo Google por meio da plataforma Firebase.

### Estrutura

O Firestore organiza dados em:

- **Coleções:** agrupamentos de documentos.
- **Documentos:** registros identificados por um ID.
- **Campos:** propriedades armazenadas em cada documento.

Exemplo:

```text
alunos (coleção)
│
├── aluno001 (documento)
│   ├── nome: "Ana"
│   ├── turma: "3DSB"
│   └── idade: 17
│
└── aluno002 (documento)
    ├── nome: "Carlos"
    ├── turma: "3DSC"
    └── idade: 18
```

Diferentemente de uma tabela relacional, documentos de uma mesma coleção podem possuir campos diferentes.

## 7. Atividade Prática — Firestore

**Objetivo:** Criar e manipular uma coleção de alunos utilizando o Firebase Cloud Firestore.

### Etapa 1 — Criar o projeto

1. Acessar https://console.firebase.google.com.
2. Criar um projeto chamado `av1-nosql`.
3. Acessar **Firestore Database**.
4. Criar o banco de dados.
5. Selecionar uma região e configurar o modo de segurança.

### Etapa 2 — Criar a coleção

Criar uma coleção chamada `alunos`.

Cadastrar pelo menos **5 documentos**, utilizando os seguintes campos:

| Campo | Tipo | Exemplo |
|---|---|---|
| nome | string | Ana Souza |
| turma | string | 3DSB |
| idade | number | 17 |
| ativo | boolean | true |

### Etapa 3 — Manipular os documentos

Realizar as seguintes operações diretamente pelo console do Firebase:

1. **CREATE:** cadastrar cinco alunos.
2. **READ:** consultar os documentos cadastrados.
3. **UPDATE:** alterar a turma de um aluno.
4. **DELETE:** excluir um aluno.

### Etapa 4 — Consultas

Utilizar os filtros do console para:

1. Localizar alunos da turma `3DSB`.
2. Localizar alunos com idade maior ou igual a 18 anos.
3. Localizar alunos com `ativo` igual a `true`.

### Entrega — Google Sala de Aula

Enviar um documento contendo:

- Captura de tela da coleção com os alunos cadastrados.
- Captura de tela de uma atualização realizada.
- Capturas de tela dos três filtros aplicados.

**Observação:** utilizar regras de segurança adequadas, sem liberar acesso público irrestrito ao banco. Não cadastrar dados pessoais reais sem autorização.
