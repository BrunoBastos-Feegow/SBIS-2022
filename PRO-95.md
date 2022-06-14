## [PRO-95-sbis-ecf-03-14-criar-configuracao-de-campos-a-serem-exibidos-na-busca-rapida](https://feegow.atlassian.net/browse/PRO-95)

- [PR com alterações no código **MERGED TO SBIS**](https://github.com/feegow/feegowclinic-v7/pull/2988)

CLINIC-#####
=====
- Sem alterações

CLINICCENTRAL
=====
- Cria configuração para habiliyar busca pelo nome da mãe em busca rápida
```
INSERT INTO `config_opcoes` (`ValorMarcado`, `ValorPadrao`, `Label`, `Coluna`, `Secao`, `sysDate`)
SELECT 'S', '0', 'Permitir busca rápida pelo nome da mãe', 'BuscarPeloNomeDaMae', 'Busca Rápida', NOW()
  FROM `config_opcoes`
  WHERE `Coluna` = 'BuscarPeloNomeDaMae'
  HAVING COUNT(*) = 0;
```