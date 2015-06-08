##_Seller_* Não VTEX Vendendo em _Marketplace_* Hospedado na VTEX
 
Este documento tem por objetivo auxiliar na integração e atualização de preço e estoque de um _SKU*_ entre um Seller não VTEX  para uma loja hospedada na VTEX e também auxiliar na descida de pedido e envio de autorização de despacho para o Seller não VTEX.

_Seller_* = Dono do produto, responsável por fazer a entrega.  
_Marketplace_* = Dono da vitrine, responsável por expor e vender o SKU.  
_SKU_* = Item a ser trocado e vendido entre Seller e Marketplace.  

###Troca de Catalogo e Atualização de Preço e Estoque de SKU


Sugestão de SKU, atualização de preço e estoque. Toda vez que houver uma alteração no preço ou no estoque de um SKU no Seller, o Seller deve enviar uma notificação de mudança de SKU para a loja hospedada na VTEX, caso a loja retorne em seu serviço o **response status 404**, significa que a **SKU não existe na loja**, então o Seller deve enviar a sugestão de SKU para a loja, caso a loja retorne em seu serviço o **response status 200 ou 202**, significa que a **SKU existe** na loja, então a loja vai no Seller consultar o novo preço ou estoque.

_Exemplo do fluxo:_

![alt text](sku-sugestion-seller-nao-vtex.png "Fluxo de troca de catalogo")

###Enviar Notificação de Mudança de Preço e Estoque de SKU


Toda vez que houver uma alteração no preço ou estoque de um SKU no Seller, o Seller deve enviar uma notificação de mudança de SKU para a loja hospeada na VTEX.

<a title="notificar mudança de sku no marketplace" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/CATALOG/CATALOG_Notification" target="_blank">[Developer] - Exemplo de Request de Notificação de Mudança - Endpoint da loja hospedada na VTEX</a>


###Enviar Sugestão de SKU


Quando o serviço de notificação descrito acima retornar um **response status 404**, significa que o SKU **NÂO existe** no marketplace hospedado na VTEX, então o Seller envia um POST com os dados da SKU que deseja sugerir para vender no marketplace. O Seller faz as sugestões de suas SKUs e o administrador do Marketplace realiza o mapeamento de marcas e categorias através do admin da loja, e aceita ou não a sugestão de SKU enviada pelo Seller.

<a title="envia sugestão de sku" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/CATALOG/CATALOG_Sugestion" target="_blank">[Developer] - Exemplo de Request de Inserção de Sugestão de SKU - Endpoint da loja hospedada VTEX</a>

_Exemplo do POST de dados:_

```json	
{
  "BrandId": null,
  "BrandName": "RAY BAN",
  "CategoryFullPath": "Oculos/Oculos de Sol/Masculino",
  "CategoryId": null,
  "EAN": [
    "0123456789123"
  ],
  "Height": 0.5,
  "Id": null,
  "Images": [
    {
      "ImageUrl": "http://www.cristalliotica.com.br/image/magictoolbox_cache/8c95d73fec130487c102a73bf1ab42ce/1/1/113/thumb450x450/2790576687/zoom-RAY%20BAN%204187%20Chris%206077_4V.jpg",
      "ImageName": "Principal",
      "FileId": null
    },
    {
      "ImageUrl": "http://www.cristalliotica.com.br/image/magictoolbox_cache/8c95d73fec130487c102a73bf1ab42ce/1/1/113/thumb450x450/2436023899/zoom-RAY%20BAN%204187%20Chris%206077_4V%201.jpg",
      "ImageName": "Lateral",
      "FileId": null
    }
  ],
  "IsAssociation": false,
  "IsKit": false,
  "IsProductSuggestion": false,
  "Length": 10,
  "ListPrice": 39900,
  "ModalId": null,
  "Price": 39900,
  "ProductDescription": "Oculos de sol feito de material de primira qualidade, com lentes anti reflexo e astes confortáveis",
  "ProductId": null,
  "ProductName": "Oculos de Sol RAY BAN",
  "ProductSpecifications": [
    {
      "FieldId": 0,
      "FieldName": "Origem",
      "FieldValueIds": null,
      "FieldValues": [
        "Importado"
      ]
    },
    {
      "FieldId": 0,
      "FieldName": "Capa Inclusa",
      "FieldValueIds": null,
      "FieldValues": [
        "Sim"
      ]
    }
  ],
  "ProductSupplementaryFields": null,
  "RefId": null, //obrigatório quando o EAN não for enviado
  "SellerId": "Cristalli",
  "SellerModifiedDate": null,
  "SellerStockKeepingUnitId": "cristalli00011", // tem que trocar esse id para os testes
  "SkuId": null,
  "SkuName": "Oculos de Sol RAY BAN Lente Polarizada",
  "SkuSpecifications": [
    {
      "FieldId": 0,
      "FieldName": "Lente Polarizada",
      "FieldValueIds": null,
      "FieldValues": [
        "Sim"
      ]
    }
  ],
  "SkuSupplementaryFields": null,
  "SynonymousPropertyNames": null,
  "WeightKg": 0.2,
  "Width": 0.5
}
```


