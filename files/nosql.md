# Aula — Banco de Dados NoSQL

## 1. Contexto Histórico

### Década de 1960 — Primeiros bancos de dados

Os primeiros sistemas de bancos de dados utilizavam principalmente modelos **hierárquicos e em rede**. Embora eficientes para determinadas aplicações, apresentavam dificuldades para modificar estruturas e consultar informações.

### Década de 1970 — Modelo Relacional

Em 1970, **Edgar F. Codd**, pesquisador da IBM, apresentou o modelo relacional, baseado na organização dos dados em tabelas.

Esse modelo permitiu:

- Organizar informações em linhas e colunas.
- Estabelecer relacionamentos entre tabelas.
- Consultar dados utilizando linguagens declarativas, posteriormente padronizadas como SQL.
- Garantir integridade por meio de regras e restrições.

O modelo relacional tornou-se predominante em sistemas empresariais, financeiros e administrativos.

### Décadas de 1990 e 2000 — Expansão da Internet

Com o crescimento da Web, surgiram aplicações que precisavam armazenar quantidades cada vez maiores de informações.

Empresas como Google e Amazon enfrentaram novos desafios:

- Milhões de usuários simultâneos.
- Grandes volumes de dados.
- Informações com estruturas variadas.
- Distribuição dos dados entre vários servidores.
- Necessidade de alta disponibilidade.

Os bancos relacionais continuaram relevantes, mas determinados cenários exigiam outras arquiteturas de armazenamento.

### Anos 2000 — Popularização do NoSQL

O termo **NoSQL** passou a representar uma variedade de bancos de dados que utilizam modelos diferentes do relacional.

Alguns marcos importantes:

| Ano | Tecnologia | Contribuição |
|---|---|---|
| 2004 | Google Bigtable (desenvolvimento inicial) | Armazenamento distribuído em larga escala |
| 2007 | Amazon Dynamo (publicação) | Arquitetura distribuída de chave-valor |
| 2009 | MongoDB | Popularização dos bancos orientados a documentos |
| 2009 | Redis | Armazenamento chave-valor em memória |
| 2012 | Apache Cassandra | Consolidação como projeto Apache |
| 2017 | Cloud Firestore | Banco documental gerenciado pelo Firebase |

Atualmente, bancos relacionais e NoSQL coexistem. A escolha depende da estrutura dos dados, das consultas, dos requisitos de consistência e da arquitetura da aplicação.

## 2. O que é NoSQL?

**NoSQL (Not Only SQL)** é uma categoria de bancos de dados que utiliza modelos diferentes do relacional, como documentos, chave-valor e grafos.

## 3. SQL × NoSQL

| Característica | SQL | NoSQL |
|---|---|---|
| Estrutura | Tabelas | Documentos, grafos etc. |
| Esquema | Geralmente rígido | Geralmente flexível |
| Relacionamentos | JOIN e chaves estrangeiras | Referências e outras estruturas |
| Consultas | SQL | APIs e consultas específicas |
| Exemplos | MySQL, PostgreSQL | MongoDB, Firestore |

## 4. ACID × BASE

| ACID | BASE |
|---|---|
| Atomicidade | Basically Available |
| Consistência | Soft State |
| Isolamento | Eventual Consistency |
| Durabilidade | — |

**ACID:** propriedades que garantem a confiabilidade das transações.

**BASE:** modelo associado à disponibilidade e à consistência eventual em sistemas distribuídos.

**Importante:** bancos NoSQL também podem oferecer transações ACID.

## 5. Tipos de Bancos NoSQL

| Tipo | Exemplos | Aplicação |
|---|---|---|
| Documentos | MongoDB, Firestore | Aplicações Web e Mobile |
| Chave-valor | Redis | Cache e sessões |
| Colunas amplas | Cassandra, HBase | Grandes volumes de dados |
| Grafos | Neo4j | Redes sociais e relacionamentos |

## 6. Firebase Cloud Firestore

Banco NoSQL do Google, organizado em **coleções, documentos e campos**.

```text
alunos (coleção)
│
├── aluno001 (documento)
│   ├── nome: "Ana"
│   ├── turma: "3DSB"
│   ├── dataNascimento: "2009-05-15"
│   └── ativo: true
```

### Conexão com Node.js ou Python

Utilizar o **Firebase Admin SDK** para conectar a aplicação ao Firestore.

| Node.js | Python |
|---|---|
| `npm install firebase-admin` | `pip install firebase-admin` |

**Node.js**

```javascript
import { initializeApp, cert } from "firebase-admin/app";
import { getFirestore } from "firebase-admin/firestore";
import { readFileSync } from "node:fs";

const chave = JSON.parse(
  readFileSync("./serviceAccountKey.json", "utf8")
);

initializeApp({ credential: cert(chave) });

const db = getFirestore();

await db.collection("alunos").add({
  nome: "Ana",
  turma: "3DSB",
  dataNascimento: "2009-05-15",
  ativo: true
});
```

**Python**

```python
import firebase_admin
from firebase_admin import credentials, firestore

cred = credentials.Certificate("serviceAccountKey.json")
firebase_admin.initialize_app(cred)

db = firestore.client()

db.collection("alunos").add({
    "nome": "Ana",
    "turma": "3DSB",
    "dataNascimento": "2009-05-15",
    "ativo": True
})
```

## 7. Atividade Prática — CRUD com Firestore

**Objetivo:** Desenvolver uma aplicação em **Node.js ou Python** para manipular dados no Firebase Cloud Firestore.

### Estrutura da coleção `alunos`

| Campo | Tipo |
|---|---|
| nome | string |
| turma | string |
| dataNascimento | string (AAAA-MM-DD) |
| ativo | boolean |

### Requisitos

1. Criar um projeto no Firebase e habilitar o Firestore.
2. Configurar a conexão utilizando o Firebase Admin SDK.
3. Implementar as operações CRUD:
   - **CREATE:** cadastrar aluno.
   - **READ:** listar alunos.
   - **UPDATE:** atualizar aluno.
   - **DELETE:** excluir aluno.
4. Implementar consultas por turma e data de nascimento.
5. Utilizar menu no terminal ou API REST.

### Entrega

Enviar no Google Sala de Aula:

- Link do repositório GitHub.
- Capturas de tela demonstrando o funcionamento do CRUD e das consultas.

**Observação:** utilizar dados fictícios e não publicar o arquivo `serviceAccountKey.json` no GitHub.
