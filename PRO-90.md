## [PRO-90-sbis-ecf-02-01-criar-campos-no-cadastro-do-profissional](https://feegow.atlassian.net/browse/PRO-90)

- [PR com ajustes no código **MERGED TO SBIS**](https://github.com/feegow/feegowclinic-v7/pull/2888)

CLINIC-######:
=====

- Cria tabela profissionais_dados_complementares
```
CREATE TABLE IF NOT EXISTS `profissionais_dados_complementares` (
`id` int(11) NOT NULL AUTO_INCREMENT,
`profissional_id` int(11) NOT NULL,

	`pais_nascimento_id` int(11) NULL DEFAULT NULL,
	`uf_nascimento_id` int(11) NULL DEFAULT NULL,
	`municipio_nascimento` VARCHAR(255) NULL DEFAULT NULL,
	`cns` VARCHAR(50) NULL DEFAULT NULL,
	
	`nome_mae` VARCHAR(255) NULL DEFAULT NULL,
	`mae_desconhecida` VARCHAR(2) NULL DEFAULT NULL,
	`nacionalidade` VARCHAR(255) NULL DEFAULT NULL,
	`data_naturalizacao` DATE NULL DEFAULT NULL,
	`passaporte` VARCHAR(255) NULL DEFAULT NULL,
	`passaporte_pais_emissor` VARCHAR(255) NULL DEFAULT NULL,
	`passaporte_data_emissao` DATE NULL DEFAULT NULL,
	`passaporte_data_validade` DATE NULL DEFAULT NULL,
	
	`rg` VARCHAR(255) NULL DEFAULT NULL,
	`rg_uf` INT(11) NULL DEFAULT NULL,
	`rg_orgao_emissor` VARCHAR(255) NULL DEFAULT NULL,
	`rg_data_emissao` DATE NULL DEFAULT NULL,
	
	PRIMARY KEY (`id`),
	INDEX `fk_profissionais_idx` (`profissional_id`),
	INDEX (`pais_nascimento_id`),
	INDEX (`uf_nascimento_id`)
	); 
```

CLINICCENTRAL
=====

- Cria tabela de gêneros
```
CREATE TABLE IF NOT EXISTS `generos` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `genero` varchar(200) DEFAULT NULL,
    `sysActive` tinyint(4) DEFAULT NULL,
    `sysUser` int(11) DEFAULT NULL,
    PRIMARY KEY (`id`)
);
```

- Insere dados padrões na tabela de gêneros
```
INSERT INTO `generos` (`id`, `genero`, `sysActive`, `sysUser`)
SELECT 1, 'Cisgênero', 1, 1
    FROM `generos`
    WHERE `genero` = 'Cisgênero'
    HAVING COUNT(*) = 0;

INSERT INTO `generos` (`id`, `genero`, `sysActive`, `sysUser`)
SELECT 2, 'Transgênero', 1, 1
    FROM `generos`
    WHERE `genero` = 'Transgênero'
    HAVING COUNT(*) = 0;

INSERT INTO `generos` (`id`, `genero`, `sysActive`, `sysUser`)
SELECT 3, 'Não Binário', 1, 1
    FROM `generos`
    WHERE `genero` = 'Não Binário'
    HAVING COUNT(*) = 0;

INSERT INTO `generos` (`id`, `genero`, `sysActive`, `sysUser`)
SELECT 99, 'Sem Iinformação', 1, 1
    FROM `generos`
    WHERE `genero` = 'Sem Iinformação'
    HAVING COUNT(*) = 0;
```