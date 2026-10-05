# Banco de Dados - Transações

## O que é uma Transação?

Uma **Transação** é um conjunto de operações SQL que devem ser executadas como uma única operação.

Um exemplo comum é uma **transferência bancária**:

1. Retirar dinheiro de uma conta;
2. Adicionar o mesmo valor em outra conta.

As duas operações precisam acontecer juntas.

Se ocorrer algum problema durante a transferência, as alterações podem ser desfeitas.

---

## Para que servem as Transações?

As Transações permitem:

- Executar várias operações juntas;
- Confirmar alterações;
- Desfazer alterações;
- Evitar dados inconsistentes;
- Aumentar a segurança das operações.

---

# Principais Comandos

## START TRANSACTION

Inicia uma Transação.

```sql id="xg4z1m"
START TRANSACTION;
```

---

## COMMIT

Confirma as alterações realizadas.

```sql id="kq7p2v"
COMMIT;
```

Depois do `COMMIT`, as alterações são confirmadas.

---

## ROLLBACK

Desfaz as alterações realizadas durante a Transação.

```sql id="rm8c3n"
ROLLBACK;
```

---

## SAVEPOINT

Cria um ponto dentro da Transação.

```sql id="ty5b9q"
SAVEPOINT ponto1;
```

Podemos retornar até esse ponto:

```sql id="vp2m6k"
ROLLBACK TO ponto1;
```

---

# Propriedades ACID

As Transações possuem quatro propriedades importantes conhecidas como **ACID**.

## Atomicidade

A Transação acontece completamente ou não acontece.

**Tudo ou nada.**

Em uma transferência bancária, não podemos retirar o dinheiro de uma conta sem adicionar o valor na outra.

---

## Consistência

O banco deve continuar respeitando suas regras antes e depois da Transação.

Exemplos:

- `PRIMARY KEY`;
- `FOREIGN KEY`;
- `NOT NULL`;
- `UNIQUE`;
- `CHECK`.

---

## Isolamento

Uma Transação não deve interferir incorretamente em outra Transação que esteja acontecendo ao mesmo tempo.

---

## Durabilidade

Depois de um:

```sql id="ns4x8r"
COMMIT;
```

as alterações confirmadas devem permanecer armazenadas.

---

## Resumo ACID

| Propriedade | Significado |
|---|---|
| **Atomicidade** | Tudo ou nada |
| **Consistência** | Mantém os dados válidos |
| **Isolamento** | Controla operações simultâneas |
| **Durabilidade** | Mantém alterações confirmadas |

---

# Problemas que podem ocorrer em uma Transação

Durante uma Transação podem ocorrer problemas que impedem a conclusão correta das operações.

## Erro em um comando SQL

Um dos comandos pode apresentar erro.

Exemplo:

```sql id="gk3p7w"
UPDATE conta
SET saldo = saldo + 200
WHERE id = 2;
```

Se a tabela correta for `contas`, o comando apresentará erro.

---

## Violação de uma regra do banco

Uma operação pode tentar inserir ou alterar um dado que não respeita as regras da tabela.

Exemplo:

```sql id="bw6m2q"
saldo DECIMAL(10,2) CHECK (saldo >= 0)
```

Se uma operação tentar deixar o saldo negativo, o banco poderá impedir a alteração.

---

## Saldo insuficiente

Em uma transferência bancária, o cliente pode tentar transferir um valor maior que o seu saldo.

Exemplo:

```text id="zh9r4c"
Saldo:        R$ 500
Transferência: R$ 800
```

A aplicação deve identificar o problema e cancelar a transferência.

---

## Falha no sistema ou aplicação

O programa responsável pela operação pode apresentar um erro durante a Transação.

Exemplo:

```text id="df2k8m"
Retira dinheiro de João
        ↓
Erro na aplicação
        ↓
Dinheiro ainda não foi adicionado para Maria
```

Nesse caso, a Transação não deve ser confirmada.

---

## Queda de conexão

A conexão entre a aplicação e o banco de dados pode ser interrompida durante uma operação.

Exemplo:

```text id="cx7v3p"
Aplicação → Banco de Dados

        X

Conexão perdida
```

A Transação pode ficar incompleta e precisar ser cancelada pelo SGBD.

---

## Falha no servidor

O servidor pode apresentar algum problema durante a execução.

Exemplos:

- Queda de energia;
- Reinicialização inesperada;
- Falha do sistema operacional;
- Falha do serviço do banco de dados.

