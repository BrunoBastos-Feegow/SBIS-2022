## [PRO-75-sbis-ecf-01-01-trazer-os-campos-de-responsaveis-tecnicos-e-odontologicos-para-o-cadastro-da-empresa](https://feegow.atlassian.net/browse/PRO-75)

- [PR com ajustes no código - **MERGED TO SBIS**](https://github.com/feegow/feegowclinic-v7/pull/2856)

CLINIC-#####
=====

- Cria coluna ResponsavelMedico na tabela empresa caso não exista:

```
SET @tmpSt = (
    SELECT IF(
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.COLUMNS 
                WHERE table_name = 'empresa' 
                AND table_schema = DATABASE() 
                AND column_name = 'ResponsavelMedico' 
        ) > 0, 
        "SELECT '---coluna já existe-'", 
        "ALTER TABLE empresa ADD ResponsavelMedico INT(11) NULL DEFAULT NULL"
    )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

- Cria coluna ResponsavelOdontologico na tabela empresa caso não exista:

```
SET @tmpSt = (
    SELECT IF(
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.COLUMNS 
            WHERE table_name = 'empresa' 
            AND table_schema = DATABASE() 
            AND column_name = 'ResponsavelOdontologico' 
        ) > 0,
        "SELECT '---coluna já existe-'", 
        "ALTER TABLE empresa ADD ResponsavelOdontologico INT(11) NULL DEFAULT NULL"
    )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

- Cria coluna ResponsavelMedico na tabela sys_financialcompanyunits caso não exista:

```
SET @tmpSt = (
    SELECT IF(
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.COLUMNS 
                WHERE table_name = 'sys_financialcompanyunits' 
                AND table_schema = DATABASE() 
                AND column_name = 'ResponsavelMedico' 
        ) > 0, 
        "SELECT '---coluna já existe-'", 
        "ALTER TABLE sys_financialcompanyunits ADD ResponsavelMedico INT(11) NULL DEFAULT NULL"
    ));
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

- Cria coluna ResponsavelOdontologico na tabela sys_financialcompanyunits caso não exista:

```
SET @tmpSt = (
    SELECT IF(
        (SELECT COUNT(*) 
            FROM INFORMATION_SCHEMA.COLUMNS 
                WHERE table_name = 'sys_financialcompanyunits' 
                AND table_schema = DATABASE() 
                AND column_name = 'ResponsavelOdontologico'
        ) > 0,
        "SELECT '---coluna já existe-'", 
        "ALTER TABLE sys_financialcompanyunits ADD ResponsavelOdontologico INT(11) NULL DEFAULT NULL"
    )
);
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```

CLINICCENTRAL
=====

- habilita campo Responsável técnico e responsável odontológico e TipoPessoa para as tabelas 'empresa' e '
  sys_financialcompanyunits' em sys_resourcesfields

```
INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `fieldTypeID`, `rowNumber`, `selectSQL`, `selectColumnToShow`, `size`)
SELECT '13', 'Responsável Técnico', 'ResponsavelMedico', '3', '1', 'select * from profissionais', 'NomeProfissional', '4'
    FROM `sys_resourcesfields`
    WHERE `resourceID` = 13 AND `columnName` = 'ResponsavelMedico'
    HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `fieldTypeID`, `rowNumber`, `selectSQL`, `selectColumnToShow`, `size`)
SELECT '13', 'Responsável Odontológico', 'ResponsavelOdontologico', '3', '1', 'select * from profissionais', 'NomeProfissional', '4'
    FROM `sys_resourcesfields`
    WHERE `resourceID` = 13 AND `columnName` = 'ResponsavelOdontologico'
    HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `fieldTypeID`, `rowNumber`, `selectSQL`, `selectColumnToShow`, `size`)
SELECT '37', 'Responsável Técnico', 'ResponsavelMedico', '3', '1', 'select * from profissionais', 'NomeProfissional', '4'  
    FROM `sys_resourcesfields`
    WHERE `resourceID` = 37 AND `columnName` = 'ResponsavelMedico'
    HAVING COUNT(*) = 0;

INSERT INTO `sys_resourcesfields` (`resourceID`, `label`, `columnName`, `fieldTypeID`, `rowNumber`, `selectSQL`, `selectColumnToShow`, `size`)
SELECT '37', 'Responsável Odontológico', 'ResponsavelOdontologico', '3', '1', 'select * from profissionais', 'NomeProfissional', '4'  
    FROM `sys_resourcesfields`
    WHERE `resourceID` = 37 AND `columnName` = 'ResponsavelOdontologico'
    HAVING COUNT(*) = 0;
```