###Atualização de Preço e ou Estoque de SKU


Toda vez que houver uma alteração no preço ou estoque, o Seller deve enviar uma notificação de mudança de SKU para a loja hospedadana VTEX, caso a loja retorne em seu serviço o **response status 200 ou 202**, significa que a SKU **existe** na loja, então a loja vai no Seller consultar o novo preço ou estoque.


<a title="busca de condições comerciais no seller" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/FULFILLMENT/FULFILLMENT_Simulation" target="_blank">[Developer] - Exemplo de Request de Busca de Condições Comerciais - Endpoint do Seller</a> 

_Exemplo do POST de dados:_

```json	
{
  "postalCode": "22251-030",
  "country": "BRA",
  "items": [
    {
      "id": "2000037",
      "quantity": 1,
      "seller": "1"
    },
    {
      "id": "34562",
      "quantity": 2,
      "seller": "1"
    }
  ]
}
``` 

---

###Simular Compra no Seller


Quando um SKU é inserido ou editado no carrinho do marketplace hospedado na VTEX, uma consulta de simulaçao de carrinho é feita no Seller para checar a validade de preço e estoque, e caso o CEP seja conhecido, retorna se também o frete e as SLAs de entrega para cada tipo de entrega. Quando se navega para a página de pagamento um outra checagem é realizada no Seller (_repare que é o mesmo endpoint usado na troca de preço e estoque).  


<a title="checar disponibilidade do carrinho no seller" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/FULFILLMENT/FULFILLMENT_Simulation" target="_blank">[Developer] - Simular Compra no Seller - Endpoint do Seller</a> 


###Enviar Pedido e Autorizar Despacho


Este tópico tem por objetivo auxiliar o Seller a receber um pedido do Marketplace, e receber a autorização para despachar o pedido.

_Exemplo do fluxo:_

![alt text](order-seller-nao-vtex.png "Fluxo de pedido") 

####Enviar Pedido

Quando o pedido é fechado no ambiente VTEX, um POST é feito no Seller não VTEX, para que este possa receber a ordem de pedido.  
 
####Exemplos de Request de Descida de Pedido - Endpoint do Seller

endpoint: **https://sellerendpoint/api/fulfillment/pvt/orders?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc=5** // sc serve para destacar o canal por onde o pedido entrou

_request:_  

