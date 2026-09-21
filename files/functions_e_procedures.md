# Banco de Dados - Functions e Procedures

## O que é uma Function?

Uma **Function** é um conjunto de comandos SQL armazenado no banco de dados que pode ser reutilizado sempre que necessário.

A principal característica de uma Function é que ela **retorna um valor**.

Ela pode receber parâmetros, realizar cálculos, executar operações e retornar um resultado.

Functions são especialmente úteis quando precisamos criar uma regra ou cálculo que será utilizado várias vezes.

---

## Para que serve uma Function?

As Functions são utilizadas para:

* Criar cálculos reutilizáveis;
* Evitar repetição de código;
* Transformar valores;
* Criar regras de negócio;
* Facilitar consultas SQL.

Por exemplo, podemos criar uma Function para calcular a média de duas notas.

---

## Sintaxe

Para criar uma Function no MySQL:

    DELIMITER $$

    CREATE FUNCTION nome_function(parametros)
    RETURNS tipo_retorno
    BEGIN
        comandos;
        RETURN valor;
    END $$

    DELIMITER ;

O `DELIMITER` permite que o MySQL interprete corretamente os vários comandos existentes dentro da Function.

---

## Exemplo simples

Vamos criar uma Function que recebe dois números e retorna a soma.

    DELIMITER $$

    CREATE FUNCTION somar(a INT, b INT)
    RETURNS INT
    BEGIN
        RETURN a + b;
    END $$

    DELIMITER ;

Depois de criada, podemos utilizar a Function em um `SELECT`:

    SELECT somar(10, 20);

Resultado:

    30

---

## Function com cálculo

Podemos utilizar Functions para realizar cálculos.

Vamos criar uma Function para calcular a média de duas notas:

    DELIMITER $$

    CREATE FUNCTION calcular_media(
        nota1 DECIMAL(5,2),
        nota2 DECIMAL(5,2)
    )
    RETURNS DECIMAL(5,2)
    BEGIN
        RETURN (nota1 + nota2) / 2;
    END $$

    DELIMITER ;

Podemos executar:

    SELECT calcular_media(8, 6);

Resultado:

    7.00

---

## Function com variável

Podemos declarar variáveis dentro de uma Function utilizando `DECLARE`.

    DELIMITER $$

    CREATE FUNCTION calcular_media(
        nota1 DECIMAL(5,2),
        nota2 DECIMAL(5,2)
    )
    RETURNS DECIMAL(5,2)
    BEGIN
        DECLARE media DECIMAL(5,2);

        SET media = (nota1 + nota2) / 2;

        RETURN media;
    END $$

    DELIMITER ;

Nesse exemplo:

* `DECLARE` cria uma variável;
* `SET` atribui um valor à variável;
* `RETURN` retorna o resultado.

---

## Function com IF

Uma Function também pode utilizar estruturas condicionais.

Vamos criar uma Function para verificar a situação de um aluno:

    DELIMITER $$

    CREATE FUNCTION verificar_aprovacao(
        media DECIMAL(5,2)
    )
    RETURNS VARCHAR(20)
    BEGIN
        IF media >= 6 THEN
            RETURN 'Aprovado';
        ELSE
            RETURN 'Reprovado';
        END IF;
    END $$

    DELIMITER ;

Podemos executar:

    SELECT verificar_aprovacao(7);

Resultado:

    Aprovado

---

## Function em uma consulta

Uma das principais vantagens de uma Function é poder utilizá-la dentro de consultas.

Considerando uma tabela `alunos`:

    SELECT
        nome,
        nota1,
        nota2,
        calcular_media(nota1, nota2) AS media
    FROM alunos;

Também podemos utilizar uma Function dentro de outra:

    SELECT
        nome,
        calcular_media(nota1, nota2) AS media,
        verificar_aprovacao(
            calcular_media(nota1, nota2)
        ) AS situacao
    FROM alunos;

---

## Excluindo uma Function

Para excluir uma Function:

    DROP FUNCTION nome_function;

Também podemos utilizar:

    DROP FUNCTION IF EXISTS nome_function;

O `IF EXISTS` evita um erro caso a Function não exista.

---

# O que é uma Procedure?

Uma **Procedure**, ou Stored Procedure, é um conjunto de comandos SQL armazenado no banco de dados.

Uma Procedure pode executar várias operações.

Diferentemente de uma Function, uma Procedure **não precisa retornar um valor através de `RETURN`**.

Uma Procedure pode:

* Consultar dados;
* Inserir dados;
* Atualizar dados;
* Excluir dados;
* Realizar cálculos;
* Utilizar condições;
* Utilizar variáveis;
* Receber parâmetros.

---

## Para que serve uma Procedure?

