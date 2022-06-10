## [PRO-75-sbis-ecf-01-01-trazer-os-campos-de-responsaveis-tecnicos-e-odontologicos-para-o-cadastro-da-empresa](https://feegow.atlassian.net/browse/PRO-75)

- [PR com ajustes no código](https://github.com/feegow/feegow-api/pull/2921)

- Queries para ajustes no banco da licença:
- Cria coluna ResponsavelMedico na tabela empresa caso não exista:
> SET @tmpSt = (SELECT IF(
(SELECT COUNT(*)
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'empresa'
AND table_schema = DATABASE()
AND column_name = 'ResponsavelMedico'
) > 0,
"SELECT 1",
"ALTER TABLE empresa ADD ResponsavelMedico INT(11) NULL DEFAULT NULL"
));
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;


- Cria coluna ResponsavelOdontologico na tabela empresa caso não exista:
> SET @tmpSt = (SELECT IF(
(SELECT COUNT(*)
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'empresa'
AND table_schema = DATABASE()
AND column_name = 'ResponsavelOdontologico'
) > 0,
"SELECT 1",
"ALTER TABLE empresa ADD ResponsavelOdontologico INT(11) NULL DEFAULT NULL"
));
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;

- Cria coluna ResponsavelMedico na tabela sys_financialcompanyunits caso não exista:
> SET @tmpSt = (SELECT IF(
(SELECT COUNT(*)
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'sys_financialcompanyunits'
AND table_schema = DATABASE()
AND column_name = 'ResponsavelMedico'
) > 0,
"SELECT 1",
"ALTER TABLE sys_financialcompanyunits ADD ResponsavelMedico INT(11) NULL DEFAULT NULL"
));
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;


- Cria coluna ResponsavelOdontologico na tabela sys_financialcompanyunits caso não exista:
> SET @tmpSt = (SELECT IF(
(SELECT COUNT(*)
FROM INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'sys_financialcompanyunits'
AND table_schema = DATABASE()
AND column_name = 'ResponsavelOdontologico'
) > 0,
"SELECT 1",
"ALTER TABLE sys_financialcompanyunits ADD ResponsavelOdontologico INT(11) NULL DEFAULT NULL"
));
PREPARE stmt FROM @tmpSt;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;