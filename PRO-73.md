## [[SBIS] ECF.01.01 - Cadastro de Empresa/Unidade - Select de Tipo de estabelecimento de saúde](https://feegow.atlassian.net/browse/PRO-73)

- [PR com ajustes no código](https://github.com/feegow/sql_statements/pull/135)

- Queries para ajustes no banco da licença:
- Cria coluna TipoPessoa na tabela empresa caso não exista:
> SET @tmpSt = (SELECT IF(
  (SELECT COUNT(*)
  FROM INFORMATION_SCHEMA.COLUMNS
  WHERE table_name = 'empresa'
  AND table_schema = DATABASE()
  AND column_name = 'TipoPessoa'
  ) > 0,
  "SELECT 1",
  "ALTER TABLE empresa ADD TipoPessoa VARCHAR(2) NULL DEFAULT NULL"
  ));
  PREPARE stmt FROM @tmpSt;
  EXECUTE stmt;
  DEALLOCATE PREPARE stmt;


- Cria coluna CPF na tabela empresa caso não exista:
> SET @tmpSt = (SELECT IF(
 (SELECT COUNT(*)
  FROM INFORMATION_SCHEMA.COLUMNS
    WHERE table_name = 'empresa'
      AND table_schema = DATABASE()
      AND column_name = 'CPF'
 ) > 0,
  "SELECT 1",
  "ALTER TABLE empresa ADD CPF VARCHAR(50) NULL DEFAULT NULL"
 ));
 PREPARE stmt FROM @tmpSt;
 EXECUTE stmt;
 DEALLOCATE PREPARE stmt;