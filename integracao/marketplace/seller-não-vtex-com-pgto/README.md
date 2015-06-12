##Seller Não VTEX Vendendo em Marketplace Hospedado na VTEX Recebendo Pagamento

Este documento tem por objetivo auxiliar na integração de um _Seller_ **não** hospedado na plataforma VTEX para uma _Marketplace_ hospedado na plataforma VTEX. Nesse modelo são integrados produtos (_SKUs_), atualização de condição comercial (preço, estoque) de um SKU e também descida de pedido, dados de pagamento e envio de autorização de despacho para o Seller não hospedado na VTEX.


> _Seller_ = Dono do produto, responsável por fazer a entrega.</br>
> _Marketplace_ = Dono da vitrine, responsável por expor e vender o SKU.</br>
> _SKU_ = Item a ser trocado e vendido entre Seller e Marketplace.</br>


####Acões que deverão ser tomadas pelo Seller não hospedado na VTEX para implementação da integração:

1. Implementar chamada de notificação de mudança de preço e estoque - Seller vai chamar endpoint da VTEX.
    > Toda vez que o SKU mudar o preço e ou o estoque no Seller, o Seller tem que chamar esse endpoint da loja na VTEX.

    _exemplo da chamada:_</br>
    ``` http://sandboxintegracao.vtexcommercestable.com.br/api/catalog_system/pvt/skuSeller/changenotification/[idSeller]/[idskuSeller] ```

   [Exemplo Completo: Enviar Notificação de Mudança de Preço e Estoque de SKU](#a1)


2. Implementar chamada de inserção de de sugestão de SKU -  Seller vai chamar endpoint da VTEX.
    > Toda vez que o serviço de notificação de mudança retornar SKU não encontrada (404), o Seller deve inserir a sugestão na loja da VTEX.

    _exemplo da chamada:_</br>
    ``` http://sandboxintegracao.vtexcommercestable.com.br/api/catalog_system/pvt/sku/SuggestionInsertUpdatev2 ```

    [Exemplo Completo: Enviar Sugestão de SKU](#a2)


3. Implementar endpoint para consulta de politica comercial (preço e estoque) - VTEX chama endpoint do Seller.
    > A loja hospedada na VTEX usará esse metodo para buscar preço e estoque no Seller tanto na indexação quanto na simulação de carrinho.

    _exemplo da chamada:_</br>
    ``` https://[seller].com.br/pvt/orderForms/simulation?sc=1&an=mechantname ```

    [Exemplo Completo: Consultar Política Comercial](#a3) </br>
    [Exemplo Completo: Simulação de Carrinho](#a4)</br>

    > NOTA:
    >> O metodo que consulta preço e estoque e o metodo que simula carrinho são os mesmos, logo, requer somente uma implementação por parte do integrador do Seller.
    >> Este é um dos principais metodos da integração, precisa ter performance e disponibilidade, pois tem impacto direto no fechamento da compra no Marketplace

4. Implementar endpoint para consultas de parcelamento - VTEX chama endpoint do Seller.
    > A loja na VTEX irá usar esse endpoint para consultar os parcelamentos oferecidos pelo Seller.

    _exemplo da chamada:_</br>
    ``` https://[seller].com.br/installments/options?sc=1&an=mechantname ```

    [Exemplo Completo: Consultar Formas de Parcelamento no Seller](#a5)

    > NOTA:
    >> Precisa ter performance e disponibilidade, pois tem impacto direto no fechamento da compra no Marketplace


5. Implementar endpoint para receber um pedido - VTEX chama endpoint do Seller.
    > A loja na VTEX irá usar esse enpoint para colocar um pedido no Seller.

    _exemplo da chamada:_</br>
    ``` https://[seller].com.br/pvt/orders?sc=1&an=mechantname ```

    [Exemplo Completo: Colocar um Pedido no Seller](#a6)


6. Implementar endpoint para receber o pagamento - VTEX chama endpoint do Seller.
    > A loja na VTEX irá usar esse endpoint para enviar o pagamento para o Seller

    _exemplo da chamada:_</br>
    ``` https://[seller].com.br/pvt/payment?sc=1&an=mechantname ```

    [Exemplo Completo: Colocar um Pagamento no Seller](#a7)

    > Nos dados de pagamento vai a url de retorno, onde o Seller irá notificar a loja na VTEX sobre o status do pagamento, ou seja, o Seller deve implementar rotina de informar status de pagamento na loja VTEX


7. Implementar endpoint para fechar a transação e autorizar despacho - VTEX chama endpoint do Seller.
    > loja na VTEX irá usar esse endpoint para avisar o Seller que já sabe do pagamento aprovado,
e que o Seller já pode andar com o pedido.

    _exemplo da chamada:_</br>
    ``` https://[seller].com.br/pvt/orders/[orderid]/fulfill?sc=1&an=mechantname ```

    [Exemplo Completo: Autorizar o Seller a Despachar o Pedido](#a8)


8. Implemtar rotina de informar nota fiscal e tracking de um pedido.
    > Nos dados do pedido é enviado uma endpoint de serviços do Marketplace, o Seller deverá invocar esse endpoint tanto pra informar nota fiscal quanto dados de rastreamanto de transportadora. O Seller ainda pode solicitar um cancelamento de um pedido que ainda não enviou nota fiscal.

    _exemplo da chamada:_</br>
    ``` https://marketplaceServicesEndpoint/pub/orders/[orderId]/invoice ```</br>
    ``` https://marketplaceServicesEndpoint/pub/orders/[orderId]/cancel ```</br>

    [Exemplo Completo: Informar nota fiscal de um pedido](#a10)</br>
    [Exemplo Completo: Informar tracking de um pedido](#a11)</br>
    [Exemplo Completo: Solicitar cancelamento de um pedido sem nota fiscal](#a12)</br>

---

##Abaixo segue o passo a passo detalhado de cada fluxo:

###Troca de Catalogo e Atualização de Preço e Estoque de SKU

Sugestão de SKU, atualização de preço e estoque. Toda vez que houver uma alteração no preço ou no estoque de um SKU no Seller, o Seller deve enviar uma notificação de mudança de SKU para a loja hospedada na VTEX, caso a loja retorne em seu serviço o **response status 404**, significa que a **SKU não existe na loja**, então o Seller deve enviar a sugestão de SKU para a loja.</br>
Caso a loja retorne em seu serviço o **response status 200 ou 202**, significa que a **SKU existe** no Marketplace, então a Marketplace vai no Seller consultar o novo preço e ou estoque.

> ATENÇÂO:
>> Este modelo não comtempla atualizações de imagens e descrição de um SKU depois dele catalogado no Marketplace, ou seja, depois do SKU mapeado e aceito pelo Marketplace, somente preço e estoque serão atualizados dinamicamente.


_Exemplo do fluxo:_

![alt text](sku-sugestion-seller-nao-vtex.png "Fluxo de troca de catalogo")

<a name="a1"><a/>
###Enviar Notificação de Mudança de Preço e Estoque de SKU


Toda vez que houver uma alteração no preço ou estoque de um SKU no Seller, o Seller deve enviar uma notificação de mudança de SKU para a loja hospeada na VTEX.

<a title="notificar mudança de sku no marketplace" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/CATALOG/CATALOG_Notification" target="_blank">[Developer] - Exemplo de Request de Notificação de Mudança - Endpoint da loja hospedada na VTEX</a>


<a name="a2"><a/>
###Enviar Sugestão de SKU


Quando o serviço de notificação descrito acima retornar um **response status 404**, significa que o SKU **NÂO existe** no marketplace hospedado na VTEX, então o Seller envia um POST com os dados da SKU que deseja sugerir para vender no Marketplace. O Seller faz as sugestões de suas SKUs e o administrador do Marketplace realiza o mapeamento de marcas e categorias através da pagina de administração do Marketplace, e aceita ou não a sugestão de SKU enviada pelo Seller.

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

<a name="a3"><a/>
###Atualização de Preço e ou Estoque de SKU


Toda vez que houver uma alteração no preço ou estoque, o Seller deve enviar uma notificação de mudança de SKU para a loja hospedadana VTEX, caso a loja retorne em seu serviço o **response status 200 ou 202**, significa que a SKU **existe** na loja, então a loja vai no Seller consultar o novo preço ou estoque.


<a title="busca de condições comerciais no Seller" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/FULFILLMENT/FULFILLMENT_Simulation" target="_blank">[Developer] - Exemplo de Request de Busca de Condições Comerciais - Endpoint do Seller</a>

_Exemplo do POST de dados:_

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

> ATENÇÂO
>> O CEP e o país não são obrigatórios, mais quando enviar 1 deles o outro se torna obrigatório.

---

###Simulação de Carrinho e Consulta Parcelamento


Este tópico tem por objetivo auxiliar o integrador na simulação de carrinho, consultar parcelamento entre o marketplace VTEX com uma loja não VTEX. Simular um pedido e consultar as formas de parcelamento.

> ATENÇÃO
>> Esses metodos são usados no momento do fechamento da compra no Marketplace, por isso, é de suma importancia para uma boa conversão, que esses metodos sejam perfomáticos e de alta disponibilidade.

###No Carrinho e no Pagamento
Quando um produto é inserido no carrinho no marketplace VTEX, ou faz se alguma edição no carrinho, uma consulta de simulaçao de carrinho é feita no Seller para checar a validade das condiçoes comerciais (preço, estoque, frete e SLAs de entrega).

*Exemplo do fuxo de chamadas no carrinho:*

![alt text](fechamento-fluxo.png "Title")


<a name="a4"><a/>
###Simulação de Carrinho


Quando ocorre uma edição no carrinho, uma chamada será feita no Seller não VTEX para checar a disponibilidade do item. Quando o CEP não for enviado, retornar sem as informações de logistica - Endpoint do Seller


endpoint: **https://Sellerendpoint/pvt/orderForms/simulation?sc=[idcanal]?an=[mechantname]**</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json** </br>
Parametro: **sc=5** // sc é o id do canal de vendas</br>


_request:_

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

_response:_

```json
{
	"items": [ //pode vir um array vazio
	    {
	        "id": "287611",//obrigatório, string - identificador so SKU
	        "requestIndex": 0, //obrigatório, int - representa a posição desse item no array original (request)
	        "price": 7390, //obrigatório, int - preço por, os dois dígitos menos significativos são os centavos
	        "listPrice": 7490, //obrigatório, int - preço de, os dois dígitos menos significativos são os centavos
	        "quantity": 1, //obrigatório, int - retornar a quantidade solicitada ou a quantidade que consegue atender
	        "Seller": "1", //obrigatório, string - retonar o que foi passado no request
	    	"merchantName": "shopfacilfastshop", //nome do gateway (enviador do pagamento) criado na VTEX para o Seller
	        "priceValidUntil": "2014-03-01T22:58:28.143"  //pode ser nulo, string - data de validade do preço.
	        "offerings":[  //array opcional de ofertas, porém não pode ser nulo: enviar array vazio ou não enviar
	            {
	                "type":"Garantia",                             //obrigatório, string - tipo do serviço
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
	        "Seller": "1",
			"merchantName": "shopfacilfastshop",
	        "priceValidUntil": null
	    }
	],
	"logisticsInfo": [ //array de informações, quando produtos indisponíveis enviar vazio []
	    {
	        "itemIndex": 0, //obrigatório, int - index do array de items
	        "stockBalance": 99, //obrigatório, int - estoque que atende
	        "quantity": 1, //obrigatório, int - retornar a quantidade solicitada ou a quantidade que consegue atender
	        "shipsTo": [ "BRA", "USA" ],  //obrigatório, array de string com as siglas dos países de entrega
	        "slas": [  //obrigatório quando o CEP e país forem passados no request. Pode ser um array vazio
	            {
	                "id": "Expressa",  //obrigatório, string - identificador tipo entrega
	                "name": "Entrega Expressa",//obrigatório, string - nome do tipo entrega
	                "shippingEstimate": "2bd", //obrigatório, string - doas estimados para a entrega, bd == "business days"
	                "price": 1000 //obrigatório, int - custo da entrega, os dois dígitos menos significativos são os centavos
	                "availableDeliveryWindows": [  //opcional, janelas de entrega,  podendo ser um array vazio
	                ]
	            },
	            {
	                "id": "Agendada",
	                "name": "Entrega Agendada",
	                "shippingEstimate": "5d",  //d - days, bd -business days
	                "price": 800,
	                "availableDeliveryWindows": [
	                     {
	                        "startDateUtc": "2013-02-04T08:00:00+00:00",  //date, obrigatório se for enviado delivery window
	                        "endDateUtc": "2013-02-04T13:00:00+00:00", //date, obrigatório se for enviado delivery window
	                        "price": 0 //int, obrigatório se for enviado delivery window - o valor adicional da entrega agendada
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
	                "shippingEstimate": "5bd", //bd - business days
	                "price": 200
	            }
	        ]
	    }
	],
	"country":"BRA",   //string, nulo se não enviado
	"postalCode":"22251-030"   //string, nulo se não enviado
}
```

> ATENÇÃO!
> > O valor do frete deve ser retornado por item consultado.
> > Quando não for passado CEP retornar o array de SLAs vazio []
> > No campo quantity, retornar o solicitado ou a quantidade que consegue atender.


<a name="mechantname"><a/>
###Consulta de Opções de Parcelamento.


Quando cliente for para a página de pagamento, uma chamada será feita no Seller para buscar as formas de parcelamento das formas de pagamento. O Seller **não** VTEX deverá conhecer préviamente os ids das formas de pagamento do marketplace VTEX - Endpoint do Seller.

> Identificadores dos cartões mais comuns no gateway das lojas VTEX:
>> 1-American Express </br>
>> 2-Visa </br>
>> 3-Diners </br>
>> 8-Hipercard </br>
>> 4-Mastercard </br>


endpoint: **https://Sellerendpoint/installments/options?sc=[idcanal]&an=[mechantname]**</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **an=nomedaloja**</br>

_request:_

```json
{
  "PaymentSystemsIds":[1,2], //ids das formas de pagamento na loja VTEX
  "SubtotalAsInt":27280, //total que se deseja parcelar
  "Items":[
    {
      	"PriceAsInt":24800, //preço do SKU
     	"Quantity":1, //quantidade do SKU
     	"Id":1940388, //id do SKU
     	"SellerId":"1",
    	"SalesChannel":5 //id do canal de vendas no Seller, caso exista
    }
  ],
  "postalCode":"22051030" //CEP
}
```


_response:_

```json
[
    {
        "paymentSystem": 2, //int -identificador da forma de pagamento
        "name": "",
        "value": 27280, //int, valor solicitado para parcelar
        "installments": [
            {
                "count": 1, //int, numero de parcelas
                "value": 27280, //int, valor da parecela
                "interestRate": 0, //int, taxa de juros
                "hasInterestRate": false // booleana - tem juros?
            },
            {
                "count": 2,
                "value": 13640,
                "interestRate": 0,
                "hasInterestRate": false
            },
            {
                "count": 3,
                "value": 9093,
                "interestRate": 0,
                "hasInterestRate": false
            },
            {
                "count": 4,
                "value": 6820,
                "interestRate": 0,
                "hasInterestRate": false
            },
            {
                "count": 5,
                "value": 5456,
                "interestRate": 0,
                "hasInterestRate": false
            },
            {
                "count": 6,
                "value": 4547,
                "interestRate": 0,
                "hasInterestRate": false
            }
        ]
    },
    {
        "paymentSystem": 1,
        "name": "",
        "value": 27280,
        "installments": [
            {
                "count": 1,
                "value": 27280,
                "interestRate": 0,
                "hasInterestRate": false
            },
            {
                "count": 2,
                "value": 13640,
                "interestRate": 0,
                "hasInterestRate": false
            },
            {
                "count": 3,
                "value": 9093,
                "interestRate": 0,
                "hasInterestRate": false
            },
            {
                "count": 4,
                "value": 6820,
                "interestRate": 0,
                "hasInterestRate": false
            }
        ]
    }
]
```

---

###Enviar Pedido e Informar Pagamento


Este tópico tem por objetivo auxiliar o Seller não VTEX a receber um pedido, receber o respectivo pagamento do pedido, e comunicar a atualização de status de pagamento.


*Exemplo do fuxo de chamadas de descida de pedido, pagamento e atualização de status de pagamento:*

![alt text](pedido-pagamento-fluxo.png "Title")

<a name="a6"><a/>
###Enviar Pedido


Quando o pedido é fechado no ambiente do Marketplace hospedado na VTEX, um POST é feito no Seller, para que este possa receber a ordem de pedido - Endpoint do Seller.

endpoint: **https://sellerendpoint/pvt/orders?sc=[idcanal]&an=[mechantname]**</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **sc** // sc serve para destacar o canal por onde o pedido entrou</br>
Parametro: **mechantname** // afiliado que esta colocando o pedido

_request:_

```json
[
  {
    "marketplaceOrderId": "959311095", //identificador do pedido no market place
    "marketplaceServicesEndpoint": "https://marketplaceservicesendpoint/", //leia o tópico Invocando MarketplaceServicesEndpoint Actions
    "marketplacePaymentValue": 11080, //valor que o marketplace se compromete a pagar para o Seller
    "items": [
      {
        "id": "2002495", //identificadro da SKU no Seller
        "quantity": 1, //quantidade comprada
        "Seller": "1",
        "commission": 0,
        "freightCommission": 0,
        "price": 9990, //preço da SKU
        "bundleItems": [], //serviços. Ex: embalagem pra presente.
        "itemAttachment": {
          "name": null,
          "content": {}
        },
        "attachments": [], //customização do item, Ex:camisa com o numero 10
        "priceTags": [],
        "measurementUnit": null, //unidade de medida
        "unitMultiplier": 0, //unidade multipladora,Ex: venda por quilo
        "isGift": false
      }
    ],
    "clientProfileData": {
      "id": "clientProfileData",
      "email": "32172239852@gmail.com.br", //e-mail do cliente
      "firstName": "Jonas", //primeiro nome do ciente
      "lastName": "Alves de Oliveira", //sobrenome do cliente
      "documentType": null,
      "document": "3244239851", //documento
      "phone": "399271258", //fone
      "corporateName": null, //se pessoa juridica, razao social
      "tradeName": null, //se pessoa juridica, nome fantasia
      "corporateDocument": null, //se pessoa juridica, documento
      "stateInscription": null, //se pessoa juridica, iscrição estadual
      "corporatePhone": null, //se pessoa juridica, fone
      "isCorporate": false, //é pessoa juridica?
      "userProfileId": null
    },
    "shippingData": {
      "id": "shippingData",
      "address": {
        "addressType": "Residencial", //tipo do endereço
        "receiverName": "Jonas Alves de Oliveira", //nome do destinatário
        "addressId": "Casa", //identificador do endereço
        "postalCode": "13476103", //código postal
        "city": "Americana", //cidade
        "state": "SP", //unidade federativa
        "country": "BRA", //país
        "street": "JOÃO DAMÁZIO GOMES", //logradouro
        "number": "311", //número do endereço
        "neighborhood": "SÃO JOSÉ", //bairro
        "complement": null, //complemnto
        "reference": "Bairro Praia Azul / Posto de Saúde 17", //refrencia
        "geoCoordinates": []
      },
      "logisticsInfo": [
        {
          "itemIndex": 0, //index do array de itens
          "selectedSla": "Normal", //tipo de entrega
          "lockTTL": "8d", //dias de reserva
          "shippingEstimate": "7d", //dias estimados para a entrega
          "price": 1090, //preço da entrega
          "deliveryWindow": null //janela de entrega
        }
      ]
    },
    "openTextField": null,
    "marketingData": null,
    "paymentData":{
		"merchantName":"shopfacilfastshop" //gateway de redirect na vtex.
	}
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
   "paymentData":{
		"merchantName":"shopfacilfastshop", //devolver o parametro an recebido no request
		"merchantPaymentReferenceId":"123543123" //inteiro, id do pagamento, número que será enviado junto com o pagamento para conciliação.
	}
  }
]

```

> NOTA:
>> Retonar no campo paymentData.merchantPaymentReferenceId um identificador que será usado futuramente na conciliação do dado enviado no pagamento, ou seja, o valor que retornar nesse campo irá indentifiar o pagamrnto desse pedido, pois esse valor será enviado junto com o pagamento. Pode ser usado o identificador do pedido no Seller, caso o formato for inteiro.

_retorno de erro:_

```json
{
	"error": {
	"code": "1",
	"message": "O verbo 'GET' não é compatível com a rota '/api/fulfillment/pvt/orders'",
	"exception": null
	}
}
```

<a name="a7"><a/>
###Enviar Pagamento

Quando o pagamento do pedido é informado no Marketplace hospedado na VTEX, um POST é feito no Seller, para que este possa receber os dados referente ao pagamento do respectivo pedido - Endpoint do Seller.

endpoint: **https://sellerendpoint/pvt/payment?sc=[idcanal]&an=[mechantname]**</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **an=shopfacilfastshop** // an é o nome do gateway da loja que ta enviando o pagamento</br>


_request:_

```json
{
	"referenceId": "123543123", //merchantPaymentReferenceId retornado no request do place order
	"transactionId": "D3AA1FC8372E430E8236649DB5EBD08E", //identificador da transação
	"paymentData": {
		"id": "F5C1A4E20D3B4E07B7E871F5B5BC9F91", //identificador do pagamento
		"paymentSystem": 2, //identificador da forma depagamento
		"cardNumber": "4444333322221111", //numero do cartão
		"cardHolder": "JONAS ALVES DE OLIVEIRA", //nome do cartão
		"expiryMonth": 11, //mes de expiração
		"expiryYear": 16, //ano de expiração
		"value": 11080, //valor do pagamento
		"installments": 3, //numero de parcelas
		"cvv2": "123", //codigo verificador
		"billingAddress": { //endereço de cobrança
			"addressType": "residential",
			"street": "Rua Cinco De Julho",
			"number": "176",
			"complement": "801",
			"postalCode": "22051-030",
			"city": "Rio De Janeiro",
			"state": "RJ",
			"country": "BRA",
			"neighborhood": ""
		}
	},
	"clientData": { //dados do cliente
		"firstName": "JONAS",
		"lastName": "ALVES DE OLIVEIRA",
		"document": "08081268731",
		"corporateName": "",
		"tradeName": "",
		"corporateDocument": "",
		"isCorporate": "false"
	},
	"shippingValue": 3691, //valor da entrega
	"callbackUrl": "https://nomedaloja.vtexpayments.com.br/api/pvt/callback/vtxstd/transactions/D3AA1FC8372E430E8236649DB5EBD08E/payments/F5C1A4E20D3B4E07B7E871F5B5BC9F91/return", //**url para falar de volta com o gateway de pagamento
	"shoppingCart": { // carrinho
	    "items": [
	      {
	        "id": "2044360", //identificador do SKU no Seller
	        "name": "Calça com Rasgos Skinny Preto 36", //nome do SKU
	        "value": 6990, //preço do SKU
	        "quantity": 1, //quantidade do item
	        "priceTags": [
	          {
	            "name": "discount@shipping-1e522efe-9638-4f93-81d7-b280cdbef920#8bacb546-ac99-4107-a54a-c17cf53034c6",
	            "value": "-4,1"
	          }
	        ],
	        "components": null,
	        "shippingDiscount": 0,
	        "discount": 0,
	        "refId": "7911105",
	        "productId": "2017257"
	      },
	      {
	        "id": "2031300",
	        "name": "Calça Jeans Claro 38",
	        "value": 7990,
	        "quantity": 1,
	        "priceTags": [
	          {
	            "name": "discount@shipping-1e522efe-9638-4f93-81d7-b280cdbef920#8bacb546-ac99-4107-a54a-c17cf53034c6",
	            "value": "-4,1"
	          }
	        ],
	        "components": null,
	        "shippingDiscount": 0,
	        "discount": 0,
	        "refId": "7824867",
	        "productId": "2009639"
	      }
	    ],
	    "freight": 0, //frete
	    "orderUrl": "http://www.cea.com.br/admin/checkout/#/orders?q=v676163cea",
	    "tax": 0,
	    "shippingdate": null,
	    "shippingestimated": "6bd" //dias estimados para entrega
	  }
}
```

<a name="a8"><a/>
_Exemplo do Response e do POST Feito na CallbackUrl de Pagamento :_

```json
{
  	"paymentId" : "F5C1A4E20D3B4E07B7E871F5B5BC9F91",   // string, not null, Payment identifier sent on authorization request
	"status" : "",    // string, not null, [approved | denied | undefined]
  	"authorizationId": "", //id da autorização quando aprovado
  	"bankIssueInvoiceUrl":"urldoboleto" //url do boleto bancario
}
```

> NOTA:
>> O response de pagamento pode ser respondido como "undefined" enquanto o Seller não tem a informação sobre o pagamento. Em caso de marketplace e Seller aceitarem boleto, quando recebido um post de pagamento com o paymentSystem igual a boleto, o Seller deve gerar o boleto e responder imediatamente com a url de boleto preenchida.


<a name="a9"><a/>
###Enviar Autorização Para Despachar


Quando o pagamento do pedido é concluído no Seller (pagamento válido), um POST deverá ser feito na "callbackUrl" do pagamento, informando sucesso do pagamento ("status":"approved"), nesse momento o marketplace VTEX envia autorização para despachar o respectivo pedido no Seller - Endpoint da Seller

endpoint: **https://Sellerendpoint/pvt/orders/[orderid]/fulfill?sc=[idcanal]&an=[mechantname]**</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **orderid** // identificador do pedido gerado no Seller </br>
Parametro: **sc** // sc é o canal de vendas cadastrado no marketplace, serve para destacar o canal por onde o pedido entrou.</br>

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

##Invocando Marketplace Services Endpoint Actions


O MarketplaceServicesEndpoint serve para receber informações do Seller referentes a nota fiscal e tracking de pedido. É permitido o envio de notas fiscais parciais, obrigando assim ao informador informar além dos valores da nota fiscal, os items ele está mandando na nota fiscal parcial. O pedido na VTEX só andará pra o status FATURADO quando o valor total de todas as notas fiscais de um pedido forem enviadas.


<a name="a10"><a/>
###Informar Nota Fiscal


Quando o Seller não VTEX emitir a Nota Fiscal, deve informar as informações da Nota Fiscal - Endpoint VTEX

endpoint: **https://marketplaceServicesEndpoint/pub/orders/{orderId}/invoice**
verb: **POST**
Content-Type: **application/json**
Accept: **application/json**
Parametro: **orderId** // id do pedido na VTEX


_request:_

```json
{
    "type": "Output", //Output|Input (venda|devolução)
    "invoiceNumber": "NFe-00001", //numero da nota fiscal
    "courier": "", //quando é nota fiscal, dados de tracking vem vazio
    "trackingNumber": "", //quando é nota fiscal, dados de tracking vem vazio
    "trackingUrl": "",//quando é nota fiscal, dados de tracking vem vazio
    "items": [ //itens da nota
      {
        "id": "345117",
        "quantity": 1,
        "price": 9003
      }
    ],
    "issuanceDate": "2013-11-21T00:00:00", //data da nota
    "invoiceValue": 9508 //valor da nota
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
<a name="a11"><a/>
###Informar Tracking


Quando o Seller entregar o pedido para a transportadora, deve informar as informações de Tracking - Endpoint VTEX

endpoint: **https://marketplaceServicesEndpoint/pub/orders/[orderId]/invoice**</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>

_request:_

```json
{
    "type": "Output",
    "invoiceNumber": "NFe-00001",
    "courier": "Correios", //transportadora
    "trackingNumber": "SR000987654321", /tracking number
    "trackingUrl": "http://traking.correios.com.br/sedex/SR000987654321", url de tracking
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

> A Nota Fiscal e o Tracking podem ser enviados na mesma chamada, basta prenncher todos os dados do POST.


<a name="a12"><a/>
###Enviar Solicitação de Cancelamento


Uma solicitação de cancelamento pode ser enviada, caso o pedido se encontre em um estado que se possa cancelar, o pedido será cancelado - Endpoint VTEX

endpoint: **https://marketplaceServicesEndpoint/pvt/orders/[orderid]/cancel**</br>
verb: **GET**</br>

> Para cancelar um pedido com nota fiscal já informada, tem quer enviado uma Nota Fiscal do tipo Input.

---

Autor: _Jonas Bolognim_</br>
Propriedade: _VTEX_ &copy;</br>
