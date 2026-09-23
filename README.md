# Atividade — Functions e Procedures (Stored Procedures)

## 1. Function de dobro

```sql
DELIMITER $$

CREATE FUNCTION dobro(numero INT) 
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN numero * 2;
END $$

DELIMITER ;

SELECT dobro(10) AS Resultado;
```

### Saída Esperada:
```text
+-----------+

| Resultado |
+-----------+

|        20 |
+-----------+
```

---

## 2. Function de situação do aluno

```sql
DELIMITER $$

CREATE FUNCTION situacao_aluno(media DECIMAL(4,2)) 
RETURNS VARCHAR(20)
DETERMINISTIC
BEGIN
    IF media >= 6.0 THEN
        RETURN 'Aprovado';
    ELSEIF media >= 4.0 THEN
        RETURN 'Recuperacao';
    ELSE
        RETURN 'Reprovado';
    END IF;
END $$

DELIMITER ;

SELECT situacao_aluno(7.5) AS Status_1, situacao_aluno(5.0) AS Status_2, situacao_aluno(3.2) AS Status_3;
```

### Saída Esperada:
```text
+----------+-------------+-----------+

| Status_1 | Status_2    | Status_3  |
+----------+-------------+-----------+

| Aprovado | Recuperacao | Reprovado |
+----------+-------------+-----------+
```

---

## 3. Procedure de consulta

```sql
DELIMITER $$

CREATE PROCEDURE buscar_alunos_por_idade(IN idade_minima INT)
BEGIN
    SELECT id, nome, idade 
    FROM alunos 
    WHERE idade >= id_minima;
END $$

DELIMITER ;

CALL buscar_alunos_por_idade(18);
```

### Saída Esperada:
```text
+----+----------------+-------+

| id | nome           | idade |
+----+----------------+-------+

|  1 | Carlos Souza   |    19 |
|  3 | Mariana Costa  |    18 |
|  4 | Lucas Almeida  |    22 |
+----+----------------+-------+
```

---

## 4. Procedure de atualização

```sql
DELIMITER $$

CREATE PROCEDURE aumentar_nota(IN aluno_id INT, IN valor_aumento DECIMAL(4,2))
BEGIN
    UPDATE alunos 
    SET nota = nota + valor_aumento 
    WHERE id = aluno_id;
END $$

DELIMITER ;

CALL aumentar_nota(5, 1.0);
```

### Saída Esperada:
```text
Query OK, 1 row affected
```

---

## 5. Function e Procedure Combinadas

```sql
DELIMITER $$

CREATE FUNCTION calcular_media(nota1 DECIMAL(4,2), nota2 DECIMAL(4,2)) 
RETURNS DECIMAL(4,2)
DETERMINISTIC
BEGIN
    RETURN (nota1 + nota2) / 2;
END $$

CREATE PROCEDURE mostrar_situacao(IN aluno_id INT)
BEGIN
    SELECT 
        nome, 
        nota1, 
        nota2, 
        calcular_media(nota1, nota2) AS media,
        situacao_aluno(calcular_media(nota1, nota2)) AS situacao
    FROM alunos
    WHERE id = aluno_id;
END $$

DELIMITER ;

CALL mostrar_situacao(5);
```

### Saída Esperada:
```text
+---------------+-------+-------+-------+----------+

| nome          | nota1 | nota2 | media | situacao |
+---------------+-------+-------+-------+----------+

| Bruno Moreira |  7.00 |  8.00 |  7.50 | Aprovado |
+---------------+-------+-------+-------+----------+
```
