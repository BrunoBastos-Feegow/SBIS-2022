## [PRO-77 [SBIS] ECF.01.03 - Criar button para Ativar e Inativar Locais de Atendimento](https://feegow.atlassian.net/browse/PRO-77)

- [PR com ajustes no código **MERGED TO SBIS**](https://github.com/feegow/feegowclinic-v7/pull/2859)

CLINIC-######:
=====

- Cria coluna Ativo na tabela locais caso não exista:
```
SET @tmpSt = (
    SELECT IF(
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.COLUMNS 
                WHERE table_name = 'locais' 
                AND table_schema = DATABASE() 
                AND column_name = 'Ativo' 
        ) > 0,
        "SELECT '---coluna já existe-'", 
        "ALTER TABLE locais ADD Ativo INT(11) NULL DEFAULT NULL"
    )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

- Cria coluna TipoLocal na tabela locais caso não exista:
```
SET @tmpSt = (
    SELECT IF(
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.COLUMNS 
                WHERE table_name = 'locais' 
                AND table_schema = DATABASE() 
                AND column_name = 'TipoLocal' 
        ) > 0,
        "SELECT '---coluna já existe-'", 
        "ALTER TABLE locais ADD TipoLocal INT(11) NULL DEFAULT NULL"
    )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

- Cria índice na coluna Ativo:
```
SET @tmpSt = (
    SELECT IF(
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.STATISTICS 
                WHERE TABLE_NAME = 'locais' 
                AND table_schema = DATABASE() 
                AND index_name LIKE 'Ativo'
        ) > 0,
        "SELECT '---coluna já existe-'", 
        "ALTER TABLE `locais` ADD INDEX `Ativo` (`Ativo`)"
    )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

CLINICCENTRAL
=====

- registra campo "Ativo" para resource 27 (locais)
```
INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `showInList`, `fieldTypeID`, `size`)
SELECT '27', 'Ativo', 'Ativo', '1', '1', '2'
    FROM `sys_resourcesfields`
    WHERE `resourceID` = '27' AND `columnName` = 'Ativo'
    HAVING COUNT(*) = 0;
```

- Registra campo "TipoLocal" para resource 27 (locais)
```
INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `showInList`, `fieldTypeID`, `size`, `selectSql`, `selectColumnToShow`)
SELECT '27', 'Tipo', 'TipoLocal', '1', '3', '2', 'select * from cliniccentral.tipo_locais', 'Tipo'
    FROM `sys_resourcesfields`
    WHERE `resourceID` = '27' AND `columnName` = 'TipoLocal'
    HAVING COUNT(*) = 0;
```

- Corrige ordenação para que os ativos fiquem sempre acima, os desativados no meio e os excluídos no fim da listagem
```
UPDATE `sys_resources` SET `initialOrder` = 'ativo DESC, sysActive DESC, NomeLocal' WHERE `tableName` = 'locais';
UPDATE `sys_resourcesfields` SET `showInList`='0' WHERE  `resourceID` = 27 AND `columnName` = 'Ativo';
```

- Atualiza query para exibição do nome da Unidade nas listagens já que o nome fantasia passa a ser opcional
> NÃO EXECUTAR: Mudou este requisito e nome fantasia permaneceu obrigatório
```
-- UPDATE `sys_resourcesfields` SET `showInList`='0' WHERE  `resourceID` = 27 AND `columnName` = 'Ativo';
-- "select id, NomeFantasia, sysActive FROM (select id, IF(NomeFantasia, NomeFantasia, UnitName) AS NomeFantasia, sysActive from sys_financialcompanyunits UNION ALL select '0', IF(NomeFantasia, NomeFantasia, NomeEmpresa) AS NomeFantasia, sysActive from empresa order by id)t where sysActive=1"
```

- Implementa tabela tipo_locais que já existia na V7.5
  - Copiando estrutura para tabela tipo_locais
```
CREATE TABLE IF NOT EXISTS `tipo_locais` (
`id` int(11) NOT NULL AUTO_INCREMENT,
`Tipo` varchar(50) DEFAULT NULL,
`sysActive` tinyint(4) DEFAULT '1',
PRIMARY KEY (`id`)
);
```

- Copiando dados para a tabela tipo_locais
```
INSERT INTO `tipo_locais` (`id`, `Tipo`, `sysActive`)
SELECT 1, 'Consultório', 1
    FROM `tipo_locais`
    WHERE `Tipo` = 'Consultório'
    HAVING COUNT(*)=0;

INSERT INTO `tipo_locais` (`id`, `Tipo`, `sysActive`)
SELECT 2, 'Quarto', 1
    FROM `tipo_locais`
    WHERE `Tipo` = 'Quarto'
    HAVING COUNT(*)=0;

INSERT INTO `tipo_locais` (`id`, `Tipo`, `sysActive`)
SELECT 3, 'Leito', 1
    FROM `tipo_locais`
    WHERE `Tipo` = 'Leito'
    HAVING COUNT(*)=0;

INSERT INTO `tipo_locais` (`id`, `Tipo`, `sysActive`)
SELECT 4, 'Residencia', 1
    FROM `tipo_locais`
    WHERE `Tipo` = 'Residencia'
    HAVING COUNT(*)=0;

INSERT INTO `tipo_locais` (`id`, `Tipo`, `sysActive`)
SELECT 5, 'Aeronave', 1
    FROM `tipo_locais`
    WHERE `Tipo` = 'Aeronave'
    HAVING COUNT(*)=0;

INSERT INTO `tipo_locais` (`id`, `Tipo`, `sysActive`)
SELECT 6, 'Embarcação', 1
    FROM `tipo_locais`
    WHERE `Tipo` = 'Embarcação'
    HAVING COUNT(*)=0;

INSERT INTO `tipo_locais` (`id`, `Tipo`, `sysActive`)
SELECT 7, 'Via pública', 1
    FROM `tipo_locais`
    WHERE `Tipo` = 'Via pública'
    HAVING COUNT(*)=0;
```