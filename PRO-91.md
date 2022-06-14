## [[SBIS] ECF.03.01 - Criar campos de informações demográficas no Dados Principais de pacientes](https://feegow.atlassian.net/browse/PRO-91)

### TODO
- [x] nome da mãe, permitindo indicação de mãe desconhecida de forma estruturada;
    - [x] colocar um check igual no campo de CPF para informar se é desconhecida ou não
    - [x] migration
    - [x] tela
    - [x] salvando

- [x] gênero (Transgênero, Cisgênero e Não-binário)
- [x] raça/cor (trocar “Negra“ por “preta”)
  - [x] lançar PR na API para alterar a cor Negra para Preta porque a tabela "corpele" está na licença...

- [x] nacionalidade (texto)

- [x] data de naturalização (para estrangeiros); (data)
  - [x] migration
  - [x] tela
  - [x] salvando

- [x] número do passaporte, país emissor, data de emissão e data de validade (para estrangeiros); (texto e data)
  - [x] migration
  - [x] tela
  - [x] salvando

- [x] número de identidade – complemento, UF, órgão e data de emissão; (texto)
  - [x] migration (obs: já existia "documento", logo os campos relacionados foram incluídos com o prefixo "documento_")
  - [x] tela
  - [x] salvando

CLINIC-#####
=====

- Adiciona as colunas necessárias na tabela pacientes 
```
-- removendo nome_mae e mae_desconhecida pois vai ficar registrado em pacientesrelativos
-- voltando nome_mae e mae_desconhecida para tabela pacientes por complicações relacionadas à configurações para exigência deste campo e outras regras

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'nome_mae'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD nome_mae VARCHAR(255) NULL DEFAULT NULL"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'mae_desconhecida'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD mae_desconhecida VARCHAR(2) NULL DEFAULT NULL"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'nacionalidade'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD nacionalidade VARCHAR(255) NULL DEFAULT NULL"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'data_naturalizacao'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD data_naturalizacao DATE NULL DEFAULT NULL"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'passaporte'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD passaporte VARCHAR(255) NULL DEFAULT NULL"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'passaporte_pais_emissor'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD passaporte_pais_emissor VARCHAR(255) NULL DEFAULT NULL"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'passaporte_data_emissao'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD passaporte_data_emissao DATE NULL DEFAULT NULL"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'passaporte_data_validade'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD passaporte_data_validade DATE NULL DEFAULT NULL"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'documento_uf'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD documento_uf INT(11) NULL DEFAULT NULL AFTER `documento`"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'documento_orgao_emissor'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD documento_orgao_emissor VARCHAR(255) NULL DEFAULT NULL AFTER `documento_uf`"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'documento_data_emissao'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD documento_data_emissao DATE NULL DEFAULT NULL AFTER `documento_orgao_emissor`"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'genero_id'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD genero_id  INT NULL DEFAULT NULL AFTER `documento_data_emissao`"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
    SELECT IF(
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.STATISTICS 
                WHERE TABLE_NAME = 'pacientes' 
                AND table_schema = DATABASE() 
                AND index_name LIKE 'genero_id'
        ) > 0,
        "SELECT '---indice já existe-'", 
        "ALTER TABLE `pacientes` ADD INDEX `genero_id` (`genero_id`)"
    )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
  SELECT IF(
    (SELECT COUNT(*)
      FROM INFORMATION_SCHEMA.COLUMNS
      WHERE table_name = 'pacientes'
      AND table_schema = DATABASE()
      AND column_name = 'uf_nascimento_id'
      ) > 0,
      "SELECT '---coluna já existe-'",
      "ALTER TABLE pacientes ADD uf_nascimento_id  INT NULL DEFAULT NULL AFTER `genero_id`"
  )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

SET @tmpSt = (
    SELECT IF(
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.STATISTICS 
                WHERE TABLE_NAME = 'pacientes' 
                AND table_schema = DATABASE() 
                AND index_name LIKE 'uf_nascimento_id'
        ) > 0,
        "SELECT '---indice já existe-'", 
        "ALTER TABLE `pacientes` ADD INDEX `uf_nascimento_id` (`uf_nascimento_id`)"
    )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

- altera cor negra para preta
```
UPDATE `corpele` SET `NomeCorPele` = 'Preta' WHERE `NomeCorPele` = 'Negra';
```

- insere opção "Sem Informação" caso não exista
```
INSERT INTO `corpele` (`NomeCorPele`, `sysActive`, `sysUser`)
SELECT 'Sem informação' as NomeCorPele, 1 as sysActive, 1 as sysUser
  FROM `corpele`
  WHERE (`NomeCorPele`='Sem informação')
  HAVING COUNT(*) = 0;
```
CLINICCENTRAL
=====

- Cria campos em sys_resourcesfields para permitir salvar as informações no save.asp
```
INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `showInList`, `fieldTypeID`)
SELECT '1', 'Nome da Mãe', 'nome_mae', 'Dados complementares', '1', '1'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'nome_mae'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`)
SELECT '1', 'Mãe desconhecida', 'mae_desconhecida', 'Dados complementares', '8'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'mae_desconhecida'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`)
SELECT '1', 'Nacionalidade', 'nacionalidade', 'Dados complementares', '1'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'nacionalidade'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`)
SELECT '1', 'Data de Naturalização', 'data_naturalizacao', 'Dados complementares', '13'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'data_naturalizacao'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`)
SELECT '1', 'Passaporte', 'passaporte', 'Dados complementares', '1'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'passaporte'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`)
SELECT '1', 'País emissor do passaporte', 'passaporte_pais_emissor', 'Dados complementares', '1'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'passaporte_pais_emissor'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`)
SELECT '1', 'Data de emissão do passaporte', 'passaporte_data_emissao', 'Dados complementares', '13'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'passaporte_data_emissao'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`)
SELECT '1', 'Data de validade do passaporte', 'passaporte_data_validade', 'Dados complementares', '13'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'passaporte_data_validade'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`, `selectSql`, `selectColumnToShow`) 	
SELECT '1', 'Estado emissor do RG', 'documento_uf', 'Dados complementares', '3', 'select * from cliniccentral.ibge_estados', 'Estado'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'documento_uf'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`)
SELECT '1', 'Órgão emissor do RG', 'documento_orgao_emissor', 'Dados complementares', '1'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'documento_orgao_emissor'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`)
SELECT '1', 'Data de emissão do RG', 'documento_data_emissao', 'Dados complementares', '13'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'documento_data_emissao'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`, `selectSql`, `selectColumnToShow`)
SELECT '1', 'Gênero', 'genero_id', 'Dados complementares', '1', 'select * from cliniccentral.generos where sysActive=1', 'genero'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'genero_id'
  HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `categoria`, `fieldTypeID`, `selectSql`, `selectColumnToShow`)
SELECT '1', 'UF de nascimento', 'uf_nascimento_id', 'Dados complementares', '1', 'SELECT * FROM cliniccentral.ibge_estados ORDER BY Estado', 'Estado'
  FROM `sys_resourcesfields`
  WHERE `resourceID` = '1' AND `columnName` = 'uf_nascimento_id'
  HAVING COUNT(*) = 0;
```