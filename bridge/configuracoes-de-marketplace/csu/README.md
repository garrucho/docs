## Configurações iniciais com o CSU

## Primeiros Passos

> A CSU **não** precisa de contratação de Channel.

1. [Seleção de política comercial.](http://help.vtex.com/hc/pt-br/articles/214166227)

2. [Configure a logística para essa política comercial.](http://help.vtex.com/hc/pt-br/articles/214166667-Atualiza%C3%A7%C3%A3o-de-estoque)

3. Entrar em contato com a Buscapé e obtenha o Token de acesso a API:

 contato@optemais.com.br

## Bridge - Visão geral

### Exemplo de acesso as Configurações de Marketplace

![Config CSU](Marketplace_Config.gif)

##Campos

![Config CSU](CSU.png)

>>Para cada campo temos:

* **Id do LicenseManager**: Id da sua Loja presente no campo "ID*" dentro do LicenseManager.
* **Política Comercial**: ID da Política Comercial do Walmart cadastrada em OMS > Configurações > Afiliados > Política Comercial.
* **Id do Afiliado**: ID do Afiliado da B2W cadastrado em OMS > Configurações > Afiliados > ID.

## OMS

Acessar: Configurações > Afiliados > Novo Afiliado

![Config CSU](rakuten_novo_afiliado.gif)

Aqui você terá os seguintes campos para serem preenchidos:

![Config CSU](Configuracoes_iniciais_CSU.jpg)

* **Nome**: Nome do Afiliado/Marketplace. Para os Marketplaces da CSU usar:
* CSU: CSU
* CSU-Santos: CSU-Santos
* CSU-Vivo: CSU-Vivo
* CSU-Souza Cruz: CSU-Souza Cruz
* Loyalty CSU: Loyalty CSU
* Loyalty Porto Seguro: Loyalty Porto Seguro
* ID: Código de Identificação do Afiliado com 3 dígitos. Para os Marketplaces da CSU usar:
* CSU: CSW
* CSU-Santos: STS
* CSU-Vivo: CVV
* CSU-Vivo: VVV
* CSU-Souza Cruz: SSC
* Loyalty CSU: CSL
* Loyalty Porto Seguro: CPS
* **Política Comercial**: ID da Política Comercial onde está definido o sortimento de Produtos a serem trabalhados no Marketplace
* **E-mail de FollowUp**: Endereço de e-mail que receberá qualquer notificação de conflito nas integrações de Pedidos (LicenseManager > Conta > Contato).
* **Endpoint de Search**: Será o mesmos para os diversos Marketplaces da CSU: http://www.optemais.com.br/WebServices/wsIntegracaoWeb/Proxy.svc/NotificacaoSku
* **Versão do Endpoint de Search**: 1
Usar meu meio de pagamento: marcar somente quando for utilizar o seu meio de pagamento. **Obs**.: Para todos os loyaltys a flag não deverá ser marcada.


## License Manager

Acessar License Manager > Usuários e buscar pelo Nome do respectivo Marketplace:

![Config CSU](image06.gif)

Para cada Marketplace temos usuários específicos associados à aplicação do Parceiro e também perfis diferentes que devem ser associados. Para os diversos Marketplaces da CSU teremos o mesmo seguinte usuário e perfil:

* **Email (appKey)**: vtexappkey-parceirocsu-ERUXDY
* **Perfil**: IntegrationProfile-Fulfillment_Gateway_GetOrder
Fazendo-o da seguinte forma:

![Config CSU](image02.gif)
