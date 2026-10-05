# Banco de Dados - Triggers

## O que é uma Trigger?

Uma **Trigger** (gatilho) é um conjunto de comandos SQL executado **automaticamente** quando determinado evento ocorre em uma tabela do banco de dados.

Os principais eventos são:

- `INSERT` — inserção de um registro;
- `UPDATE` — alteração de um registro;
- `DELETE` — exclusão de um registro.

Diferentemente de um comando SQL comum, uma Trigger não precisa ser executada manualmente. O próprio banco de dados executa a Trigger quando o evento configurado acontece.

---

## Para que serve uma Trigger?

Triggers podem ser utilizadas para:

- Registrar alterações realizadas no banco;
- Criar históricos;
- Validar informações;
- Atualizar dados automaticamente;
- Automatizar regras do sistema;
- Realizar auditorias.

Por exemplo, podemos criar uma Trigger para registrar automaticamente toda vez que o preço de um produto for alterado.

---

## Quando uma Trigger é executada?

Uma Trigger pode ser executada **antes** ou **depois** de uma operação.

### BEFORE

Executa a Trigger **antes** da operação.

```sql id="4xdh43"
BEFORE INSERT
```

### AFTER

Executa a Trigger **depois** da operação.

```sql id="h5vifk"
AFTER INSERT
```

Podemos combinar `BEFORE` e `AFTER` com os eventos:

| Trigger | Execução |
|---|---|
| `BEFORE INSERT` | Antes de inserir |
| `AFTER INSERT` | Depois de inserir |
| `BEFORE UPDATE` | Antes de atualizar |
| `AFTER UPDATE` | Depois de atualizar |
| `BEFORE DELETE` | Antes de excluir |
| `AFTER DELETE` | Depois de excluir |

---

## Sintaxe

```sql id="mmj4ak"
CREATE TRIGGER nome_trigger
BEFORE INSERT
ON tabela
FOR EACH ROW
BEGIN

    -- comandos SQL

END;
```

---

## NEW

A palavra `NEW` representa os **novos valores** de um registro.

```sql id="wj6efl"
NEW.preco
```

Pode ser utilizada principalmente em:

```text id="z5dybb"
INSERT
UPDATE
```

---

## OLD

A palavra `OLD` representa os **valores anteriores** de um registro.

```sql id="kgyn36"
OLD.preco
```

Pode ser utilizada principalmente em:

```text id="2lqebc"
UPDATE
DELETE
```

---

## OLD e NEW no UPDATE

Considere:

```sql id="7pnbz6"
UPDATE produtos
SET preco = 150
WHERE id = 1;
```

Se anteriormente o preço era `100`:

```text id="xdbrwy"
OLD.preco = 100
NEW.preco = 150
```

---

## Exemplo

Considere a tabela:

```sql id="w6g6d7"
CREATE TABLE produtos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100),
    preco DECIMAL(10,2)
);
```

Crie uma tabela para armazenar o histórico:

```sql id="uychzj"
CREATE TABLE historico_precos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    produto_id INT,
    preco_antigo DECIMAL(10,2),
    preco_novo DECIMAL(10,2),
    data_alteracao DATETIME
);
```

Agora podemos criar uma Trigger que registra automaticamente as alterações de preço:

```sql id="d5kbx8"
CREATE TRIGGER registrar_alteracao_preco
AFTER UPDATE
ON produtos
FOR EACH ROW
BEGIN

    INSERT INTO historico_precos (
        produto_id,
        preco_antigo,
        preco_novo,
        data_alteracao
    )
    VALUES (
        NEW.id,
        OLD.preco,
        NEW.preco,
        NOW()
    );

END;
```

Quando executarmos:

```sql id="qdsdd5"
UPDATE produtos
SET preco = 200
WHERE id = 1;
```

a Trigger será executada automaticamente.

---

## Visualizando as Triggers

```sql id="yxlhwe"
SHOW TRIGGERS;
```

---

## Excluindo uma Trigger

```sql id="6d4oln"
DROP TRIGGER nome_trigger;
```

---

# Exercício

Considere um sistema de controle de estoque.

Utilize as seguintes tabelas:

### Tabela produtos

```sql id="37hlqm"
CREATE TABLE produtos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100),
    quantidade INT
);
```

### Tabela historico_estoque

```sql id="yb16sn"
CREATE TABLE historico_estoque (
    id INT PRIMARY KEY AUTO_INCREMENT,
    produto_id INT,
    quantidade_antiga INT,
    quantidade_nova INT,
    data_alteracao DATETIME
);
```

Crie uma **Trigger** chamada `registrar_alteracao_estoque` que:

1. Seja executada **após uma alteração** na tabela `produtos`;
2. Utilize `OLD` para obter a quantidade anterior;
3. Utilize `NEW` para obter a nova quantidade;
4. Registre o `id` do produto alterado;
5. Registre essas informações automaticamente na tabela `historico_estoque`;
6. Utilize `NOW()` para registrar a data e hora da alteração.

Após criar a Trigger:

1. Insira pelo menos **2 produtos**;
2. Altere a quantidade de um dos produtos utilizando `UPDATE`;
3. Altere novamente a quantidade desse produto;
4. Consulte a tabela `historico_estoque`;
5. Verifique se as duas alterações foram registradas corretamente.

```sql id="a6tq6u"
SELECT * FROM historico_estoque;
```