```json	
{
"marketplaceOrderId": "959311095", //tem que trocar esse id para testes
"marketplaceServicesEndpoint": "https://urlmarketplace/", //leia o tópico Invocando MarketplaceServicesEndpoint Actions
"marketplacePaymentValue": 11080,
"items": [
  {
    "id": "2002495",
    "quantity": 1,
    "seller": "1",
    "commission": 0,
    "freightCommission": 0,
    "price": 9990,
    "bundleItems": [], //serviços. Ex: embalagem pra presente.
    "itemAttachment": { 
      "name": null,
      "content": {}
    },
    "attachments": [], //customização do item, Ex:camisa com o numero 10
    "priceTags": [],
    "measurementUnit": null, unidade de medida
    "unitMultiplier": 0, unidade multipladora,Ex: venda por quilo
    "isGift": false
  }
],
"clientProfileData": {
  "id": "clientProfileData",
  "email": "32172239852@gmail.com.br",
  "firstName": "Jonas",
  "lastName": "Alves de Oliveira",
  "documentType": null,
  "document": "3244239851",
  "phone": "399271258",
  "corporateName": null,
  "tradeName": null,
  "corporateDocument": null,
  "stateInscription": null,
  "corporatePhone": null,
  "isCorporate": false,
  "userProfileId": null
},
"shippingData": {
  "id": "shippingData",
  "address": {
    "addressType": "Residencial",
    "receiverName": "Jonas Alves de Oliveira",
    "addressId": "Casa",
    "postalCode": "13476103",
    "city": "Americana",
    "state": "SP",
    "country": "BRA",
    "street": "JOÃO DAMÁZIO GOMES",
    "number": "311",
    "neighborhood": "SÃO JOSÉ",
    "complement": null,
    "reference": "Bairro Praia Azul / Posto de Saúde 17",
    "geoCoordinates": []
  },
  "logisticsInfo": [
    {
      "itemIndex": 0,
      "selectedSla": "Normal",
      "lockTTL": "8d",
      "shippingEstimate": "7d",
      "price": 1090,
      "deliveryWindow": null
    }
  ]
},
"openTextField": null,
"marketingData": null,
"paymentData":null
}
```

_response:_  

```json
{
"marketplaceOrderId": "959311095",
"orderId": "123543123",
"followUpEmail": "75c70c09dbb3498c9b3bbdee59bf0687@ct.vtex.com.br",
"items": [
  {
    "id": "2002495",
    "quantity": 1,
    "seller": "1",
    "commission": 0,
    "freightCommission": 0,
    "price": 9990,
    "bundleItems": [],
    "priceTags": [],
    "measurementUnit": "un",
    "unitMultiplier": 1,
    "isGift": false
  }
],
"clientProfileData": {
  "id": "clientProfileData",
  "email": "5c77abaea35f4cb6824b9326942c00e5@ct.vtex.com.br",
  "firstName": "JONAS",
  "lastName": "ALVES DE OLIVEIRA",
  "documentType": "cpf",
  "document": "32133239851",
  "phone": "1592712979",
  "corporateName": null,
  "tradeName": null,
  "corporateDocument": null,
  "stateInscription": null,
  "corporatePhone": null,
  "isCorporate": false,
  "userProfileId": null
},
"shippingData": {
  "id": "shippingData",
  "address": {
    "addressType": "Residencial",
    "receiverName": "JONAS ALVES DE OLIVEIRA",
    "addressId": "Casa",
    "postalCode": "13476103",
    "city": "Americana",
    "state": "SP",
    "country": "BRA",
    "street": "JOÃO DAMÁZIO GOMES",
    "number": "121",
    "neighborhood": "SÃO JOSÉ",
    "complement": null,
    "reference": "Bairro Praia Azul / Posto de Saúde 17",
    "geoCoordinates": []
  },
  "logisticsInfo": [
    {
      "itemIndex": 0,
      "selectedSla": "Normal",
      "lockTTL": "8d",
      "shippingEstimate": "5d",
      "price": 1090,
      "deliveryWindow": null
    }
  ]
},
"paymentData":null
}
```

_Exemplo do Retorno de Erro:_  

```json
{
	"error": {
	"code": "1",
	"message": "O verbo 'GET' não é compatível com a rota '/api/fulfillment/pvt/orders'",
	"exception": null
	}
}
```

