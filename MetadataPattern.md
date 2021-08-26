# Objetos e Campos customizados
## Nomenclatura
### Regras Gerais
* Nome de campos, comentários, regras, etc - em inglês
* Evite utilizar "_" em nome de campos e objetos
* Evite utilizar o padrão sugerido pela Salesforce para nomes API

Maus Exemplos:
* Label: Tipo de Sugestão
* ApiName: Tipo_de_sugest_o__c

* Label: Configuração Padrão
* ApiName: Configura__o_Padr_o__c

Bons Exemplos:
* Label: Tipo de Sugestão
* ApiName: SuggestionType__c

* Label: Configuração Padrão
* ApiName: DefaultSetting__c


### Campos
Nome em português caso a organização esteja em português, caso contrario inglês.

* API Name:
    * Utilizar as regras gerais
    * Primeiro carácter em maiúsculo, e os caracteres subsequentes em minuscuso, se possuir composição de palavras, o primeiro carácter da palavra subsequente em maiúsculo e o restante minusculo.

Maus Exemplos:
* Label: Tipo de Carga
* ApiName: tipo_de_Carga__c

* Label: Configuração Original
* ApiName: configura__o_Original__c

Bons Exemplos:
* Label: Tipo de Sugestão
* ApiName: SuggestionType__c

* Label: Configuração Padrão
* ApiName: DefaultSetting__c

### Não utilizar prefixos ou sufixos
Maus Exemplos:
* Label: Tipo de Sugestão
* ApiName: SuggestionType**Txt**__c

* Label: Configuração Padrão
* ApiName: DefaultSetting**Lkp**__c

* Label: Configuração Padrão
* ApiName: **Lkp**DefaultSetting__c

Bons Exemplos:
* Label: Tipo de Sugestão
* ApiName: SuggestionType__c

* Label: Configuração Padrão
* ApiName: DefaultSetting__c

#### Observações
* Procurar não gerar redundância no nome do atributo
   * Ex: Objeto Address, não utilizar "AddressType", mas sim "Type".
* Não truncar nome de campo, o nome tem que refletir o objetivo do campo.
* Utilizar a descrição do campo para expressar o propósito do campo.

#### Outras definições
* Child Relationship Name: NomeDoObjeto_NomeDoCampoRelacionamento.
* Related List Label: NomeDoObjeto (NomeDoCampoDoRelacionamento)
* Para relacionamentos utilizar sempre o nome do objeto que está sendo referenciado.


# Tipo de Registro
## Regras gerais
* Utilizar o formato [NomeDoObjeto] - NomeDoTipoDeRegistro para o Name do RecordType
   * Ex: [Case] Shipping Company
   * Em integrações utilizando REST API padrão da SF, o campo Name do RecordType pode ser utilizado como ID Externo. Mas ele precisa ser unico em toda a organização.
* Utilizar o formato "**RT**NomeDoObjetoNomeDoTipoDeRegistro".
   * Ex: RTAccountShippingCompany


# Layouts
#### Regras gerais Layout de Página
* Utilizar o formato [NomeDoObjeto] - NomeDoTipoDeRegistro para o Name 
   * Ex: [Case] Shipping Company
   * Caso só existe um layout manter o padrão "[Nome do Objeto] Default Layout"
   
#### Regras gerais Layout Compacto
* Utilizar o formato [NomeDoObjeto] - NomeDoTipoDeRegistro para o Name 
   * Ex: [Case] Shipping Company
   * Caso só exista um layout manter o padrão "[Nome do Objeto] Default Layout"
* Utilizar o formato **CL** + NomeDoObjeto + Name + "Layout" para o nome API.
   * Ex: CLAccountShippingCompanyLayout
   * Caso só exista um layout manter o padrão **CL**NomeDoObjeto + "DefaultLayout"


#### Regras gerais Página Lightning
* Utilizar o formato [NomeDoObjeto] - NomeDoLayout ou NomeDoTipoDeRegistro para o Name 
   * Ex: [Case] Shipping Company
   * Caso só existe um layout manter o padrão "[Nome do Objeto] Default Layout"