As Transações ajudam o SGBD a manter o banco em um estado consistente mesmo quando ocorrem falhas.

---

## Resumo

| Problema | Exemplo |
|---|---|
| Erro SQL | Nome incorreto de tabela ou coluna |
| Violação de regra | Valor não permitido |
| Saldo insuficiente | Transferir mais dinheiro do que possui |
| Erro na aplicação | Programa interrompe a operação |
| Queda de conexão | Aplicação perde conexão com o banco |
| Falha no servidor | Queda de energia ou reinicialização |

Quando um problema é identificado antes da confirmação da Transação, podemos utilizar:

```sql id="mq5t8z"
ROLLBACK;
```

para desfazer as alterações.

---

# Exemplo — Transferência Bancária

Considere a tabela:

```sql id="pj4r6x"
CREATE TABLE contas (
    id INT PRIMARY KEY AUTO_INCREMENT,
    titular VARCHAR(100),
    saldo DECIMAL(10,2)
);
```

Vamos cadastrar duas contas:

```sql id="kn8b3m"
INSERT INTO contas (titular, saldo)
VALUES
('João', 1000.00),
('Maria', 500.00);
```

João deseja transferir **R$ 200,00 para Maria**.

---

## Fluxo da Transferência

| Etapa | Operação | João | Maria |
|---|---|---:|---:|
| Inicial | Saldos iniciais | R$ 1.000 | R$ 500 |
| 1 | `START TRANSACTION` | R$ 1.000 | R$ 500 |
| 2 | Retira R$ 200 de João | R$ 800 | R$ 500 |
| 3 | Adiciona R$ 200 para Maria | R$ 800 | R$ 700 |
| 4 | `COMMIT` | R$ 800 | R$ 700 |

---

## Código da Transferência

```sql id="hv7c2q"
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 200
WHERE id = 1;

UPDATE contas
SET saldo = saldo + 200
WHERE id = 2;

COMMIT;
```

Resultado:

| Titular | Antes | Depois |
|---|---:|---:|
| João | R$ 1.000 | R$ 800 |
| Maria | R$ 500 | R$ 700 |

---

# Exemplo com Problema

Imagine que o dinheiro foi retirado de João:

```sql id="rw3n9k"
START TRANSACTION;

UPDATE contas
SET saldo = saldo - 200
WHERE id = 1;
```

Nesse momento:

| Titular | Saldo |
|---|---:|
| João | R$ 800 |
| Maria | R$ 500 |

Antes de adicionar o dinheiro para Maria, ocorre um problema.

Para cancelar a operação:

```sql id="zb6q4m"
ROLLBACK;
```

Resultado:

| Titular | Durante a Transação | Após ROLLBACK |
|---|---:|---:|
| João | R$ 800 | R$ 1.000 |
| Maria | R$ 500 | R$ 500 |

Assim, a transferência foi cancelada e os valores voltaram ao estado anterior.

---

# COMMIT x ROLLBACK

| Comando | Função |
|---|---|
| `COMMIT` | Confirma as alterações |
| `ROLLBACK` | Desfaz as alterações |
| `ROLLBACK TO` | Retorna até um `SAVEPOINT` |

---

# Exercício

Considere a tabela:

```sql id="xq2v7p"
CREATE TABLE contas (
    id INT PRIMARY KEY AUTO_INCREMENT,
    titular VARCHAR(100),
    saldo DECIMAL(10,2)
);
```

Cadastre:

```sql id="mc9k4r"
INSERT INTO contas (titular, saldo)
VALUES
('Ana', 1500.00),
('Carlos', 800.00);
```

Situação inicial:

| Titular | Saldo |
|---|---:|
| Ana | R$ 1.500,00 |
| Carlos | R$ 800,00 |

Ana deseja transferir **R$ 300,00 para Carlos**.

## Tarefa

Realize a transferência utilizando uma Transação.

1. Inicie utilizando `START TRANSACTION`;
2. Retire R$ 300,00 da conta de Ana;
3. Adicione R$ 300,00 à conta de Carlos;
4. Consulte os saldos utilizando `SELECT`;
5. Confirme a transferência utilizando `COMMIT`;
6. Consulte novamente os saldos.

Depois, faça uma nova transferência de **R$ 200,00 de Carlos para Ana**, mas desta vez:

1. Inicie uma nova Transação;
2. Realize as alterações;
3. Consulte os saldos;
4. Execute `ROLLBACK`;
5. Consulte novamente os saldos;
6. Observe o que aconteceu com os valores.