As Procedures são utilizadas para:

* Automatizar operações;
* Reutilizar comandos SQL;
* Centralizar regras de negócio;
* Executar várias operações em sequência;
* Facilitar operações frequentes;
* Reduzir a repetição de código.

---

## Sintaxe

A estrutura básica de uma Procedure é:

    DELIMITER $$

    CREATE PROCEDURE nome_procedure(parametros)
    BEGIN
        comandos;
    END $$

    DELIMITER ;

---

## Exemplo simples

Vamos criar uma Procedure que lista todos os alunos:

    DELIMITER $$

    CREATE PROCEDURE listar_alunos()
    BEGIN
        SELECT *
        FROM alunos;
    END $$

    DELIMITER ;

Para executar uma Procedure utilizamos `CALL`:

    CALL listar_alunos();

---

## Procedure com parâmetro

Uma Procedure pode receber informações através de parâmetros.

    DELIMITER $$

    CREATE PROCEDURE buscar_aluno(
        IN id_aluno INT
    )
    BEGIN
        SELECT *
        FROM alunos
        WHERE id = id_aluno;
    END $$

    DELIMITER ;

Para executar:

    CALL buscar_aluno(5);

Nesse exemplo, `id_aluno` recebe o valor `5`.

---

## Parâmetro IN

O parâmetro `IN` representa um valor de entrada.

Exemplo:

    CREATE PROCEDURE buscar_aluno(
        IN id_aluno INT
    )

Ao executar:

    CALL buscar_aluno(10);

O valor `10` será recebido pelo parâmetro `id_aluno`.

---

## Procedure para inserir dados

Uma Procedure também pode executar comandos `INSERT`.

    DELIMITER $$

    CREATE PROCEDURE cadastrar_aluno(
        IN nome_aluno VARCHAR(100),
        IN idade_aluno INT
    )
    BEGIN
        INSERT INTO alunos(nome, idade)
        VALUES(nome_aluno, idade_aluno);
    END $$

    DELIMITER ;

Executando:

    CALL cadastrar_aluno('Carlos', 20);

A Procedure executará o `INSERT` na tabela `alunos`.

---

## Procedure para atualizar dados

Também podemos utilizar `UPDATE`.

    DELIMITER $$

    CREATE PROCEDURE atualizar_idade(
        IN id_aluno INT,
        IN nova_idade INT
    )
    BEGIN
        UPDATE alunos
        SET idade = nova_idade
        WHERE id = id_aluno;
    END $$

    DELIMITER ;

Executando:

    CALL atualizar_idade(5, 21);

Nesse caso, o aluno de ID `5` terá sua idade alterada para `21`.

---

## Procedure para excluir dados

Também podemos utilizar `DELETE`.

    DELIMITER $$

    CREATE PROCEDURE excluir_aluno(
        IN id_aluno INT
    )
    BEGIN
        DELETE FROM alunos
        WHERE id = id_aluno;
    END $$

    DELIMITER ;

Executando:

    CALL excluir_aluno(5);

---

## Parâmetro OUT

O parâmetro `OUT` permite que uma Procedure produza um valor de saída.

Exemplo:

    DELIMITER $$

    CREATE PROCEDURE contar_alunos(
        OUT quantidade INT
    )
    BEGIN
        SELECT COUNT(*)
        INTO quantidade
        FROM alunos;
    END $$

    DELIMITER ;

Para executar:

    CALL contar_alunos(@total);

Depois podemos consultar o valor armazenado:

    SELECT @total;

O `@total` receberá a quantidade de alunos existentes na tabela.

---

## Parâmetro INOUT

O parâmetro `INOUT` pode receber um valor e também modificar esse valor.

Exemplo:

    DELIMITER $$

    CREATE PROCEDURE aumentar_valor(
        INOUT valor DECIMAL(10,2),
        IN percentual DECIMAL(5,2)
    )
    BEGIN
        SET valor = valor + (valor * percentual / 100);
    END $$

    DELIMITER ;

Podemos executar:

    SET @valor = 100;

    CALL aumentar_valor(@valor, 10);

    SELECT @valor;

Resultado:

    110.00

Nesse caso:

* `@valor` começa com `100`;
* a Procedure recebe esse valor;
* calcula o aumento de `10%`;
* modifica o valor;
* `@valor` passa a ser `110`.

---

## Procedure com IF

Procedures também podem utilizar estruturas condicionais.

    DELIMITER $$

    CREATE PROCEDURE verificar_idade(
        IN idade INT
    )
    BEGIN
        IF idade >= 18 THEN
            SELECT 'Maior de idade' AS resultado;
        ELSE
            SELECT 'Menor de idade' AS resultado;
        END IF;
    END $$

    DELIMITER ;

