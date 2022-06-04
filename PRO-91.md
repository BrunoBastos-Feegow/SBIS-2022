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