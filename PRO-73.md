## [[SBIS] ECF.01.01 - Cadastro de Empresa/Unidade - Select de Tipo de estabelecimento de saúde](https://feegow.atlassian.net/browse/PRO-73)

- [PR COM AJUSTES NO CÓDIGO - **MERGED TO SBIS**](https://github.com/feegow/feegowclinic-v7/pull/2855)
- [PR COM MIGRATIONS - API - **MERGED**](https://github.com/feegow/feegow-api/pull/2920)
- [PR COM MIGRATIONS - SQL_STATEMENTS - **MERGED**](https://github.com/feegow/sql_statements/pull/135)

### NOTAS:

- As PRs com as queries já foram aceitas em produção. As queries nesta documentação podem ser executadas novamente pois
  farão a verificação antes de alterar qualquer coisa no banco.

CLINIC-######:
=====

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
    )
);
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
    )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```


CLINIC-CENTRAL:
=====

- Tabela para controlar tipos de estabelecimentos e permitir distinguir entre pessoa física e jurídica
```
CREATE TABLE IF NOT EXISTS `pessoas_tipos` (
    `id`            int(11)      NOT NULL AUTO_INCREMENT,
    `nome`          VARCHAR(255) NOT NULL,
    `identificador` VARCHAR(2)   NOT NULL,
    `ativo`         INT(1)       NOT NULL,
    PRIMARY KEY (`id`)
);
```

- Inserindo dados default para tabela pessoas_tipos
```
INSERT INTO `pessoas_tipos` (nome, identificador, ativo)
SELECT 'Consultorio (CNPJ)', 'PJ', 1
	FROM `pessoas_tipos` 
	WHERE `nome` = 'Consultorio (CNPJ)' AND `identificador` = 'PJ'
	HAVING COUNT(*) = 0;
	
INSERT INTO `pessoas_tipos` (nome, identificador, ativo)
SELECT 'Consultorio (CPF)', 'PF', 1
	FROM `pessoas_tipos` 
	WHERE `nome` = 'Consultorio (CPF)' AND `identificador` = 'PF'
	HAVING COUNT(*) = 0;
	
INSERT INTO `pessoas_tipos` (nome, identificador, ativo) 
SELECT 'Clinica', 'PJ', 1
	FROM `pessoas_tipos` 
	WHERE `nome` = 'Clinica' AND `identificador` = 'PJ'
	HAVING COUNT(*) = 0;
	
INSERT INTO `pessoas_tipos` (nome, identificador, ativo)
SELECT 'Policlinica', 'PJ', 1
	FROM `pessoas_tipos` 
	WHERE `nome` = 'Policlinica' AND `identificador` = 'PJ'
	HAVING COUNT(*) = 0;
	
INSERT INTO `pessoas_tipos` (nome, identificador, ativo)
SELECT 'Unidade Hospitalar', 'PJ', 1
	FROM `pessoas_tipos` 
	WHERE `nome` = 'Unidade Hospitalar' AND `identificador` = 'PJ'
	HAVING COUNT(*) = 0;
```

- Habilita campo CPF e TipoPessoa para as tabelas 'empresa' e 'sys_financialcompanyunits' em sys_resourcesfields
```
INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `fieldTypeID`, `rowNumber`, `size`)
SELECT '13', 'CPF', 'CPF', '8', '111', '3'
	FROM `sys_resourcesfields` 
	WHERE `resourceID` = 13 AND `columnName` = 'CPF' 
	HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `fieldTypeID`, `rowNumber`, `size`)
SELECT '13', 'Tipo de Estabelecimento', 'TipoPessoa', '3', '1', '3'
	FROM `sys_resourcesfields` 
	WHERE `resourceID` = 13 AND `columnName` = 'TipoPessoa' 
	HAVING COUNT(*) = 0;
			
INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `fieldTypeID`, `rowNumber`, `size`)
SELECT '37', 'CPF', 'CPF', '8', '111', '3'
	FROM `sys_resourcesfields` 
	WHERE `resourceID` = 37 AND `columnName` = 'CPF' 
	HAVING COUNT(*) = 0;
			
INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `fieldTypeID`, `rowNumber`, `size`)
SELECT '37', 'Tipo de Estabelecimento', 'TipoPessoa', '3', '1', '3'
	FROM `sys_resourcesfields` 
	WHERE `resourceID` = 37 AND `columnName` = 'TipoPessoa' 
	HAVING COUNT(*) = 0;
```