Executando:

    CALL verificar_idade(20);

Resultado:

    Maior de idade

---

## Procedure com CASE

Também podemos utilizar `CASE`.

    DELIMITER $$

    CREATE PROCEDURE classificar_nota(
        IN nota DECIMAL(5,2)
    )
    BEGIN
        CASE
            WHEN nota >= 9 THEN
                SELECT 'Excelente' AS resultado;

            WHEN nota >= 7 THEN
                SELECT 'Bom' AS resultado;

            WHEN nota >= 6 THEN
                SELECT 'Regular' AS resultado;

            ELSE
                SELECT 'Insuficiente' AS resultado;
        END CASE;
    END $$

    DELIMITER ;

Executando:

    CALL classificar_nota(8);

Resultado:

    Bom

---

# Function x Procedure

| Function | Procedure |
|---|---|
| Retorna um valor | Não precisa retornar um valor |
| Utiliza `RETURN` | Não utiliza `RETURN` para retornar o resultado principal |
| Pode ser utilizada em `SELECT` | É executada com `CALL` |
| Pode receber parâmetros | Pode receber parâmetros |
| Boa para cálculos e transformações | Boa para processos e operações |
| Pode ser utilizada dentro de expressões | Pode executar várias instruções SQL |

---

## Exemplo da diferença

### Function

Uma Function pode ser utilizada diretamente em uma expressão:

    SELECT calcular_media(8, 6);

Resultado:

    7.00

A Function retorna um valor.

### Procedure

Uma Procedure é executada através de `CALL`:

    CALL buscar_aluno(10);

A Procedure pode executar uma consulta, inserir dados, atualizar registros ou realizar várias operações.

---

## Quando utilizar Function?

Utilize uma Function quando o objetivo principal for:

* Calcular um valor;
* Transformar um valor;
* Criar uma regra que produza um resultado;
* Utilizar esse resultado dentro de uma consulta.

Exemplo:

    SELECT calcular_media(8, 6);

---

## Quando utilizar Procedure?

Utilize uma Procedure quando o objetivo principal for executar um processo.

Por exemplo:

    CALL cadastrar_aluno('Carlos', 20);

Nesse caso, a Procedure executa uma operação de cadastro.

---

## Excluindo uma Procedure

Para excluir uma Procedure:

    DROP PROCEDURE nome_procedure;

Também podemos utilizar:

    DROP PROCEDURE IF EXISTS nome_procedure;

---

# Resumo

## Function

Uma Function:

    Recebe parâmetros
           ↓
       Processa
           ↓
    Retorna um valor

Exemplo:

    SELECT calcular_media(8, 6);

---

## Procedure

Uma Procedure:

    Recebe parâmetros
           ↓
       Executa comandos
           ↓
    Produz um resultado ou altera dados

Exemplo:

    CALL cadastrar_aluno('Carlos', 20);

---

# Principais comandos

## Functions

    CREATE FUNCTION
    RETURN
    DROP FUNCTION

## Procedures

    CREATE PROCEDURE
    CALL
    DROP PROCEDURE

## Parâmetros

    IN
    OUT
    INOUT

---

# Exercícios

## 1. Function de dobro

Crie uma Function chamada `dobro` que receba um número inteiro e retorne o dobro desse número.

Teste utilizando:

    SELECT dobro(10);

O resultado esperado é:

    20

---

## 2. Function de situação do aluno

Crie uma Function chamada `situacao_aluno` que receba uma média e retorne:

* `Aprovado` para médias maiores ou iguais a `6`;
* `Recuperação` para médias maiores ou iguais a `4` e menores que `6`;
* `Reprovado` para médias menores que `4`.

Teste a Function utilizando `SELECT`.

---

## 3. Procedure de consulta

Crie uma Procedure chamada `buscar_alunos_por_idade` que receba uma idade mínima e liste todos os alunos que possuem idade maior ou igual ao valor informado.

Exemplo:

    CALL buscar_alunos_por_idade(18);

---

## 4. Procedure de atualização

Crie uma Procedure chamada `aumentar_nota` que receba:

* O ID do aluno;
* O valor do aumento.

A Procedure deverá utilizar `UPDATE` para aumentar a nota do aluno.

Exemplo:

    CALL aumentar_nota(5, 1);

---

## 5. Function e Procedure

Crie uma Function chamada `calcular_media` que receba duas notas e retorne a média.

Depois crie uma Procedure chamada `mostrar_situacao` que receba o ID de um aluno e apresente:

* Nome;
* Nota 1;
* Nota 2;
* Média;
* Situação.

A média deverá ser calculada utilizando a Function criada anteriormente.

Exemplo:

    CALL mostrar_situacao(5);
