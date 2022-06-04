## [[SBIS] ECF.03.17 - Cabeçalho de paciente fixo no prontuário](https://feegow.atlassian.net/browse/PRO-97)

### TODO
- [ ] Manter fixo o cabeçalho do prontuário do paciente em todas as abas do prontuário do paciente com as seguintes informações:
    - [x] Nome completo; ```já na TopBar``` 
    - [x] Número de identificação do paciente no sistema; ```já em dadosComplementares```
    - [x] Sexo; ```já em dadosComplementares```
    - [x] Data de nascimento; ```já em dadosComplementares```
    - [x] Idade (anos, meses e dias); ```já na TopBar```
    - [x] CPF; ```já em dadosComplementares```
    - [x] Alergias e intolerâncias ativas; ```botão p/ exibir modal``` 
      - [x] COLOCAR INFORMAÇÃO
    - [x] Diagnósticos ativos; ```botão p/ exibir modal``` 
      - [x] COLOCAR INFORMAÇÃO
    - [x] Fotografia do paciente, quando houver.```já em dadosComplementares```
  
> NOTA: Para alergias e intolerâncias e diagnósticos, pode-se utilizar mecanismos que permitam a visualização dos itens a partir de um link, como um tooltip ou pop up.

### ALTERAÇÕES
- alterado js em Pacientes.asp para mudar o comportamento quando clica em algum item do menu, deixando de ocultar a div "dadosComplementares"
- incluída funcção js para abrir os modais criados para "alergias e intolerâncias" e "diagnósticos"
- criadas páginas separadas para exibir alergias e diagnosticos (visualizarAlergiasEIntolerancioas.asp e visualizarDiagnosticos.asp)
- query destas páginas fazendo join com cliniccentral para trazer dados de CID sem precisar novas queries para cada resultado
- query de join com CID separa as alergias com "like '%alergia%' no campo descricao de CID"
