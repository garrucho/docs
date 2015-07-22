##Seller Não VTEX Vendendo em Marketplace Hospedado na VTEX

Este documento tem por objetivo auxiliar na integração de um _Seller_ **não** hospedado na plataforma VTEX para uma _Marketplace_ hospedado na plataforma VTEX. Nesse modelo são integrados produtos (_SKUs_), atualização de condição comercial (preço, estoque) de um SKU, além de descida de pedido e envio de autorização de despacho de pedido para o Seller.

> _Seller_ = Dono do produto, responsável por fazer a entrega.</br>
> _Marketplace_ = Dono da vitrine, responsável por expor e vender o SKU.</br>
> _SKU_ = Item a ser trocado e vendido entre Seller e Marketplace.</br>


####Ações que deverão ser tomadas pelo Seller não hospedado na VTEX para implementação da integração:

1. Implementar chamada de notificação de mudança de preço e estoque - Seller vai chamar endpoint da VTEX.    Toda vez que o SKU mudar o preço e ou o estoque no Seller, o Seller tem que chamar esse endpoint da loja na VTEX, simplesmente comunicando a mudança. Ao receber esse request o Marketplace vai buscar o preço e estoque no Seller no método de consulta politica comercial que vamos falar mais abaixo.

    _exemplo da chamada:_</br>
    ``` https://sandboxintegracao.vtexcommercestable.com.br/api/catalog_system/pvt/skuSeller/changenotification/[idSeller]/[idskuSeller] ```

    [Exemplo Completo: Enviar Notificação de Mudança de Preço e Estoque de SKU](#a1)

2. Implementar chamada de inserção de de sugestão de SKU -  Seller vai chamar endpoint da VTEX. Toda vez que o serviço de notificação de mudança retornar SKU **não** encontrada (404), o Seller deve inserir a sugestão na loja da VTEX.

    _exemplo da chamada:_</br>
    ``` https://sandboxintegracao.vtexcommercestable.com.br/api/catalog_system/pvt/sku/SuggestionInsertUpdatev2 ```

    [Exemplo Completo: Enviar Sugestão de SKU](#a2)

3. Implementar endpoint para consulta de politica comercial (preço e estoque) - VTEX chama endpoint do Seller.
    A loja hospedada na VTEX usará esse metodo para buscar preço e estoque no Seller tanto na indexação (catalogar preço e estoque), quanto na simulação de carrinho.

    _exemplo da chamada:_</br>
    ``` https://[seller].com.br/pvt/orderForms/simulation?sc=1&an=mechantname ```

     > NOTA:
     >> O metodo que consulta preço e estoque e o metodo que simula carrinho são os mesmos, mas por uma questão de construção de cache requer duas implementaçãoes, uma sendo GET e outra POST.
     >> Este é um dos principais metodos da integração, precisa ter performance e disponibilidade, pois tem impacto direto no fechamento da compra no Marketplace.

 [Exemplo Completo: Consultar Política Comercial](#a3) </br>
 [Exemplo Completo: Simulação de Carrinho  - POST e GET](#a4) </br>

4. Implementar endpoint para receber um pedido - VTEX chama endpoint do Seller. A loja na VTEX irá usar esse endpoint para colocar um pedido no Seller.

    _exemplo da chamada:_</br>
    ``` https://[seller].com.br/pvt/orders?sc=1&an=mechantname ```

    [Exemplo Completo: Colocar um Pedido no Seller](#a6)

5. Implementar endpoint para autorizar despacho - VTEX chama endpoint do Seller. A loja na VTEX irá usar esse endpoint para avisar o Seller que já pode entregar com o pedido.

    _exemplo da chamada:_</br>
    ``` https://[seller].com.br/pvt/orders/[orderid]/fulfill?sc=1&an=mechantname ```

    [Exemplo Completo: Autorizar o Seller a Despachar o Pedido](#a8)

6. Implementar rotina de informar dados de nota fiscal e rastreamento de entrega de um pedido. Nos dados do pedido é enviado uma endpoint de serviços do Marketplace, o Seller deverá invocar esse endpoint tanto pra informar dados de nota fiscal quanto dados de rastreamento de transportadora. O Seller ainda pode solicitar um cancelamento de um pedido que ainda não enviou nota fiscal.

    _exemplo da chamada:_</br>
    ``` https://marketplaceServicesEndpoint/pub/orders/[marketplaceorderId]/invoice ```</br>
    ``` https://marketplaceServicesEndpoint/pub/orders/[marketplaceorderId]/cancel ```</br>

    [Exemplo Completo: Informar nota fiscal de um pedido](#a10)</br>
    [Exemplo Completo: Informar tracking de um pedido](#a11)</br>
    [Exemplo Completo: Solicitar cancelamento de um pedido sem nota fiscal](#a12)</br>

7. Rota de validação de cadastro - Auxiliar o diagnostico de problemas de integração que tenham causa, incompatibilidade de configuração entre os envolvidos (Marketplace e Seller) - para o futuro, desconsiderar

---

##Abaixo segue o passo a passo detalhado de cada fluxo:

###Troca de Catalogo e Atualização de Preço e Estoque de SKU

Sugestão de SKU, atualização de preço e estoque. Toda vez que houver uma alteração no preço ou no estoque de um SKU no Seller, o Seller deve enviar uma notificação de mudança de SKU para a loja hospedada na VTEX, caso a loja retorne em seu serviço o **response status 404**, significa que a **SKU não existe na loja**, então o Seller deve enviar a sugestão de SKU para a loja.</br>
Caso a loja retorne em seu serviço o **response status 200 ou 202**, significa que a **SKU existe** no Marketplace, então a Marketplace vai no Seller consultar o novo preço e ou estoque.

> ATENÇÃO:
>> Este modelo **não** contempla atualizações de imagens e descrição de um SKU depois de catalogado (aceito) no Marketplace, ou seja, depois do SKU mapeado e aceito pelo Marketplace, somente preço e estoque serão atualizados dinamicamente.


_exemplo do fluxo:_

![alt text](sku-sugestion-seller-nao-vtex.png "Fluxo de troca de catalogo")

<a name="a1"><a/>
###Enviar Notificação de Mudança de Preço e Estoque de SKU


Toda vez que houver uma alteração no preço ou estoque de um SKU no Seller, o Seller deve enviar uma notificação de mudança de SKU para a loja hospedada na VTEX.

<a title="notificar mudança de sku no marketplace" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/CATALOG/CATALOG_Notification" target="_blank">[Developer] - Exemplo de Request de Notificação de Mudança - Endpoint da loja hospedada na VTEX</a>


<a name="a2"><a/>
###Enviar Sugestão de SKU para Venda


Quando o serviço de notificação descrito acima retornar um **response status 404**, significa que o SKU **NÃO existe** no Marketplace hospedado na VTEX, então o Seller envia um POST com os dados da SKU que deseja sugerir para vender no Marketplace.

> O Seller faz as sugestões de suas SKUs e o administrador do Marketplace realiza o mapeamento de marcas e categorias através da pagina de administração do Marketplace, e aceita ou não a sugestão de SKU enviada pelo Seller.

<a title="envia sugestão de sku" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/CATALOG/CATALOG_Sugestion" target="_blank">[Developer] - Exemplo de Request de Inserção de Sugestão de SKU - Endpoint da loja hospedada na VTEX</a>

_exemplo do POST de dados:_

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
  "RefId": null, // obrigatório quando o EAN não for enviado
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

<a name="a3"><a/>
###Atualização de Preço e ou Estoque de SKU

Toda vez que houver uma alteração no preço ou estoque, o Seller deve enviar uma notificação de mudança de SKU para a loja hospedada na VTEX, caso a loja retorne em seu serviço o **response status 200 ou 202**, significa que a SKU **existe** na loja, então a loja vai no Seller consultar o novo preço ou estoque.

<a title="busca de condições comerciais no Seller" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/FULFILLMENT/FULFILLMENT_Simulation" target="_blank">[Developer] - Exemplo de Request de Busca de Condições Comerciais - Endpoint do Seller</a>

_exemplo por POST:_

```json
{
  "items": [
    {
      "id": "2000037",
      "quantity": 1,
      "Seller": "1"
    },
    {
      "id": "34562",
      "quantity": 2,
      "Seller": "1"
    }
  ],
  "marketingData": null,
  "postalCode": "22051030", //não obrigatório
  "country": "BRA", //não obrigatório
  "selectedSla": null,
  "clientProfileData": null,
  "geoCoordinates": []
}
```

_Exemplo por GET:_

QueryString com UrlEncode: </br>
``` purchaseContext=%7b%22items%22%3a%5b%7b%22id%22%3a%2213%22%2c%22quantity%22%3a1%2c%22seller%22%3a%221%22%7d%5d%2c%22country%22%3a%22BRA%22%7d&sc=1&an=shopfacilfastshop ``` </br>
QueryString decodificada: </br>
``` purchaseContext={"items":[{"id":"2002129","quantity":1,"seller":"1"}],"marketingData":null,"postalCode":"22011050","country":"BRA","selectedSla":null,"clientProfileData":null,"geoCoordinates":[]}&sc=1&an=shopfacilfastshop ``` </br>

> ATENÇÂO
>> O CEP e o país não são obrigatórios, mais quando tiver 1 deles o outro se torna obrigatório.

---

###Simulação de Carrinho

Este tópico tem por objetivo auxiliar o integrador na simulação de carrinho, entre o Marketplace hospedado na VTEX com um Seller **não** VTEX. Quando um produto (SKU) é inserido no carrinho no marketplace VTEX, ou faz se alguma edição no carrinho, uma consulta de simulação de carrinho é feita no Seller para checar a validade das condiçoes comerciais (preço, estoque, frete e SLAs de entrega).


> ATENÇÃO
>> Esses metodos são usados no momento do fechamento da compra no Marketplace, por isso, é de suma importancia para uma boa conversão, que esses metodos sejam perfomáticos e de alta disponibilidade.

_exemplo do fluxo de chamadas no carrinho:_

![alt text](fechamento-fluxo-nao-vtex.png "fluxo de fechamento de pedido")


<a name="a4"><a/>
###Simulação de Carrinho

Quando ocorre uma edição no carrinho, uma chamada será feita no Seller para checar a disponibilidade dos itens - Endpoint do Seller.

endpoint: ```https://[sellerendpoint]/pvt/orderForms/simulation?sc=[idcanal]&an=[mechantname]```</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>

> ATENÇÃO
>> Este método também requer uma implementação em GET, que será usada para construção
>> de cache do lado da plataforma VTEX, evitando tantos acessos ao POST. </br>

> PARÂMETROS
>> ?**sc**=[idcanal]**an**=[mechantname]. Esses parametros servem para o Seller fazer o controle de qual Marketplace está fazendo a chamada em seus serviços, pois, esse modelo, uma vez bem implementado servirá para vender em qualquer Marketplace hospedado na VTEX, dando ao Seller a oportunidade de vender em N Marketplace ao mesmo tempo.
>>>**sc**=1&**an**=marketplaceseller, onde **sc** seria a campanha (será enviado 1 como padrão) e **an** seria o identificador do marketplace. Opcional o uso pelo Seller.


_request por POST:_

```json
{
  "items": [
    {
      "id": "2000037", //identificador do item no Seller
      "quantity": 1, //quantidade desejada
      "seller": "1"
    },
    {
      "id": "34562",
      "quantity": 2,
      "seller": "1"
    }
  ],
  "marketingData": null,
  "postalCode": "22051030", // não obrigatório
  "country": "BRA", // não obrigatório
  "selectedSla": null,
  "clientProfileData": null,
  "geoCoordinates": []
}
```

Na pratilheira e na página de detalhe de produto uma chamada **GET** será feita no Seller para checar a disponibilidade dos itens - Endpoint do Seller.

endpoint: ``` https://[sellerendpoint]/pvt/orderForms/simulation?purchaseContext=%7b%22items%22%3a%5b%7b%22id%22%3a%2213%22%2c%22quantity%22%3a1%2c%22seller%22%3a%221%22%7d%5d%2c%22country%22%3a%22BRA%22%7d&sc=idCanal&an=merchantName ```</br>
verb: **GET**</br>
Accept: **application/json**</br>
Parametro: **purchaseContext** - Esse parâmetro na QueryString é o mesmo JSON do POST serializado e com URLEncode</br>

_request Por GET:_

QueryString com UrlEncode: </br>
``` purchaseContext=%7b%22items%22%3a%5b%7b%22id%22%3a%2213%22%2c%22quantity%22%3a1%2c%22seller%22%3a%221%22%7d%5d%2c%22country%22%3a%22BRA%22%7d&sc=1&an=shopfacilfastshop ``` </br>
QueryString decodificada: </br>
``` purchaseContext={"items":[{"id":"2002129","quantity":1,"seller":"1"}],"marketingData":null,"postalCode":"22011050","country":"BRA","selectedSla":null,"clientProfileData":null,"geoCoordinates":[]}&sc=1&an=shopfacilfastshop ``` </br>


_Via POST:_</br>
``` https://[sellerendpoint]/pvt/orderForms/simulation ```</br>
```json
{"items":[{"id":"13","quantity":1,"seller":"1"}],"country":"BRA"}
```

_Via GET:_</br>
``` https://[sellerendpoint]/pvt/orderForms/simulation?purchaseContext=%7b%22items%22%3a%5b%7b%22id%22%3a%2213%22%2c%22quantity%22%3a1%2c%22seller%22%3a%221%22%7d%5d%2c%22country%22%3a%22BRA%22%7d ```

_response (POST e GET):_

```json
{
	"items": [ //pode vir um array vazio caso item insidponivel
	    {
	        "id": "287611",// obrigatório, string - identificador so SKU
	        "requestIndex": 0, // obrigatório, int - representa a posição desse item no array original (request)
	        "price": 7390, // obrigatório, int - preço por, os dois dígitos menos significativos são os centavos
	        "listPrice": 7490, // obrigatório, int - preço de, os dois dígitos menos significativos são os centavos
	        "quantity": 1, // obrigatório, int - retornar a quantidade solicitada ou a quantidade que consegue atender
	        "seller": "1", // obrigatório, string - retonar o que foi passado no request
	        "priceValidUntil": "2014-03-01T22:58:28.143"  // pode ser nulo, string - data de validade do preço.
	        "offerings":[  //array opcional de ofertas, porém não pode ser nulo: enviar array vazio.
	            {
	                "type":"Garantia", //obrigatório, string - tipo do serviço
	                "id":"5",  //obrigatório, string - identificador do serviço
	                "name":"Garantia de 1 ano", //obrigatório, string - nome do serviço
	                "price":10000  //obrigatório, int - preço do serviço, os dois dígitos menos significativos são os centavos
	            },
	            {
	                "type":"Embalagem de Presente",
	                "id":"6",
	                "name":"Embalagem de Presente",
	                "price":250
	            }
	        ]
	    },
	    {
	        "id": "5837",
	        "requestIndex": 1,
	        "price": 890,
	        "listPrice": 990,
	        "quantity": 5,
	        "seller": "1",
	        "priceValidUntil": null
	    }
	],
	"logisticsInfo": [ // array de informações, quando produtos indisponíveis retornar vazio []
	    {
	        "itemIndex": 0, // obrigatório, int - index do array de items
	        "stockBalance": 99, // obrigatório, int - estoque que atende
	        "quantity": 1, // obrigatório, int - retornar a quantidade solicitada ou a quantidade que consegue atender
	        "shipsTo": [ "BRA"],  // obrigatório, array de string com as siglas dos países de entrega
	        "slas": [  // obrigatório,  pode ser um array vazio na ausencia de CEP.
	            {
	                "id": "Expressa",  //obrigatório, string - identificador tipo entrega
	                "name": "Entrega Expressa",// obrigatório, string - nome do tipo entrega
	                "shippingEstimate": "2bd", // obrigatório, string - doas estimados para a entrega, bd == "business days"
	                "price": 1000 // obrigatório, int - custo da entrega, os dois dígitos menos significativos são os centavos
	                "availableDeliveryWindows": [  // opcional, janelas de entrega,  podendo ser um array vazio
	                ]
	            },
	            {
	                "id": "Agendada",
	                "name": "Entrega Agendada",
	                "shippingEstimate": "5d",  // d - days, bd -business days
	                "price": 800,
	                "availableDeliveryWindows": [
	                     {
	                        "startDateUtc": "2013-02-04T08:00:00+00:00",  // date, obrigatório se for enviado delivery window
	                        "endDateUtc": "2013-02-04T13:00:00+00:00", // date, obrigatório se for enviado delivery window
	                        "price": 0 // int, obrigatório se for enviado delivery window - o valor adicional da entrega agendada
	                    },
	                ]
	            }
	        ]
	    },
	    {
	        "itemIndex": 1,
	        "stockBalance": 1237,
	        "quantity": 5,
	        "shipsTo": [ "BRA" ],
	        "slas": [
	            {
	                "id": "Normal",
	                "name": "Entrega Normal",
	                "shippingEstimate": "5bd", // bd - business days
	                "price": 200
	            }
	        ]
	    }
	],
	"country":"BRA",   // string, nulo se não enviado
	"postalCode":"22251-030"   // string, nulo se não enviado
}
```

> ATENÇÃO!
> > O valor do frete deve ser retornado por item consultado.
> > Quando não for passado CEP retornar o array de SLAs vazio []
> > No campo quantity, retornar o solicitado ou a quantidade que consegue atender.


###Enviar Pedido e Autorizar Despacho

Este tópico tem por objetivo auxiliar o Seller a receber um pedido do Marketplace, e receber a autorização para despachar o pedido.

_exemplo do fluxo:_

![alt text](order-seller-nao-vtex.png "Fluxo de pedido")

<a name="a6"><a/>
###Enviar Pedido

Quando o pedido é fechado no ambiente do Marketplace hospedado na VTEX, um POST é feito no Seller, para que este possa receber a ordem de pedido - endpoint do Seller.

endpoint: ```https://sellerendpoint/pvt/orders?sc=[idcanal]&an=[mechantname]```</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>

_request:_

```json
[
  {
    "marketplaceOrderId": "959311095", // identificador do pedido no market place
    "marketplaceServicesEndpoint": "https://marketplaceservicesendpoint/", // leia o tópico Invocando MarketplaceServicesEndpoint Actions
    "marketplacePaymentValue": 11080, //valor que o marketplace se compromete a pagar para o Seller
    "items": [
      {
        "id": "2002495", // identificadro da SKU no Seller
        "quantity": 1, // quantidade comprada
        "Seller": "1",
        "commission": 0,
        "freightCommission": 0,
        "price": 9990, // preço da SKU
        "bundleItems": [], // serviços. Ex: embalagem pra presente.
        "itemAttachment": {
          "name": null,
          "content": {}
        },
        "attachments": [], // customização do item, Ex:camisa com o numero 10
        "priceTags": [],
        "measurementUnit": null, // unidade de medida
        "unitMultiplier": 0, // unidade multipladora, Ex: venda por quilo
        "isGift": false
      }
    ],
    "clientProfileData": {
      "id": "clientProfileData",
      "email": "32172239852@gmail.com.br", // e-mail do cliente
      "firstName": "Jonas", // primeiro nome do ciente
      "lastName": "Alves de Oliveira", // sobrenome do cliente
      "documentType": null,
      "document": "3244239851", // documento
      "phone": "399271258", // fone
      "corporateName": null, // se pessoa juridica, razao social
      "tradeName": null, // se pessoa juridica, nome fantasia
      "corporateDocument": null, // se pessoa juridica, documento
      "stateInscription": null, // se pessoa juridica, iscrição estadual
      "corporatePhone": null, // se pessoa juridica, fone
      "isCorporate": false, // é pessoa juridica?
      "userProfileId": null
    },
    "shippingData": {
      "id": "shippingData",
      "address": {
        "addressType": "Residencial", // tipo do endereço
        "receiverName": "Jonas Alves de Oliveira", // nome do destinatário
        "addressId": "Casa", // identificador do endereço
        "postalCode": "13476103", // código postal
        "city": "Americana", // cidade
        "state": "SP", // unidade federativa
        "country": "BRA", // país
        "street": "JOÃO DAMÁZIO GOMES", // logradouro
        "number": "311", // número do endereço
        "neighborhood": "SÃO JOSÉ", // bairro
        "complement": null, // complemnto
        "reference": "Bairro Praia Azul / Posto de Saúde 17", // refrencia
        "geoCoordinates": []
      },
      "logisticsInfo": [
        {
          "itemIndex": 0, // index do array de itens
          "selectedSla": "Normal", // tipo de entrega
          "lockTTL": "8d", // dias de reserva
          "shippingEstimate": "7d", // dias estimados para a entrega
          "price": 1090, // preço da entrega
          "deliveryWindow": null // janela de entrega
        }
      ]
    },
    "openTextField": null,
    "marketingData": null,
    "paymentData":null
  }
]
```

_response:_

```json
[
  {
    "marketplaceOrderId": "959311095",
    "orderId": "123543123", //** - identificador do pedido inserido no Seller
    "followUpEmail": "75c70c09dbb3498c9b3bbdee59bf0687@ct.vtex.com.br",
    "items": [
      {
        "id": "2002495",
        "quantity": 1,
        "Seller": "1",
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
]

```

_exemplo do retorno de Erro:_

```json
{
	"error": {
	"code": "1",
	"message": "O verbo 'GET' não é compatível com a rota '/api/fulfillment/pvt/orders'",
	"exception": null
	}
}
```

<a name="a8"><a/>
###Enviar Autorização Para Despachar

Quando o pagamento do pedido é concluído no marketplace VTEX (pagamento válido), envia se ao Seller uma autorização para dar andamento no processo de entrega do pedido - endpoint da Seller.

endpoint: ```https://Sellerendpoint/pvt/orders/[orderid]/fulfill?sc=[idcanal]&an=[mechantname]```</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>

_request:_

```json
{
	"marketplaceOrderId": "959311095" // identificador do pedido originado no Marketplace
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

##Invocando Marketplace Services Endpoint Actions

O MarketplaceServicesEndpoint serve para receber informações do Seller referentes a nota fiscal e rastreamento de entrega de pedido. É permitido o envio de notas fiscais parciais, obrigando assim ao informador informar além dos valores da nota fiscal, os itens ele está mandando na nota fiscal parcial. O pedido na VTEX só andará pra o status FATURADO quando o valor total de todas as notas fiscais de um pedido forem enviadas.


<a name="a10"><a/>
###Informar Dados de Nota Fiscal

Quando o Seller emitir a nota fiscal, deve enviar as informações da nota fiscal - endpoint plataforma VTEX.

endpoint: ```https://marketplaceServicesEndpoint/pub/orders/[marketplaceorderId]/invoice```
verb: **POST**
Content-Type: **application/json**
Accept: **application/json**
Parametro: **orderId** // id do pedido no Seller

_request:_

```json
{
    "type": "Output", // Output|Input (venda|devolução)
    "invoiceNumber": "NFe-00001", // numero da nota fiscal
    "courier": "", // quando é nota fiscal, dados de tracking vem vazio
    "trackingNumber": "", // quando é nota fiscal, dados de tracking vem vazio
    "trackingUrl": "",// quando é nota fiscal, dados de tracking vem vazio
    "items": [ // itens da nota
      {
        "id": "345117",
        "quantity": 1,
        "price": 9003
      }
    ],
    "issuanceDate": "2013-11-21T00:00:00", // data de emissao da nota
    "invoiceValue": 9508 // valor da nota
}
```

_response:_

```json
{
    "date": "2014-02-07T15:22:56.7612218-02:00", // data do recibo
    "orderId": "123543123",
    "receipt": "38e0e47da2934847b489216d208cfd91" // protocolo gerado, pode ser nulo
}
```
<a name="a11"><a/>
###Informar Rastreamento de Entrega


Quando o Seller entregar o pedido para a transportadora, deve informar as informações de rastreamento - endpoint plataforma VTEX.

endpoint: ``` https://marketplaceServicesEndpoint/pub/orders/[marketplaceorderId]/invoice ```</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>

_request:_

```json
{
    "type": "Output",
    "invoiceNumber": "NFe-00001",
    "courier": "Correios", // transportadora
    "trackingNumber": "SR000987654321", // identificador de rastreamentor
    "trackingUrl": "http://traking.correios.com.br/sedex/SR000987654321", // url de rastreamento
    "items": [
      {
        "id": "345117",
        "quantity": 1,
        "price": 9003
      }
    ],
    "issuanceDate": "2013-11-21T00:00:00", // formato esperado
    "invoiceValue": 9508
}
```

_response:_

```json
{
    "date": "2014-02-07T15:22:56.7612218-02:00", // data do recebimento
    "orderId": "123543123",
    "receipt": "38e0e47da2934847b489216d208cfd91" // protocolo gerado confirmando o recebimento do POST (GUID)
}
```

> A Nota Fiscal e o Tracking podem ser enviados na mesma chamada, basta preencher todos os dados do POST.


<a name="a12"><a/>
###Enviar Solicitação de Cancelamento


Uma solicitação de cancelamento pode ser enviada, caso o pedido se encontre em um estado que se permita cancelar, o pedido será cancelado - endpoint plataforma VTEX.

endpoint: ``` https://marketplaceServicesEndpoint/pvt/orders/[marketplaceorderId]/cancel ```</br>
verb: **GET**</br>

> Para cancelar um pedido com nota fiscal já informada, enviar uma Nota Fiscal do tipo Input com o valor cheio do pedido.


>POR LER ATE AQUI
>>Na ferramenta POSTMAN usando esse link ``` https://www.getpostman.com/collections/ba95d70124bba8e05fe0 ``` é possível importar uma coleção de requests. Nela tem um exemplo de cada request citado nesse documento.

---

Autor: _Jonas Bolognim_</br>
Propriedade: _VTEX_ &copy;</br>