* Utilizar o formato **LP** + NomeDoObjeto + Name + "Layout" para o nome API.
   * Ex: LPAccountShippingCompanyLayout
   * Caso só exista um layout manter o padrão **LP**NomeDoObjeto + "DefaultLayout"


# Process Builder / Flow
## Regras gerais
* Aplicar as [boas práticas][boaspraticas] ao criar um fluxo automatizado

## Nomenclatura
### Nome do Fluxo
* Utilizar nomes que expressem o propósito do fluxo de forma que qualquer pessoa fora do projeto consiga entender, mesmo sem o contexto geral do mesmo.
* Utilizar o campo descrição sempre que possível para contextualizao

### API Name
Para facilitar o processo de deployment, (seleção dos artefatos) deverá ser utilizado um prefixo para determinar o API Name conforme exemplo abaixo.

|                       |       API Name                         |Exemplo             |Observação |
| -------------------   | -------------------------------------- |--------------------|-----------|
|  Fluxos               |  **flow** + NomeDoObjeto + NomeDoFluxo |flowCaseAfterInsert |Nome do objeto só é necessário caso o contexto seja relacionado a um objeto|
|  Process Builder      |  **PB** + NomeDoObjeto + NomeDoFluxo   |PBContractAutomation|           |

## Flow
## Nomenclatura de componentes

* Deixar o fluxo sempre o mais simples possível, facilitando o máximo a leitura e interpretação de quem for dar manutenção dos mesmo, segue sugestões de padrões a serem utilizadas. 

| Componente           |       API Name                         |Exemplo                  |Observação |
| -------------------  | ------------------------------------|-------------------------|-----------|
| Atribuição           |  DescriçãoDoComponente + **Assign** |OpportunityAssign   |Nome do objeto só é necessário caso o contexto seja relacionado a um objeto|
| Decisão              |  DescriçãoDoComponente              |ContractStatus      |Nome da API de Resultado = RótuloDecisão + NomeAPIDoComponente. **Ex: ActiveContractStatus**|
| Loop                 |  DescriçãoDoComponente + **Loop**   |ContractLineItemLoop     |           |
| Criar registros      |  DescriçãoDoComponente + **Insert** |OpportunityLineItemInsert|           |
| Atualizar registros  |  DescriçãoDoComponente + **Update** |CaseUpdate               |           |
| Obter registros      |  DescriçãoDoComponente + **Select** |OpportunitySelect        |           |


# Prefixos utilizados


| Componente                 |       API Name                                   |Exemplo                  |Observação            |
| ---------------------------|--------------------------------------------------|-------------------------|-------------------   |
| Alerta de Email            |  **EA** + NomeDoObjeto + NomeDoAlerta            |EACaseSurveyInvite       |EA - Email Alert      |
| Modelo de Email            |  **ET** + NomeDoObjeto + NomeDoModelo            |ETCaseSurveyInvite       |ET - Email Template   |
| Regra de fluxo de trabalho |  **WR** + NomeDoObjeto + NomeDaRegra             |WROpportunityExternalID  |WR - Workflow Rule    |
| Atualização de campo       |  **FU** + NomeDoObjeto + NomeDoCampo + Descrição |FUCaseStatusClose        |FU - Field Update     |
| Layout compacto            |  **CL** + NomeDoObjeto + Descrição               |CLCasePartnerLayout      |CL - Compact Layout   |
| Página Lightning           |  **LP** + NomeDoObjeto + Descrição               |LPCaseClientLayout       |LP - Lightning Page   |
| Process Builder            |  **PB** + NomeDoObjeto + Descrição               |PBCaseAutomation         |PB - Process Builder  |
| Flow                       |  **flow** + NomeDoObjeto + Descrição             |flowContactBeforeInsert  |                      |
| Conjunto de permissão      |  **PS** + Descrição                              |PSContractManager        |PS - Permission Set   |

[boaspraticas]: https://help.salesforce.com/s/articleView?id=sf.process_considerations_design_bestpractices.htm&type=5
