## [PRO-87-sbis-ecf-02-02-criar-configuracao-para-validar-registro-no-cadastro-do-profissional](https://feegow.atlassian.net/browse/PRO-87)

- [PR com ajustes no código **MERGED TO SBIS**](https://github.com/feegow/feegowclinic-v7/pull/2868)

CLINIC-######:
=====
- Sem alterações

CLINICCENTRAL
=====

- Insere opções de configuração para validações de Conselho
```
INSERT INTO `config_opcoes` (`ValorPadrao`, `Label`, `Coluna`, `Secao`, `sysActive`)
SELECT '1', 'Validar Documento do Conselho ao salvar', 'ValidarDocumentoConselho', 'Profissionais', '0'
FROM config_opcoes
  WHERE `Coluna` = 'ValidarDocumentoConselho'
  HAVING COUNT(*) = 0;
UPDATE `config_opcoes` SET `sysActive` = 0 WHERE `Coluna` = 'ValidarDocumentoConselho';
```
```
INSERT INTO `config_opcoes` (`ValorPadrao`, `Label`, `Coluna`, `Secao`)
SELECT '1', 'Validar duplicidade do documento do conselho', 'BloquearDocumentoConselhoDuplicado', 'Profissionais'
  FROM config_opcoes
  WHERE `Coluna` = 'BloquearDocumentoConselhoDuplicado'
  HAVING COUNT(*) = 0;
```