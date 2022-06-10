## [PRO-76-sbis-ecf-01-02-criar-configuracao-para-cnes-duplicados-e-validacao](https://feegow.atlassian.net/browse/PRO-76)

- [PR com ajustes no código](https://github.com/feegow/feegowclinic-v7/pull/2863)

CLINICCENTRAL 
=====

- Insere opções de configuração para validações de CNES
```
INSERT INTO `config_opcoes` (`ValorPadrao`, `Label`, `Coluna`, `Secao`)
SELECT '1', 'Validar CNES ao salvar', 'ValidarCNES', 'Empresa'
    FROM `config_opcoes`
    WHERE `Coluna` = 'ValidarCNES'
    HAVING COUNT(*) = 0;

INSERT INTO `config_opcoes` (`ValorPadrao`, `Label`, `Coluna`, `Secao`)
SELECT '1', 'Validar duplicidade de CNES', 'BloquearCNESDuplicado', 'Empresa'
    FROM `config_opcoes`
    WHERE `Coluna` = 'BloquearCNESDuplicado'
    HAVING COUNT(*) = 0;
```