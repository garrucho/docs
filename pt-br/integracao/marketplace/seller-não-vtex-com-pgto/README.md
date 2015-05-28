---
layout: docs
title: Seller Não VTEX Vendendo em Marketplace VTEX Recebendo Pagamento
application: marketplace
docType: guide
---

##Seller Não VTEX Vendendo em Marketplace VTEX Recebendo Pagamento
 
Este documento tem por objetivo auxiliar na integração e atualização de condição comercial (preço, estoque, frete, SLAs de entrega) de um SKU* entre um Seller não VTEX  para uma loja hospedada na versão smartcheckout da VTEX e também auxiliar na descida de pedido, dados de pagamento e envio de autorização de despacho para o Seller não VTEX.

###Troca de Catalogo de SKU e Atualização de Condição Comercial de SKU  
{: #1 .slug-text}

Troca de catalogo (sugestão de SKU) entre o Seller não VTEX com um Marketplace hospedado na VTEX.  

<a href="http://lab.vtex.com/docs/integracao/guide/marketplace/seller-n%C3%A3o-vtex/index.html#troca-de-catalogo-de-sku-e-atualizao-de-condio-comercial-de-sku" target="_blank">[[Guide] Troca de catalogo entre o Seller não VTEX com um Marketplace hospedado na VTEX]</a>    

---

###Simulação de Carrinho e Consulta Parcelamento  
{: #2 .slug-text}

Este tópico tem por objetivo auxiliar o integrador na simulação de carrinho, consultar parcelamento entre o marketplace VTEX com uma loja não VTEX. Simular um pedido e consultar as formas de parcelamento.

###No Carrinho e no Pagamento
Quando um produto é inserido no carrinho no marketplace VTEX, ou faz se alguma edição no carrinho, uma consulta de simulaçao de carrinho é feita no Seller para checar a validade das condiçoes comerciais (preço, estoque, frete e SLAs de entrega).  

*Exemplo do fuxo de chamadas no carrinho:*  

![alt text](fechamento-fluxo.png "Title")  

###Simulação de Carrinho  
{: #2 .slug-text}  

Quando ocorre uma edição no carrinho, uma chamada será feita no Seller não VTEX para checar a disponibilidade do item. Quando o CEP não for enviado, retornar sem as informações de logistica - Endpoint do Seller  


endpoint: **https://sellerendpoint/pvt/orderForms/simulation?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc=5** // sc é o id do canal de vendas


_request:_  
{% highlight json %}  
{
    "postalCode":"22251-030",            //obrigatório se country estiver preenchido, string
    "country":"BRA",                     //obrigatório se postalCode estiver preenchido, string      
    "items": [                           //obrigatório: deve conter pelo menos um objeto item
        {
            "id":"287611",               //obrigatório, string
            "quantity":1,                //obrigatório-quantidade do item a ser simulada, int
            "seller":"seller1"           //sigla do do seller criado no admin // obrigatório, string
        },
        {
            "id":"5837",
            "quantity":5,
            "seller":"seller1"
        }
    ]
}
{% endhighlight %}  

_response:_  

{% highlight json %}  
{
	"items": [                                                     //pode vir um array vazio
	    {
	        "id": "287611",                                        //obrigatório, string
	        "requestIndex": 0,                                     //obrigatório, int - representa a posição desse item no array original (request)
	        "price": 7390,                                         //Os dois dígitos menos significativos são os centavos //obrigatório, int
	        "listPrice": 7490,                                     //Os dois dígitos menos significativos são os centavos //obrigatório, int
	        "quantity": 1,                                         //obrigatório, int
	        "seller": "1",                                         //Id do seller cadastrado na loja // obrigatório, string,
	    	"merchantName": "shopfacilfastshop",				   //nome do gateway (enviador do pagamento) criado na VTEX para o Seller.
	        "priceValidUntil": "2014-03-01T22:58:28.143"           //data, pode ser nulo
	        "offerings":[                                          //Array opcional, porém não pode ser nulo: enviar array vazio ou não enviar a propriedade
	            {
	                "type":"Garantia",                             //obrigatório, string
	                "id":"5",                                      //obrigatório, string
	                "name":"Garantia de 1 ano",                    //obrigatório, string
	                "price":10000                                  //Os dois dígitos menos significativos são os centavos //obrigatório, int
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
	        "price": 890,                                          //Os dois dígitos menos significativos são os centavos
	        "listPrice": 990,                                      //Os dois dígitos menos significativos são os centavos
	        "quantity": 5,
	        "seller": "1",
			"merchantName": "shopfacilfastshop",	
	        "priceValidUntil": null
	    }
	],
	"logisticsInfo": [                                            //obrigatório (se vier vazio é considerado que o item não está disponível) -  todos os itens devem ter os mesmos SLAs
	    {
	        "itemIndex": 0,                                       //obrigatório, int - representa os dados de sla do item de resposta (response)
	        "stockBalance": 99,                                   //obrigatório  quando o CEP foi passado no request, estoque, int
	        "quantity": 1,                                        //obrigatório quando o CEP foi passado no request, qauntidade pasada no request, int
	        "shipsTo": [ "BRA", "USA" ],                          //obrigatório, array de string com as siglas dos países de entrega
	        "slas": [                                             //obrigatório quando o CEP foi passado no request. Pode ser um array vazio
	            {
	                "id": "Expressa",                             //obrigatório, id tipo entrega, string
	                "name": "Entrega Expressa",                   //obrigatório, nome do tipo entrega, string
	                "shippingEstimate": "2bd",                    //bd == "business days" //obrigatório, string
	                "price": 1000                                 //Os dois dígitos menos significativos são os centavos, obrigatório, int
	                "availableDeliveryWindows": [                 //opcional, podendo ser um array vazio
	                ]
	            },
	            {
	                "id": "Agendada",
	                "name": "Entrega Agendada",
	                "shippingEstimate": "5d",                     //d == "days, bd == "business days"
	                "price": 800,
	                "availableDeliveryWindows": [
	                     {
	                        "startDateUtc": "2013-02-04T08:00:00+00:00",       //date, obrigatório se for enviado delivery window
	                        "endDateUtc": "2013-02-04T13:00:00+00:00",         //date, obrigatório se for enviado delivery window
	                        "price": 0        //int, obrigatório se for enviado delivery window - o valor total da entrega agendada é o valor base mais o valor desse campo
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
	                "shippingEstimate": "5bd",                    //bd == "business days"
	                "price": 200
	            }
	        ]
	    }
	],
	"country":"BRA",                                           //string, nulo se não enviado
	"postalCode":"22251-030"                                   //string, nulo se não enviado    
}
{% endhighlight %}  


###Consulta de Opções de Parcelamento.  
{: #2 .slug-text}  

Quando cliente for para a página de pagamento, uma chamada será feita no Seller para buscar as formas de parcelamento das formas de pagamento. O Seller não VTEX deverá conhcer préviamente os ids das formas de pagamento do marketplace VTEX (1-American Express, 2-Visa, 3-Diners, 8-Hipercard, 4-Mastercard) - Endpoint do Seller  
 
Identificadores dos cartões mais comuns no gateway das lojas VTEX:



endpoint: **https://sellerendpoint/installments/options?an=[nomedaloja]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **an=nomedaloja**

_request:_  

{% highlight json %}  
{
  "PaymentSystemsIds":[1,2], //ids das formas de pagamento
  "SubtotalAsInt":27280, //total que deseja parcelar
  "Items":[
    {
      	"PriceAsInt":24800, //preço do SKU
     	"Quantity":1, //quantidade do SKU
     	"Id":1940388, //id do SKU
     	"SellerId":"seller1",
    	"SalesChannel":2 //id do canal de vendas criado para o seller
    }
  ],
  "postalCode":"22051030" //CEP
}
{% endhighlight %}  

_response:_

{% highlight json %} 
[
    {
        "paymentSystem": 2,
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
{% endhighlight %} 

---

###Enviar Pedido e Informar Pagamento  
{: #4 .slug-text}  

Este tópico tem por objetivo auxiliar o Seller não VTEX a receber um pedido, receber o respectivo pagamento do pedido, e comunicar a atualização de status de pagamento.


*Exemplo do fuxo de chamadas de descida de pedido, pagamento e atualização de status de pagamento:*  

![alt text](pedido-pagamento-fluxo.png "Title") 

###Enviar Pedido  
{: #5 .slug-text}  

Quando o pedido é fechado no ambiente VTEX, um POST é feito no Seller não VTEX, para que este possa receber a ordem de pedido - Endpoint do Seller

endpoint: **https://sellerendpoint/pvt/orders?sc=[idcanal]&affiliateId[idafiliado]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **sc** // sc serve para destacar o canal por onde o pedido entrou  
Parametro: **affiliateId** // afiliado que esta colocando o pedido

_request:_  

{% highlight json %}  
[
  {
    "marketplaceOrderId": "959311095", //identificador do pedido no market place
    "marketplaceServicesEndpoint": "https://urlmarketplace/", //leia o tópico Invocando MarketplaceServicesEndpoint Actions
    "marketplacePaymentValue": 11080, //valor que o marketplace se compromete a pagar para o Seller
    "items": [
      {
        "id": "2002495", //identificadro da SKU no Seller
        "quantity": 1, //quantidade comprada
        "seller": "1",
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
{% endhighlight %} 

_response:_  

{% highlight json %}  
[
  {
    "marketplaceOrderId": "959311095",
    "orderId": "123543123", //** - identificador do pedido inserido no seller
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
   "paymentData":{
		"merchantName":"shopfacilfastshop", //devover o parametro recebido no request
		"merchantPaymentReferenceId":"123543123" //inteiro id do pagamento, número que será enviado junto com o pagamento para conciliação.
	}
  }
]  
{% endhighlight %} 


_retorno de erro:_  

{% highlight json %} 
{
	"error": {
	"code": "1",
	"message": "O verbo 'GET' não é compatível com a rota '/api/fulfillment/pvt/orders'",
	"exception": null
	}
}
{% endhighlight %}  


###Enviar Pagamento
{: #5 .slug-text}  

Quando o pagamento do pedido é informado no ambiente VTEX, um POST é feito no Seller não VTEX, para que este possa receber os dados referente ao pagamento do respectivo pedido - Endpoint do Seller

endpoint: **https://sellerendpoint/pvt/payment?an=shopfacilfastshop**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **an=shopfacilfastshop** // an é o nome do gateway da loja que ta enviando o pagamento


_request:_    

{% highlight json %} 
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
{% endhighlight %}


_Exemplo do Response e do POST Feito na CallbackUrl de Pagamento :_

{% highlight json %} 
{
  	"paymentId" : "F5C1A4E20D3B4E07B7E871F5B5BC9F91",   // string, not null, Payment identifier sent on authorization request
	"status" : "",    // string, not null, [approved | denied | undefined]
  	"authorizationId": "", //id da autorização quando aprovado
  	"bankIssueInvoiceUrl":"urldoboleto" //url do boleto bancario
}
{% endhighlight %}

**O response de pagamento pode ser respondido como "undefined" enquanto o Seller não tem a informação sobre o pagamento. Em caso de marketplace e seller aceitarem boleto, quando recebido um post de pagamento com o paymentSystem igual a boleto, o seller deve gerar o boleto e responder imediatamente com a url de boleto preenchida.
	    

###Enviar Autorização Para Despachar
{: #8 .slug-text}

Quando o pagamento do pedido é concluído no Seller (pagamento válido), um POST deverá ser feito na "callbackUrl" do pagamento, informando sucesso do pagamento ("status":"approved"), nesse momento o marketplace VTEX envia autorização para despachar o respectivo pedido no Seller - Endpoint da Seller

endpoint: **https://sellerendpoint/pvt/orders/[orderid]/fulfill?sc=[idcanal]**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **orderid** // identificador do pedido gerado no seller
Parametro: **sc** // sc é o canal de vendas cadastrado no marketplace, serve para destacar o canal por onde o pedido entrou.  

_request:_  
  
{% highlight json %}
{
	"marketplaceOrderId": "959311095" //id do pedido originado no canal de vendas
}
{% endhighlight %}  

_response:_  

{% highlight json %}
{
	"date": "2014-10-06 18:52:00",
	"marketplaceOrderId": "959311095",
	"orderId": "123543123",
	"receipt": "e39d05f9-0c54-4469-a626-8bb5cff169f8",
}
{% endhighlight %}  

##Invocando Marketplace Services Endpoint Actions
{: #9 .slug-text}

O MarketplaceServicesEndpoint serve para receber informações do Seller referentes a nota fiscal e tracking de pedido. É permitido o envio de notas fiscais parciais, obrigando assim ao informador informar além dos valores da nota fiscal, os items ele está mandando na nota fiscal parcial. O pedido na VTEX só andará pra o status FATURADO quando o valor total de todas as notas fiscais de um pedido forem enviadas.

###Informar Nota Fiscal 
{: #10 .slug-text}

Quando o Seller não VTEX emitir a Nota Fiscal, deve informar as informações da Nota Fiscal - Endpoint VTEX

endpoint: **https://marketplaceServicesEndpoint/pub/orders/{orderId}/invoice**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  
Parametro: **orderId** // id do pedido na VTEX


_request:_  

{% highlight json %}  
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
{% endhighlight %} 

_response:_  

{% highlight json %} 
{
    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
    "orderId": "123543123",
    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
}
{% endhighlight %} 

###Informar Tracking 
{: #10 .slug-text}  

Quando o Seller não VTEX entregar o pedido para a transportadora, deve informar as informações de Tracking - Endpoint VTEX

endpoint: **https://marketplaceServicesEndpoint/pub/orders/[orderId]/invoice**  
verb: **POST**  
Content-Type: **application/json**  
Accept: **application/json**  

_request:_  

{% highlight json %} 
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
{% endhighlight %}  

_response:_  

{% highlight json %} 
{
    "date": "2014-02-07T15:22:56.7612218-02:00", //data do recibo
    "orderId": "123543123",
    "receipt": "38e0e47da2934847b489216d208cfd91" //protocolo gerado, pode ser nulo
}
{% endhighlight %}  

**A Nota Fiscal e o Tracking podem ser enviados na mesma chamada, basta prenncher todos os dados do POST.

###Enviar Solicitação de Cancelamento
{: #10 .slug-text}  

Uma solicitação de cancelamento pode ser enviada, caso o pedido se encontre em um estado que se possa cancelar, o pedido será cancelado - Endpoint VTEX

endpoint: **https://marketplaceServicesEndpoint/pvt/orders/[orderid]/cancel**  
verb: **GET**

Obs=Para cancelar um pedido com Nota Fiscal Informada, tem quer enviado uma Nota Fiscal do tipo Input.

---

Autor: _Jonas Bolognim_  
Propriedade: _VTEX_ &copy;