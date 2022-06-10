## [[SBIS] ECF.01.01 - Cadastro de Empresa/Unidade - Select de Tipo de estabelecimento de saúde](https://feegow.atlassian.net/browse/PRO-73)

- [PR COM AJUSTES NO CÓDIGO - **MERGED TO SBIS**](https://github.com/feegow/feegowclinic-v7/pull/2855)
- [PR COM MIGRATIONS - API - **MERGED**](https://github.com/feegow/feegow-api/pull/2920)
- [PR COM MIGRATIONS - SQL_STATEMENTS - **MERGED**](https://github.com/feegow/sql_statements/pull/135)

### NOTAS:

- As PRs com as queries já foram aceitas em produção. As queries nesta documentação podem ser executadas novamente pois
  farão a verificação antes de alterar qualquer coisa no banco.

### Queries para ajustes no banco da licença:

- Cria coluna TipoPessoa na tabela empresa caso não exista:

```
SET @tmpSt = ( 
    SELECT IF( 
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.COLUMNS 
                WHERE table_name = 'empresa' 
                AND table_schema = DATABASE() 
                AND column_name = 'TipoPessoa' 
        ) > 0,
        "SELECT '---coluna já existe-'",
        "ALTER TABLE empresa ADD TipoPessoa VARCHAR(2) NULL DEFAULT NULL"
  ));
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

- Cria coluna CPF na tabela empresa caso não exista:

```
SET @tmpSt = (
    SELECT IF(
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.COLUMNS 
                WHERE table_name = 'empresa' 
                AND table_schema = DATABASE() 
                AND column_name = 'CPF' 
        ) > 0,
        "SELECT '---coluna já existe-'", 
        "ALTER TABLE empresa ADD CPF VARCHAR(50) NULL DEFAULT NULL"
));
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```