####Enviar Autorização Para Despachar

Quando o pagamento do pedido é concluído no marketplace VTEX (pagamento válido), envia se ao Seller uma autorização para dar andamento no processo de fulfillmente do pedido.  
 
*Exemplos de Request de Autorização - Endpoint da Seller*

endpoint: **https://sellerendpoint/api/fulfillment/pvt/orders/[orderid]/fulfill?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc=5** // sc serve para destacar o canal por onde o pedido entrou.

_request:_    

```json
{
	"marketplaceOrderId": "959311095" //id do pedido originado no canal de vendas
}
``` 

_response:_  

```json
{
	"date": "2014-10-06 18:52:00",
	"marketplaceOrderId": "959311095",
	"orderId": "123543123",
	"receipt": "e39d05f9-0c54-4469-a626-8bb5cff169f8",
}
```

###Invocando Marketplace Services Endpoint Actions


O MarketplaceServicesEndpoint serve para receber informações do Seller referentes a nota fiscal e tracking de pedido. É permitido o envio de notas fiscais parciais, obrigando assim ao informador informar além dos valores da nota fiscal, os items que está mandando na nota fiscal parcial. O pedido na VTEX só andará pra o status FATURADO quando o valor total de todas as notas fiscais de um pedido forem enviadas.

####Exemplos de Request Para Informar Nota Fiscal - Endpoint da loja hospedada na VTEX

endpoint: **https://marketplaceServicesEndpoint/pvt/orders/[orderId]/invoice**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  

_request:_  

```json
{
    "type": "Output", //Output|Input, venda ou devolução
    "invoiceNumber": "NFe-00001", //numero da nota fiscal
    "invoiceKey": "123456789012345678901234", //opcional, chave de acesso da nota fiscal
    "courier": "", //opcional quando somente nota fiscal
    "trackingNumber": "", //opcional quando somente nota fiscal
    "trackingUrl": "",//opcional quando somente nota fiscal
    "items": [ //itens da nota
      {
        "id": "345117",
        "quantity": 1,
        "price": 9003
      }
    ],
    "issuanceDate": "2013-11-21T00:00:00", //data da nota
    "invoiceValue": 9508 //valor da nota, iten mais frete
}
```

_response:_  

```json
{
    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
    "orderId": "123543123",
    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
}
```

####Exemplos de Request Para Informar Tracking - Endpoint da loja hospedada na VTEX

endpoint: **https://marketplaceServicesEndpoint/pvt/orders/[orderId]/invoice**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  

_request:_  
  
```json
{
    "type": "Output",
    "invoiceNumber": "NFe-00001",
    "courier": "Correios", //transportadora
    "trackingNumber": "SR000987654321", //tracking number
    "trackingUrl": "http://traking.correios.com.br/sedex/SR000987654321", //url de tracking
    "items": [
      {
        "id": "345117",
        "quantity": 1,
        "price": 9003
      }
    ],
    "issuanceDate": "2013-11-21T00:00:00",
    "invoiceValue": 9508
}
```

_response:_  

```json
{
    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
    "orderId": "123543123",
    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
}
```

**A Nota Fiscal e o Tracking podem ser enviados na mesma chamada, basta prenncher todos os dados do DTO de POST.

###Enviar Solicitação de Cancelamento


Uma solicitação de cancelamento pode ser enviada, caso o pedido se encontre em um estado que se possa cancelar, o pedido será cancelado. 
 
####Exemplos de Request de Cancelamento - Endpoint da loja hospedada na VTEX

endpoint: **https://marketplaceServicesEndpoint/pvt/orders/[orderid]/cancel**  
verb: **GET**

Na ferramenta POSTMAN usando esse link [https://www.getpostman.com/collections/ba95d70124bba8e05fe0] é possível importar uma coleção de requests. Nela tem um exemplo de cada request citado nesse documento.

---

Autor: _Jonas Bolognim_  
Propriedade: _VTEX_