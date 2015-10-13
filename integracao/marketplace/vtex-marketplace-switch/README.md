##Marketplace ou Aplicativos vendendo para Sellers usando o VTEX Marketplace Switch

Este documento tem por finalidade auxiliar na integração de aplicativos com o _VTEX Marketplace Switch_. Este modelo contempla troca de catalogo, atualização de condição comercial (preço, estoque) de um _SKU_, além de auxiliar na descida de pedido e traking de entrega.

> ALGUNS CONCEITOS:
>> _VTEX Marketplace Switch_ = Concentrador de Sellers e SKUs (lojas integradas ao Switch).</br>
>> _Aplicativos_ = Afiliado, dono da vitrine (cara com o cliente final), responsável por expor e vender o SKU.</br>
>> _SKU_ = Item a ser trocado e vendido entre Marketplace e Seller.</br>
>> _Endpoint_ = Ponto de acesso de um serviço na internet, serviço pronto para receber uma requisição e devolver uma resposta.</br>


####Ações que deverão ser tomadas pelo aplicativo para implementação da integração:

1. Implementar o endpoint para receber notificação de mudança de preço e estoque - VTEX Marketplace Switch vai chamar endpoint do aplicativo. Toda vez que o SKU mudar no VTEX Marketplace Switch, o VTEX Marketplace Switch vai chamar esse endpoint do aplicativo, simplesmente comunicando a mudança. Ao receber esse request o aplicativo vem buscar o preço e estoque no VTEX Marketplace Switch no metodo de consulta politica comercial que vamos falar mais abaixo.

 _exemplo da chamada:_</br>
 ``` https://marketplace.com.br/api/notification/ ```

 [Exemplo Completo: Enviar Notificação de Mudança](#a1)

2. Implementar busca de dados de SKU no VTEX Marketplace Switch - Aplicativo vai chamar endpoint do VTEX Marketplace Switch. Toda vez que o serviço de notificação de mudança do VTEX Marketplace Switch avisar sobre uma SKU, e o aplicativo ainda nao tem a SKU catalogada, o aplicativo vem no VTEX Marketplace Switch buscar os dados da SKU.

 _exemplo da chamada:_</br>
 ```http://sandboxintegracao.vtexcommercestable.com.br/api/catalog_system/pvt/sku/stockkeepingunitbyid/310118210 ```</br>
 ```http://sandboxintegracao.vtexcommercestable.com.br/api/catalog_system/pvt/sku/stockkeepingunitbyid/310118190 ```</br>

 [Exemplo Completo: Buscar Dados de SKU](#a2)

3. Implementar busca de politica comercial (preço e estoque, frete, tipo entrega e custo) - Aplicativo vai chamar endpoint do VTEX Marketplace Switch. Toda vez que o serviço de notificação de mudança do VTEX Marketplace Switch avisar sobre mudança de uma SKU (já catalogada), o aplicativo busca preço e ou estoque no VTEX Marketplace Switch pra se atualizar.

 _exemplo da chamada:_</br>
 ``` http://sandboxintegracao.vtexcommercestable.com.br/api/checkout/pub/orderForms/simulation?sc=1&affiliateId=LBB ```

 [Exemplo Completo: Consultar Política Comercial no VTEX Marketplace Switch](#a3)
 [Exemplo Completo: simula Carrinho no VTEX Marketplace Switch](#a8)

 >Os metodos de buscar condições comerciais e simular carrinho são os mesmos.

4. Implementar rotina que coloca um pedido no VTEX Marketplace Switch - Aplicativo vai chamar endpoint do VTEX Marketplace Switch. O Aplicativo irá usar esse enpoint para colocar um pedido no VTEX Marketplace Switch.

 _exemplo da chamada:_</br>
 ``` https://sandboxintegracao.vtexcommercestable.com.br/api/checkout/pub/orders?sc=1&affiliateId=LBB ```

 [Exemplo Completo: Colocar um Pedido no VTEX Marketplace Switch](#a4)

5. Implementar rotina que informa pagamento no VTEX Marketplace Switch - Aplicativo vai chamar endpoint do VTEX Marketplace Switch. O aplicativo irá usar esse enpoint para informar um pagamento no PCI gateway do VTEX Marketplace Switch.

 _exemplo da chamada:_</br>
 ``` https://sandboxintegracao.vtexpayments.com.br/split/564031077937/payments ``` </br>
 </br>
 _564031077937=identificador do grupo do pedido_  </br>

 [Exemplo Completo: Informa Pagamento no VTEX Marketplace Switch](#a5)

6. Implementar rotina que informa sucesso no processo de criação do pedido no VTEX Marketplace Switch - Aplicativo vai chamar endpoint do VTEX Marketplace Switch. O aplicativo irá usar esse enpoint para fechar o processo de criação de pedido do VTEX Marketplace Switch.

 _exemplo da chamada:_</br>
 ``` https://sandboxintegracao.vtexcommercestable.com.br/checkout/gatewayCallback/564032167396/Success ```

 [Exemplo Completo: Finaliza Fluxo de Fechamento de Pedido no VTEX Marketplace Switch](#a6)

7. Implementar rotina que busca dados do pagamento no PCI gateway do VTEX Marketplace Switch - Aplicativo vai chamar endpoint do PCI gateway do VTEX Marketplace Switch. O aplicativo irá usar esse enpoint para buscar dados do pagamento no PCI gateway do VTEX Marketplace Switch. Dentro do dados de pagamento recuperar o id do pagamento para ser usado no passo 8 abaixo.

 _exemplo da chamada:_</br>
 ``` https://sandboxintegracao.vtexpayments.com.br/api/pvt/transactions/4497E676DD3544CFAD27D0FB9201D3F4/payments ```</br>
 </br>
 _4497E676DD3544CFAD27D0FB9201D3F4=identificador da transação_ </br>

 [Exemplo Completo: Informa sucesso do pagamento PCI gateway do do VTEX Marketplace Switch](#a7)

8. Implementar rotina que informa sucesso na aprovação do pagamento no PCI gateway do VTEX Marketplace Switch - Aplicativo vai chamar endpoint do PCI gateway do VTEX Marketplace Switch. O aplicativo irá usar esse enpoint para aprovar o pagamento no PCI gateway do VTEX Marketplace Switch.

 _exemplo da chamada:_</br>
 ``` https://sandboxintegracao.vtexpayments.com.br/api/pvt/payments/0BBE8A2A3B81477587AC1C3726369794/payment-notification ```</br>
 </br>
 _0BBE8A2A3B81477587AC1C3726369794=identificador do pagamento_ </br>

 [Exemplo Completo: Informa sucesso do pagamento no PCI gateway do VTEX Marketplace Switch](#a8)

9. Ouve o feed de status de pedido - Endpoint do VTEX Marketplace Switch. O aplicativo deve se subescrever no feed de status de pedidos, ouvir e tratar o status que lhe interessa e confirmar os status que vai ignorar.

 [Exemplo Completo: Ouvir feed de status de pedido](#a9)</br>


 - - -

###Abaixo segue o exemplo detalhado de cada fluxo:

###Troca de Catalogo de SKU e Atualização de Condição Comercial de SKU

* Dentro do VTEX Marketplace Switch, será criado um afiliado, que é o interessado em receber o catálogo e as atualizações de condições comerciais.

* O afiliado, deverá implementar um endpoint onde receberá informações sobre alterações de SKUs.

* Na loja VTEX, cada vez que uma SKU comercializado pelo afiliado sofrer uma alteração na condição comercial, ou for reindexado, o endpoint de notificação do afiliado será chamado.

* Quando o afiliado recebe uma notificação que um SKU mudou, o afiliado deve checar se já comercializa aquela SKU daquela loja que comunicou.

  * **Caso SIM**: O afiliado consulta a condição comercial no VTEX Marketplace Switch e se atualiza.

  * **Caso NÃO**: O afiliado busca a SKU no VTEX Marketplace Switch, insere no seu catalogo, e depois repete o cenário do "Caso SIM".

> As requisições de notificação de mudança devem ser tratadas assincronamente, ou seja, recebe a notificação, coloca se numa fila, e depois processa, evitando assim a honeração dos sistemas envolvidos.

<a name="a1"></a>
###Notificação de Mudança de SKU

Notifica o aplicativo que houve uma mudança de uma SKU - Endpoint do Afiliado (Aplicativo). Onotificação de SKu vai com duas flags. indicando se a mudança foi de preço ou estoque. Caso essas 2 flags foram enviadas como falsas, é porque uma outra mudança ocorreu na SKU - descrição, peso, etc (nesses casos o aplicativo pode optar por buscar a SKU no VTEX Marketplace Switch para se atualizar).

endpoint: ``` https://[endpointdoafiliado}/api/notification/ ```</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>

_request:_

```json
{
	"IdSku" : "310118183", //identificador da SKU
	"An" : "sandboxintegracao", //loja vtex
	"IdAffiliate":"LBB", // id do afilado cadastrado na loja vtex
	"StockModified": true, //mudou estoque?
	"PriceModified": true //mudou preço?
}
```

<a name="a3"></a>
###Busca de Condições Comerciais

Acessa o VTEX Marketplace Switch pegando as condições comerciais (preço, estoque, SLAs de entrega) de uma SKU  - Endpoint do VTEX Marketplace Switch

endpoint: ``` https://[loja].vtexcommercestable.com.br/api/checkout/pub/orderForms/simulation?sc=[idpoliticacomercial]&affiliateId=[idafiliado] ```</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **sc** // sc é a politica comercial praticada</br>
Parametro: **affiliateId** // o id do afiliado cadastrado no VTEX Marketplace Switch</br>

_request:_

```json
{
  "items": [ // lista de itens
    {
      "id": "310118190", //identificado do SKU no Marketplace Switch
      "quantity": 1, // quantidade solicitada
      "seller": "walmartv5" //identificado do Seller no Marketplace Switch
    },
    {
      "id": "310118210",
      "quantity": 1,
      "seller": "parceiro01"
    }
  ],
  "postalCode": null, //opcional, CEP
  "country": null //opcional, sigla do país
}
```

_response:_

```json
{
  "items": [
    {
      "id": "310118190",
      "requestIndex": 0,
      "quantity": 1,
      "seller": "walmartv5",
      "tax": 0,
      "priceValidUntil": null,
      "price": 39901,
      "listPrice": 52900,
      "sellingPrice": 39901,
      "offerings": [
        {
          "type": "servico",
          "id": "38",
          "name": "servico",
          "allowGiftMessage": false,
          "attachmentOfferings": [
            {
              "name": "Informações",
              "required": false,
              "schema": {
                "texto": {
                  "maximumNumberOfCharacters": 23,
                  "domain": [
                    "teste1",
                    " teste2",
                    " teste3"
                  ]
                }
              }
            },
            {
              "name": "placa",
              "required": false,
              "schema": {
                "Número": {
                  "maximumNumberOfCharacters": 1,
                  "domain": []
                },
                "Município": {
                  "maximumNumberOfCharacters": 30,
                  "domain": [
                    "RIO DE JANEIRO"
                  ]
                },
                "País": {
                  "maximumNumberOfCharacters": 3,
                  "domain": []
                }
              }
            }
          ],
          "price": 1000
        }
      ],
      "priceTags": [],
      "measurementUnit": "un",
      "unitMultiplier": 1
    },
    {
      "id": "310118210",
      "requestIndex": 1,
      "quantity": 1,
      "seller": "parceiro01",
      "tax": 0,
      "priceValidUntil": null,
      "price": 249,
      "listPrice": 3000,
      "sellingPrice": 249,
      "offerings": [
        {
          "type": "Embalagem Simples",
          "id": "1",
          "name": "Embalagem Simples",
          "allowGiftMessage": true,
          "attachmentOfferings": [
            {
              "name": "message",
              "required": false,
              "schema": {
                "text": {
                  "maximumNumberOfCharacters": 300,
                  "domain": []
                }
              }
            }
          ],
          "price": 250
        }
      ],
      "priceTags": [],
      "measurementUnit": "un",
      "unitMultiplier": 1
    }
  ],
  "ratesAndBenefitsData": {
    "attachmentId": "ratesAndBenefitsData",
    "rateAndBenefitsIdentifiers": [],
    "teaser": []
  },
  "paymentData": {
    "installmentOptions": [
      {
        "paymentSystem": "1",
        "bin": null,
        "paymentName": "American Express",
        "paymentGroupName": "creditCardPaymentGroup",
        "value": 40150,
        "installments": [
          {
            "count": 1,
            "hasInterestRate": false,
            "interestRate": 0,
            "value": 40150,
            "total": 40150,
            "sellerMerchantInstallments": [
              {
                "id": "SANDBOXINTEGRACAO",
                "count": 1,
                "hasInterestRate": false,
                "interestRate": 0,
                "value": 40150,
                "total": 40150
              }
            ]
          }
        ]
      },
      {
        "paymentSystem": "2",
        "bin": null,
        "paymentName": "Visa",
        "paymentGroupName": "creditCardPaymentGroup",
        "value": 40150,
        "installments": [
          {
            "count": 1,
            "hasInterestRate": false,
            "interestRate": 0,
            "value": 40150,
            "total": 40150,
            "sellerMerchantInstallments": [
              {
                "id": "SANDBOXINTEGRACAO",
                "count": 1,
                "hasInterestRate": false,
                "interestRate": 0,
                "value": 40150,
                "total": 40150
              }
            ]
          },
          {
            "count": 2,
            "hasInterestRate": false,
            "interestRate": 0,
            "value": 20075,
            "total": 40150,
            "sellerMerchantInstallments": [
              {
                "id": "SANDBOXINTEGRACAO",
                "count": 2,
                "hasInterestRate": false,
                "interestRate": 0,
                "value": 20075,
                "total": 40150
              }
            ]
          },
          {
            "count": 3,
            "hasInterestRate": false,
            "interestRate": 0,
            "value": 13383,
            "total": 40150,
            "sellerMerchantInstallments": [
              {
                "id": "SANDBOXINTEGRACAO",
                "count": 3,
                "hasInterestRate": false,
                "interestRate": 0,
                "value": 13383,
                "total": 40150
              }
            ]
          }
        ]
      }
    ],
    "paymentSystems": [
      {
        "id": 1,
        "name": "American Express",
        "groupName": "creditCardPaymentGroup",
        "validator": null,
        "stringId": "1",
        "template": "creditCardPaymentGroup-template",
        "requiresDocument": false,
        "isCustom": false,
        "description": null,
        "requiresAuthentication": false
      },
      {
        "id": 2,
        "name": "Visa",
        "groupName": "creditCardPaymentGroup",
        "validator": null,
        "stringId": "2",
        "template": "creditCardPaymentGroup-template",
        "requiresDocument": false,
        "isCustom": false,
        "description": null,
        "requiresAuthentication": false
      }
    ],
    "payments": [],
    "giftCards": [],
    "giftCardMessages": [],
    "availableAccounts": []
  },
  "selectableGifts": [],
  "marketingData": null,
  "postalCode": null,
  "country": null,
  "logisticsInfo": [
    {
      "itemIndex": 0,
      "stockBalance": 2080,
      "quantity": 1,
      "shipsTo": [],
      "slas": []
    },
    {
      "itemIndex": 1,
      "stockBalance": 8780,
      "quantity": 1,
      "shipsTo": [],
      "slas": []
    }
  ],
  "messages": []
}
```

>Exemplo de messages
```json
    "messages": [
    {
      "code": "cannotBeDelivered",
      "text": "O item Produto Único Sku Unico não pode ser entregue para o CEP selecionado",
      "status": "error"
    }
  ]
```

<a name="a2"></a>
###Busca Informações de uma SKU

Acessa o VTEX Marketplace Switch e busca dados de uma SKU - Endpoint do VTEX Marketplace Switch

endpoint: ``` http://[loja].vtexcommercestable.com.br/api/catalog_system/pvt/sku/stockkeepingunitbyid/[idsku] ```</br>
verb: **GET**</br>
Accept: **application/json**</br>
Parametro: **idSku** // identificador do SKU</br>


_response:_

```json
{
    "Id": 1634, //id da sku
    "ProductId": 1634, //id do produto pai da sku
    "NameComplete": "Primer Lisse Minute Clarins - Base Facial Alisadora 15ml", //nome completo
    "ProductName": "Primer Lisse Minute Clarins - Base Facial Alisadora",
    "ProductDescription": "<strong>Primer Lisse Minute</strong> deixa a pele imediatante alisada e suave ao toque. O primer da <strong>Clarins</strong> atenua as imperfeições e reduz a aparência dos poros. A pele fica linda a pronta para receber a make!</br>Com o rosto limpo aplique o primer com os dedos ou com um pincel. Espalhe bem começando pela zona T (testa, nariz e queixo) em direção aos cantos externos do rosto.",
    "SkuName": "15ml",// nome da SKU
    "IsActive": true, // está ativa?
    "IsTransported": true,
    "IsInventoried": true,
    "IsGiftCardRecharge": false,
    "ImageUrl": "http://epocacosmeticos.vteximg.com.br/arquivos/ids/162097-55-55/primer-lisse-minute-15ml-clarins.jpg",
    "DetailUrl": "/primer-lisse-minute-clarins-base-facial-alisadora/p", //link do produto na loja
    "CSCIdentification": null,
    "BrandId": "2000050",//identificador da marca
	"BrandName": "Clarins", //nome da marca
    "Dimension": {//dimensões
        "cubicweight": 0.2083,
        "height": 10,
        "length": 10,
        "weight": 100,
        "width": 10
    },
    "IsKit": false,
    "KitItems": [],
    "Services": [//serviços
        {
            "Id": 1098,
            "Name": "Embalagem presente",
            "IsFile": false,
            "IsGiftCard": false,
            "IsRequired": false,
            "Options": [
                {
                    "Id": 1098,// identificador do serviço
                    "Name": "Embalagem presente",//nome do serviço
                    "Description": "Embalagem presente",
                    "PriceName": "Embalagem presente",
                    "ListPrice": 1, //preço DE do serviço
                    "Price": 1 //preço POR do serviço
                }
            ],
            "Attachments": []
        }
    ],
    "Categories": [],
    "Attachments": [],
    "Collections": [],
    "SkuSellers": [ //lista de sellers que vende essa sku
        {
            "SkuCommercialOffer": {
                "DeliverySlaSamples": [
                    {
                        "Region": {
                            "Id": "1",
                            "Name": "RJ",
                            "CountryCode": "BRA",
                            "ZipCode": "22080010"
                        },
                        "DeliverySlaPerTypes": [
                            {
                                "TypeName": "Normal",
                                "Price": 9.9,
                                "Time": "00:00:00"
                            }
                        ]
                    }
                ],
                "Price": 174,
                "ListPrice": 174,
                "AvailableQuantity": 22
            },
            "SellerId": "1",
            "StockKeepingUnitId": 1634,
            "SellerStockKeepingUnitId": "1634",
            "IsActive": true,
            "FreightCommissionPercentage": 0,
            "ProductCommissionPercentage": 0
        }
    ],
    "SkuPriceSheet": [],
    "Images": [ //imagens, a primeira é a principal
        {
            "ImageUrl": "http://epocacosmeticos.vteximg.com.br/arquivos/ids/162097/primer-lisse-minute-15ml-clarins.jpg",
            "ImageName": null,
            "FileId": 162097
        }
    ],
    "SkuSpecifications": [ //variações
        {
            "FieldId": 283,
            "FieldName": "Variação",
            "FieldValueIds": [ //identificador da variação
                2688
            ],
            "FieldValues": [ //valor da variação
                "15ml"
            ]
        }
    ],
    "ProductSpecifications": [],
    "ProductClustersIds": "135,149,150,151,152,153,154,155,156,157,158,159,160,161,219,310,358,366,406,407,408,451,473,480,486,525,535,546,549,552,561,569",
    "ProductCategoryIds": "/1000004/1000012/1000040/", //arvore de identificadores de categorias
    "ProductCategories": {
        "1000004": "Maquiagem", //identificador e nome da categoria
        "1000012": "Primer e Finalizador",
        "1000040": "Fixador da Maquiagem"
    },
    "CommercialConditionId": 1,
    "RewardValue": 0,
    "AlternateIds": {
        "Ean": "3380814700219",
        "RefId": "005899"
    },
    "AlternateIdValues": [
        "3380814700219",
        "005899"
    ],
    "EstimatedDateArrival": null,
    "MeasurementUnit": "un",
    "UnitMultiplier": 1,
    "InformationSource": "Indexer",
    "ModalType": null
}
```

- - -

###Simulação de Carrinho e Informação de Pagamento

Este tópico tem por objetivo auxiliar o na simulação de carrinho entre um aplicativo e o VTEX Marketplace Switch.

###No Carrinho e no Pagamento

Quando um produto é inserido no carrinho do aplicativo, ou faz se alguma edição no carrinho, uma consulta de simulaçao de carrinho deverá ser feita no VTEX Marketplace Switch para checar a validade das condições comerciais (preço, estoque, frete e SLAs de entrega). Quando o cliente vai para o pagamento, uma outra simulação de carrinho deverá ser realizada.


<a name="a8"></a>
###Simulação de Carrinho

Acessao VTEX Marketplace Switch simulando um carrinho, para checar as condições comerciais e as SLAs de entrega - Endpoint do VTEX Marketplace Switch

endpoint: ``` https://[loja].vtexcommercestable.com.br/api/checkout/pub/orderForms/simulation?sc=[idcanal]&affiliateId=[idafiliado] ```
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **sc** // sc é o canal de vendas</br>
Parametro: **affiliateId** // affiliateId é o id do afiliado cadastrado na loja VTEX</br>

_request:_

```json
{
  "items": [ // lista de itens
    {
      "id": "310118190", //identificado do SKU no Marketplace Switch
      "quantity": 1, // quantidade solicitada
      "seller": "walmartv5" //identificado do Seller no Marketplace Switch
    },
    {
      "id": "310118210",
      "quantity": 1,
      "seller": "parceiro01"
    }
  ],
  "postalCode": "05577000", //opcional, CEP
  "country": "BRA" //opcional, sigla do país
}
```

_response:_

```json
{
  "items": [
    {
      "id": "310118190",
      "requestIndex": 0,
      "quantity": 1,
      "seller": "walmartv5",
      "tax": 0,
      "priceValidUntil": null,
      "price": 39901,
      "listPrice": 52900,
      "sellingPrice": 39901,
      "offerings": [
        {
          "type": "servico",
          "id": "38",
          "name": "servico",
          "allowGiftMessage": false,
          "attachmentOfferings": [
            {
              "name": "Informações",
              "required": false,
              "schema": {
                "texto": {
                  "maximumNumberOfCharacters": 23,
                  "domain": [
                    "teste1",
                    " teste2",
                    " teste3"
                  ]
                }
              }
            }
          ],
          "price": 1000
        }
      ],
      "priceTags": [],
      "measurementUnit": "un",
      "unitMultiplier": 1
    },
    {
      "id": "310118210",
      "requestIndex": 1,
      "quantity": 1,
      "seller": "parceiro01",
      "tax": 0,
      "priceValidUntil": null,
      "price": 249,
      "listPrice": 3000,
      "sellingPrice": 249,
      "offerings": [
        {
          "type": "Embalagem Simples",
          "id": "1",
          "name": "Embalagem Simples",
          "allowGiftMessage": true,
          "attachmentOfferings": [
            {
              "name": "message",
              "required": false,
              "schema": {
                "text": {
                  "maximumNumberOfCharacters": 300,
                  "domain": []
                }
              }
            }
          ],
          "price": 250
        }
      ],
      "priceTags": [],
      "measurementUnit": "un",
      "unitMultiplier": 1
    }
  ],
  "ratesAndBenefitsData": {
    "attachmentId": "ratesAndBenefitsData",
    "rateAndBenefitsIdentifiers": [],
    "teaser": []
  },
  "paymentData": {
    "installmentOptions": [
      {
        "paymentSystem": "1",
        "bin": null,
        "paymentName": "American Express",
        "paymentGroupName": "creditCardPaymentGroup",
        "value": 40150,
        "installments": [
          {
            "count": 1,
            "hasInterestRate": false,
            "interestRate": 0,
            "value": 40150,
            "total": 40150,
            "sellerMerchantInstallments": [
              {
                "id": "SANDBOXINTEGRACAO",
                "count": 1,
                "hasInterestRate": false,
                "interestRate": 0,
                "value": 40150,
                "total": 40150
              }
            ]
          }
        ]
      },
      {
        "paymentSystem": "2",
        "bin": null,
        "paymentName": "Visa",
        "paymentGroupName": "creditCardPaymentGroup",
        "value": 40150,
        "installments": [
          {
            "count": 1,
            "hasInterestRate": false,
            "interestRate": 0,
            "value": 40150,
            "total": 40150,
            "sellerMerchantInstallments": [
              {
                "id": "SANDBOXINTEGRACAO",
                "count": 1,
                "hasInterestRate": false,
                "interestRate": 0,
                "value": 40150,
                "total": 40150
              }
            ]
          },
          {
            "count": 2,
            "hasInterestRate": false,
            "interestRate": 0,
            "value": 20075,
            "total": 40150,
            "sellerMerchantInstallments": [
              {
                "id": "SANDBOXINTEGRACAO",
                "count": 2,
                "hasInterestRate": false,
                "interestRate": 0,
                "value": 20075,
                "total": 40150
              }
            ]
          },
          {
            "count": 3,
            "hasInterestRate": false,
            "interestRate": 0,
            "value": 13383,
            "total": 40150,
            "sellerMerchantInstallments": [
              {
                "id": "SANDBOXINTEGRACAO",
                "count": 3,
                "hasInterestRate": false,
                "interestRate": 0,
                "value": 13383,
                "total": 40150
              }
            ]
          }
        ]
      }
    ],
    "paymentSystems": [
      {
        "id": 1,
        "name": "American Express",
        "groupName": "creditCardPaymentGroup",
        "validator": null,
        "stringId": "1",
        "template": "creditCardPaymentGroup-template",
        "requiresDocument": false,
        "isCustom": false,
        "description": null,
        "requiresAuthentication": false
      },
      {
        "id": 2,
        "name": "Visa",
        "groupName": "creditCardPaymentGroup",
        "validator": null,
        "stringId": "2",
        "template": "creditCardPaymentGroup-template",
        "requiresDocument": false,
        "isCustom": false,
        "description": null,
        "requiresAuthentication": false
      }
    ],
    "payments": [],
    "giftCards": [],
    "giftCardMessages": [],
    "availableAccounts": []
  },
  "selectableGifts": [],
  "marketingData": null,
  "postalCode": "05577000",
  "country": "BRA",
  "logisticsInfo": [
    {
      "itemIndex": 0,
      "stockBalance": 2080,
      "quantity": 1,
      "shipsTo": [],
      "slas": [
        {
          "id": "performance",
          "name": "performance",
          "deliveryIds": [
            {
              "courierId": "1427277",
              "warehouseId": "1_1",
              "dockId": "1_1_1",
              "courierName": "Teste de Performance",
              "quantity": 1
            }
          ],
          "shippingEstimate": "3bd",
          "shippingEstimateDate": null,
          "lockTTL": null,
          "availableDeliveryWindows": [],
          "deliveryWindow": null,
          "price": 286,
          "listPrice": 286,
          "tax": 0
        },
        {
          "id": "Entrega Agendada",
          "name": "Entrega Agendada",
          "deliveryIds": [
            {
              "courierId": "58",
              "warehouseId": "1_1",
              "dockId": "1_1_1",
              "courierName": "Entrega Agendada QA [Não mexer]",
              "quantity": 1
            }
          ],
          "shippingEstimate": "1d",
          "shippingEstimateDate": null,
          "lockTTL": null,
          "availableDeliveryWindows": [
            {
              "startDateUtc": "2015-10-14T08:00:00+00:00",
              "endDateUtc": "2015-10-14T12:00:00+00:00",
              "price": 800,
              "lisPrice": 800,
              "tax": 0
            },
            {
              "startDateUtc": "2015-10-14T13:00:00+00:00",
              "endDateUtc": "2015-10-14T20:00:00+00:00",
              "price": 800,
              "lisPrice": 800,
              "tax": 0
            }
          ],
          "deliveryWindow": null,
          "price": 550,
          "listPrice": 550,
          "tax": 0
        },
        {
          "id": "normal",
          "name": "normal",
          "deliveryIds": [
            {
              "courierId": "1a574b3",
              "warehouseId": "1_1",
              "dockId": "1_1_1",
              "courierName": "Mundo",
              "quantity": 1
            }
          ],
          "shippingEstimate": "16bd",
          "shippingEstimateDate": null,
          "lockTTL": null,
          "availableDeliveryWindows": [],
          "deliveryWindow": null,
          "price": 1370,
          "listPrice": 1370,
          "tax": 0
        },
        {
          "id": "PAC",
          "name": "PAC",
          "deliveryIds": [
            {
              "courierId": "56",
              "warehouseId": "1_1",
              "dockId": "1_1_1",
              "courierName": "PAC",
              "quantity": 1
            }
          ],
          "shippingEstimate": "8bd",
          "shippingEstimateDate": null,
          "lockTTL": null,
          "availableDeliveryWindows": [],
          "deliveryWindow": null,
          "price": 1540,
          "listPrice": 1540,
          "tax": 0
        },
        {
          "id": "Expressa",
          "name": "Expressa",
          "deliveryIds": [
            {
              "courierId": "55",
              "warehouseId": "1_1",
              "dockId": "1_1_1",
              "courierName": "Sedex",
              "quantity": 1
            }
          ],
          "shippingEstimate": "7bd",
          "shippingEstimateDate": null,
          "lockTTL": null,
          "availableDeliveryWindows": [],
          "deliveryWindow": null,
          "price": 1650,
          "listPrice": 1650,
          "tax": 0
        }
      ]
    },
    {
      "itemIndex": 1,
      "stockBalance": 8780,
      "quantity": 1,
      "shipsTo": [],
      "slas": [
        {
          "id": "PAC",
          "name": "PAC",
          "deliveryIds": [
            {
              "courierId": "19c9acd",
              "warehouseId": "1_1",
              "dockId": "16f4d65",
              "courierName": "[TESTE QA] Não mexer",
              "quantity": 1
            }
          ],
          "shippingEstimate": "3bd",
          "shippingEstimateDate": null,
          "lockTTL": null,
          "availableDeliveryWindows": [],
          "deliveryWindow": null,
          "price": 0,
          "listPrice": 0,
          "tax": 0
        }
      ]
    }
  ],
  "messages": []
}
```

>Caso o CEP e Pais não for enviado, não será retornado informações de SLA de entrega. slas[] vazio.


###Enviar Pedido e Informar Pagamento

Este tópico tem por objetivo auxiliar um aplicativo enviar um pedido, iformar um pagamento e fechar o processo de pedido no VTEX Marketplace Switch.

<a name="a4"></a>
###Enviar Pedido

Quando o pedido é fechado em um aplicativo, um PUT deve ser feito no VTEX Marketplace Switch, para que essa possa receber a ordem de pedido - Endpoint do VTEX Marketplace Switch

endpoint: ``` https://[loja].vtexcommercestable.com.br/api/checkout/pub/orders?sc=1&affiliateId=LBB ```</br>
verb: **PUT**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **sc** // sc é o canal de vendas cadastrado na VTEX.</br>
Parametro: **affiliateId** // affiliateId é o id do afiliado cadastrado no VTEX Marketplace Switch</br>

_request:_

```json
{
  "items": [
    {
      "id": "310118190",
      "quantity": 1,
      "seller": "walmartv5",
      "price": 39901,
      "bundleItems": []
    },
    {
      "id": "310118210",
      "quantity": 1,
      "seller": "parceiro01",
      "price": 249,
      "bundleItems": []
    }
  ],
  "clientProfileData": {
    "email": "jonasrj@hotmail.com.br",
    "firstName": "Jonas",
    "lastName": "Bolognim",
    "document": "13402886561",
    "phone": "999981477",
    "corporateName": null,
    "tradeName": null,
    "corporateDocument": null,
    "stateInscription": null,
    "corporatePhone": null,
    "isCorporate": false
  },
  "shippingData": {
    "id": "shippingData",
    "address": {
      "addressType": "1",
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
      "reference": "Bairro Praia Azul / Posto de Saúde 17"
    },
    "logisticsInfo": [
      {
        "itemIndex": 0,
        "selectedSla": "normal",
        "lockTTL": "16bd",
        "price": 1370,
        "deliveryWindow": null
      },
      {
        "itemIndex": 0,
        "selectedSla": "PAC",
        "lockTTL": "3bd",
        "price": 0,
        "deliveryWindow": null
      }
    ]
  },
  "paymentData": {
    "payments": [
      {
        "value": 41520,
        "referenceValue": 41520,
        "installments": 1,
        "paymentSystem": "201"
      }
    ]
  },
  "gift-cards": []
}
```

_response:_

```json
{
  "orderForm": null,
  "transactionData": {
    "merchantTransactions": [
      {
        "id": "SANDBOXINTEGRACAO",
        "transactionId": "AA11D2427D57456B8B2499532C32F893",
        "merchantName": "SANDBOXINTEGRACAO",
        "payments": [
          {
            "paymentSystem": "201",
            "bin": null,
            "accountId": null,
            "value": 41520,
            "referenceValue": 41520,
            "giftCardRedemptionCode": null,
            "giftCardProvider": null,
            "giftCardId": null
          }
        ]
      }
    ],
    "receiverUri": "https://sandboxintegracao.vtexpayments.com.br/split/568600722512/payments",
    "gatewayCallbackTemplatePath": "/checkout/gatewayCallback/568600722512/{messageCode}"
  },
  "orders": [
    {
      "orderId": "568600722512-01",
      "orderGroup": "568600722512",
      "state": "payment-pending",
      "value": 41271,
      "items": [
        {
          "uniqueId": "11A9F2B976A243CFAD9C99D7FBE44E78",
          "id": "310118190",
          "productId": "310116836",
          "refId": "0896e250-df2c-4dab-a6de-bf8c5e248f7b",
          "name": "Irrigador Oral e Fio Dental de Água Waterpik Ultra 110 volts - Excepcional Para Limpeza Profunda em Pontes, Coroas, Aparelhos Ortodônticos e Implantes Irrigador Dental Waterpik Ultra 100W 110v",
          "skuName": "Irrigador Dental Waterpik Ultra 100W 110v",
          "modalType": null,
          "priceValidUntil": null,
          "tax": 0,
          "price": 39901,
          "listPrice": 52900,
          "manualPrice": null,
          "sellingPrice": 39901,
          "rewardValue": 0,
          "isGift": false,
          "additionalInfo": {
            "brandName": "Marca",
            "brandId": "2000012",
            "offeringInfo": null,
            "offeringType": null,
            "offeringTypeId": null
          },
          "preSaleDate": null,
          "productCategoryIds": "/1000010/",
          "productCategories": {
            "1000010": "Higiene e Beleza"
          },
          "defaultPicker": null,
          "handlerSequence": 0,
          "handling": false,
          "quantity": 1,
          "seller": "walmartv5",
          "imageUrl": "http://sandboxintegracao.vteximg.com.br/arquivos/ids/156755-55-55/Irrigador-Dental-Waterpik-Ultra-100W-110v_0.jpg",
          "detailUrl": "/irrigador-oral-e-fio-dental-de-agua-waterpik-ultra-110-volts---excepcional-para-limpeza-profunda-em-pontes-coroas-aparelhos-ortodonticos-e-implantes-310116836/p",
          "components": [],
          "bundleItems": [],
          "attachments": [],
          "itemAttachment": {
            "name": null,
            "content": {}
          },
          "attachmentOfferings": [],
          "offerings": [
            {
              "type": "servico",
              "id": "38",
              "name": "servico",
              "allowGiftMessage": false,
              "attachmentOfferings": [
                {
                  "name": "Informações",
                  "required": false,
                  "schema": {
                    "texto": {
                      "maximumNumberOfCharacters": 23,
                      "domain": [
                        "teste1",
                        " teste2",
                        " teste3"
                      ]
                    }
                  }
                }
              ],
              "price": 1000
            }
          ],
          "priceTags": [],
          "availability": "available",
          "measurementUnit": "un",
          "unitMultiplier": 1
        }
      ],
      "sellers": [
        {
          "id": "walmartv5",
          "name": "VTEX MKP - QA",
          "logo": "http://portal.vtexcommercestable.com.br/arquivos/logo.jpg"
        }
      ],
      "totals": [
        {
          "id": "Items",
          "name": "Total dos Itens",
          "value": 39901
        },
        {
          "id": "Discounts",
          "name": "Total dos Descontos",
          "value": 0
        },
        {
          "id": "Shipping",
          "name": "Total do Frete",
          "value": 1370
        },
        {
          "id": "Tax",
          "name": "Total da Taxa",
          "value": 0
        }
      ],
      "clientProfileData": {
        "attachmentId": "clientProfileData",
        "email": "jonasrj@hotmail.com.br",
        "firstName": "Jonas",
        "lastName": "Bolognim",
        "document": "13402886561",
        "documentType": "cpf",
        "phone": "999981477",
        "corporateName": null,
        "tradeName": null,
        "corporateDocument": null,
        "stateInscription": null,
        "corporatePhone": null,
        "isCorporate": false
      },
      "ratesAndBenefitsData": {
        "attachmentId": "ratesAndBenefitsData",
        "rateAndBenefitsIdentifiers": [],
        "teaser": []
      },
      "shippingData": {
        "attachmentId": "shippingData",
        "address": {
          "addressType": "residential",
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
            "selectedSla": "normal",
            "slas": [
              {
                "id": "performance",
                "name": "performance",
                "deliveryIds": [
                  {
                    "courierId": "1427277",
                    "warehouseId": "1_1",
                    "dockId": "1_1_1",
                    "courierName": "Teste de Performance",
                    "quantity": 1
                  }
                ],
                "shippingEstimate": "3bd",
                "shippingEstimateDate": null,
                "lockTTL": "15d",
                "availableDeliveryWindows": [],
                "deliveryWindow": null,
                "price": 286,
                "listPrice": 286,
                "tax": 0
              },
              {
                "id": "Entrega Agendada",
                "name": "Entrega Agendada",
                "deliveryIds": [
                  {
                    "courierId": "58",
                    "warehouseId": "1_1",
                    "dockId": "1_1_1",
                    "courierName": "Entrega Agendada QA [Não mexer]",
                    "quantity": 1
                  }
                ],
                "shippingEstimate": "1d",
                "shippingEstimateDate": null,
                "lockTTL": "15d",
                "availableDeliveryWindows": [
                  {
                    "startDateUtc": "2015-10-14T08:00:00+00:00",
                    "endDateUtc": "2015-10-14T12:00:00+00:00",
                    "price": 800,
                    "lisPrice": 800,
                    "tax": 0
                  },
                  {
                    "startDateUtc": "2015-10-14T13:00:00+00:00",
                    "endDateUtc": "2015-10-14T20:00:00+00:00",
                    "price": 800,
                    "lisPrice": 800,
                    "tax": 0
                  }
                ],
                "deliveryWindow": null,
                "price": 550,
                "listPrice": 550,
                "tax": 0
              },
              {
                "id": "normal",
                "name": "normal",
                "deliveryIds": [
                  {
                    "courierId": "1a574b3",
                    "warehouseId": "1_1",
                    "dockId": "1_1_1",
                    "courierName": "Mundo",
                    "quantity": 1
                  }
                ],
                "shippingEstimate": "16bd",
                "shippingEstimateDate": null,
                "lockTTL": "15d",
                "availableDeliveryWindows": [],
                "deliveryWindow": null,
                "price": 1370,
                "listPrice": 1370,
                "tax": 0
              },
              {
                "id": "PAC",
                "name": "PAC",
                "deliveryIds": [
                  {
                    "courierId": "56",
                    "warehouseId": "1_1",
                    "dockId": "1_1_1",
                    "courierName": "PAC",
                    "quantity": 1
                  }
                ],
                "shippingEstimate": "8bd",
                "shippingEstimateDate": null,
                "lockTTL": "15d",
                "availableDeliveryWindows": [],
                "deliveryWindow": null,
                "price": 1540,
                "listPrice": 1540,
                "tax": 0
              },
              {
                "id": "Expressa",
                "name": "Expressa",
                "deliveryIds": [
                  {
                    "courierId": "55",
                    "warehouseId": "1_1",
                    "dockId": "1_1_1",
                    "courierName": "Sedex",
                    "quantity": 1
                  }
                ],
                "shippingEstimate": "7bd",
                "shippingEstimateDate": null,
                "lockTTL": "15d",
                "availableDeliveryWindows": [],
                "deliveryWindow": null,
                "price": 1650,
                "listPrice": 1650,
                "tax": 0
              }
            ],
            "shipsTo": [],
            "itemId": "310118190"
          }
        ],
        "availableAddresses": [
          {
            "addressType": "residential",
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
          }
        ]
      },
      "paymentData": {
        "attachmentId": "paymentData",
        "transactionId": "AA11D2427D57456B8B2499532C32F893",
        "payments": [],
        "giftCards": [],
        "transactions": [
          {
            "isActive": true,
            "transactionId": "AA11D2427D57456B8B2499532C32F893",
            "merchantName": "SANDBOXINTEGRACAO",
            "payments": []
          }
        ],
        "merchantName": "SANDBOXINTEGRACAO"
      },
      "clientPreferencesData": null,
      "giftRegistryData": null,
      "marketingData": null,
      "storePreferencesData": {
        "countryCode": "BRA",
        "checkToSavePersonDataByDefault": false,
        "templateOptions": {
          "toggleCorporate": false
        },
        "timeZone": "E. South America Standard Time",
        "currencyCode": "BRL",
        "currencyLocale": 1046,
        "currencySymbol": "R$",
        "currencyFormatInfo": {
          "currencyDecimalDigits": 2,
          "currencyDecimalSeparator": ",",
          "currencyGroupSeparator": ".",
          "currencyGroupSize": 3,
          "startsWithCurrencySymbol": true
        }
      },
      "openTextField": null,
      "changeData": null,
      "salesChannel": "1",
      "followUpEmail": "83e33f638b5d4d3f9a00bb3062801487@ct.vtex.com.br",
      "creationDate": "2015-10-13T20:12:03.6383629Z",
      "lastChange": "2015-10-13T20:12:12.9046797Z",
      "timeZoneCreationDate": "2015-10-13T17:12:03.6383629",
      "timeZoneLastChange": "2015-10-13T17:12:12.9046797",
      "isCompleted": false,
      "hostName": "sandboxintegracao",
      "merchantName": null,
      "userType": "",
      "roundingError": 0
    },
    {
      "orderId": "568600722512-02",
      "orderGroup": "568600722512",
      "state": "payment-pending",
      "value": 249,
      "items": [
        {
          "uniqueId": "EE5DB23686494286A2FFCDD689051804",
          "id": "310118210",
          "productId": "310116844",
          "refId": "Cod-Ref-Item-ModeloC",
          "name": "Produto Exemplo 2 Exemplo 2",
          "skuName": "Exemplo 2",
          "modalType": null,
          "priceValidUntil": null,
          "tax": 0,
          "price": 249,
          "listPrice": 3000,
          "manualPrice": null,
          "sellingPrice": 249,
          "rewardValue": 0,
          "isGift": false,
          "additionalInfo": {
            "brandName": "Marca Exemplo",
            "brandId": "1",
            "offeringInfo": null,
            "offeringType": null,
            "offeringTypeId": null
          },
          "preSaleDate": null,
          "productCategoryIds": "/3/",
          "productCategories": {
            "3": "Moda"
          },
          "defaultPicker": null,
          "handlerSequence": 0,
          "handling": false,
          "quantity": 1,
          "seller": "parceiro01",
          "imageUrl": "http://sandboxintegracao.vteximg.com.br/arquivos/ids/156894-55-55/Exemplo-2_0.jpg",
          "detailUrl": "/produto-exemplo-2-310116844/p",
          "components": [],
          "bundleItems": [],
          "attachments": [],
          "itemAttachment": {
            "name": null,
            "content": {}
          },
          "attachmentOfferings": [],
          "offerings": [
            {
              "type": "Embalagem Simples",
              "id": "1",
              "name": "Embalagem Simples",
              "allowGiftMessage": true,
              "attachmentOfferings": [
                {
                  "name": "message",
                  "required": false,
                  "schema": {
                    "text": {
                      "maximumNumberOfCharacters": 300,
                      "domain": []
                    }
                  }
                }
              ],
              "price": 250
            }
          ],
          "priceTags": [],
          "availability": "available",
          "measurementUnit": "un",
          "unitMultiplier": 1
        }
      ],
      "sellers": [
        {
          "id": "parceiro01",
          "name": "Parceiro01",
          "logo": "http://portal.vtexcommercestable.com.br/arquivos/logo.jpg"
        }
      ],
      "totals": [
        {
          "id": "Items",
          "name": "Total dos Itens",
          "value": 249
        },
        {
          "id": "Discounts",
          "name": "Total dos Descontos",
          "value": 0
        },
        {
          "id": "Shipping",
          "name": "Total do Frete",
          "value": 0
        },
        {
          "id": "Tax",
          "name": "Total da Taxa",
          "value": 0
        }
      ],
      "clientProfileData": {
        "attachmentId": "clientProfileData",
        "email": "jonasrj@hotmail.com.br",
        "firstName": "Jonas",
        "lastName": "Bolognim",
        "document": "13402886561",
        "documentType": "cpf",
        "phone": "999981477",
        "corporateName": null,
        "tradeName": null,
        "corporateDocument": null,
        "stateInscription": null,
        "corporatePhone": null,
        "isCorporate": false
      },
      "ratesAndBenefitsData": {
        "attachmentId": "ratesAndBenefitsData",
        "rateAndBenefitsIdentifiers": [],
        "teaser": []
      },
      "shippingData": {
        "attachmentId": "shippingData",
        "address": {
          "addressType": "residential",
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
            "selectedSla": "PAC",
            "slas": [
              {
                "id": "PAC",
                "name": "PAC",
                "deliveryIds": [
                  {
                    "courierId": "19c9acd",
                    "warehouseId": "1_1",
                    "dockId": "16f4d65",
                    "courierName": "[TESTE QA] Não mexer",
                    "quantity": 1
                  }
                ],
                "shippingEstimate": "3bd",
                "shippingEstimateDate": null,
                "lockTTL": "23d",
                "availableDeliveryWindows": [],
                "deliveryWindow": null,
                "price": 0,
                "listPrice": 0,
                "tax": 0
              }
            ],
            "shipsTo": [],
            "itemId": "310118210"
          }
        ],
        "availableAddresses": [
          {
            "addressType": "residential",
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
          }
        ]
      },
      "paymentData": {
        "attachmentId": "paymentData",
        "transactionId": "AA11D2427D57456B8B2499532C32F893",
        "payments": [],
        "giftCards": [],
        "transactions": [
          {
            "isActive": true,
            "transactionId": "AA11D2427D57456B8B2499532C32F893",
            "merchantName": "SANDBOXINTEGRACAO",
            "payments": []
          }
        ],
        "merchantName": "SANDBOXINTEGRACAO"
      },
      "clientPreferencesData": null,
      "giftRegistryData": null,
      "marketingData": null,
      "storePreferencesData": {
        "countryCode": "BRA",
        "checkToSavePersonDataByDefault": false,
        "templateOptions": {
          "toggleCorporate": false
        },
        "timeZone": "E. South America Standard Time",
        "currencyCode": "BRL",
        "currencyLocale": 1046,
        "currencySymbol": "R$",
        "currencyFormatInfo": {
          "currencyDecimalDigits": 2,
          "currencyDecimalSeparator": ",",
          "currencyGroupSeparator": ".",
          "currencyGroupSize": 3,
          "startsWithCurrencySymbol": true
        }
      },
      "openTextField": null,
      "changeData": null,
      "salesChannel": "1",
      "followUpEmail": "e5c3e9ac0364456bbdd3ea8dea14db0b@ct.vtex.com.br",
      "creationDate": "2015-10-13T20:12:07.7099271Z",
      "lastChange": "2015-10-13T20:12:12.9202808Z",
      "timeZoneCreationDate": "2015-10-13T17:12:07.7099271",
      "timeZoneLastChange": "2015-10-13T17:12:12.9202808",
      "isCompleted": false,
      "hostName": "sandboxintegracao",
      "merchantName": null,
      "userType": "",
      "roundingError": 0
    }
  ]
}
```

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

<a name="a5"></a>
###Informa Pagamento

Informa o pagamento do pedido - Endpoint do VTEX Marketplace Switch. Nesse modelo um pagamento customizado dever
ser enviado, para que se complete o fluxo do pedido.

endpoint: ``` https://[loja].vtexpayments.com.br/split/[iddogrupodopedido]/payments ```</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **iddogrupodopedido** //identificador retornado no insert do pedido</br>

_request:_

```json
{
  "callbackUrl": "", //opciona, enviar vazio
  "paymentsArray": "[{\"paymentSystem\": 201, //identificador do meio de pagamento
                      \"paymentSystemName\": \"FastShop\", //nome do meio de pagamento
                      \"groupName\": \"\",
                      \"currencyCode\":\"BRL\", //moeda
                      \"installments\": 1, //numero de parcelas
                      \"value\": 41520, //valor total do pagamento
                      \"installmentsInterestRate\": 0, //taxa de juros
                      \"installmentsValue\": 41520, //valor da parcela
                      \"referenceValue\": 41520, //valer sem juros
                      \"fields\": //quando cartão
                                    { \"document\": \"\", //documento do cartão
                                    \"accountId\": \"\", //identificador do profile do cartão
                                    \"addressId\": \"\", //identificador do endereço de cobrança
                                    \"cardNumber\":\"\", //numero do cartao
                                    \"holderName\": \"\", //nome do cartão
                                    \"dueDate\": \"\", //data de validade do cartão
                                    \"validationCode\": \"\" }, //código de segurança do cartão
                      \"transaction\": { \"id\": \"AA11D2427D57456B8B2499532C32F893\", //identificador da transação retornado no insert do pedido
                                         \"merchantName\": \"SANDBOXINTEGRACAO\", //merchant name retornado no insert do pedido
                                         \"payments\": null}}]"
}
```


_exemplo sem comentários:_
```json
{
  "callbackUrl": "",
  "paymentsArray": "[{\"paymentSystem\": 201, \"paymentSystemName\": \"FastShop\", \"groupName\": \"\",\"currencyCode\":\"\",\"installments\": 1, \"value\": 41520, \"installmentsInterestRate\": 0, \"installmentsValue\": 41520, \"referenceValue\": 41520, \"fields\": { \"document\": \"\", \"accountId\": \"\",\"addressId\": \"\",\"cardNumber\":\"\", \"holderName\": \"\", \"dueDate\": \"\", \"validationCode\": \"\" },\"transaction\": { \"id\": \"AA11D2427D57456B8B2499532C32F893\", \"merchantName\": \"SANDBOXINTEGRACAO\", \"payments\": null}}]"
}
```

_response:_

```json
200
```

<a name="a6"></a>
###Fechamento do Fluxo de Descida de Pedido

Fecha o fluxo de fechamento de pedido - Endpoint do VTEX Marketplace Switch. Esta chamada serve para fechar o fluxo de fechamento do pedido no VTEX Marketplace Switch. O retorno de sucesso desse metodo é um status code 302.

endpoint: ``` https://sandboxintegracao.vtexcommercestable.com.br/checkout/gatewayCallback/[iddogrupodopedido]/Success ```</br>
verb: **GET**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **iddogrupodopedido** //identificador retornado no insert do pedido</br>

_response:_

```json
302
```

<a name="a7"></a>
###Recuperar e Aprovar o Pagamento

Recupera o identificador do pagamento e informa o sucesso no pagamento do pedido - Endpoint do VTEX Marketplace Switch

Recupera dados de pagamento:</br>

endpoint: ``` https://[loja].vtexpayments.com.br/api/pvt/transactions/[idtransacao]/payments ```</br>
verb: **GET**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **idtransacao** //identificador do transação do pagamento</br>

_response:_

```json
[
  {
    "id": "929FDCB1C9FB4111875F2863A47FF8D2", //identificador do pagamento, usado para manipular o pagamento
    "paymentSystem": 201,
    "paymentSystemName": "FastShop Money F$",
    "group": "promissory",
    "isCustom": true,
    "allowInstallments": false,
    "allowIssuer": false,
    "allowNotification": true,
    "isAvailable": true,
    "description": null,
    "self": {
      "href": "/api/pvt/transactions/CE0C550E892044D3BDDA435984D73860/payments/929FDCB1C9FB4111875F2863A47FF8D2"
    },
    "tid": null,
    "returnCode": null,
    "returnMessage": null,
    "status": "Settling",
    "connector": "Promissory",
    "ConnectorResponses": null,
    "connectorResponse": null,
    "ShowConnectorResponses": false,
    "value": 90500,
    "installmentsInterestRate": 0,
    "installmentsValue": 90500,
    "referenceValue": 90500,
    "installments": 1,
    "currencyCode": "",
    "provider": null,
    "fields": [
      {
        "name": "affiliationId",
        "value": "65216163-55ff-4224-982e-01fe6bcf8184"
      },
      {
        "name": "callbackUrl",
        "value": ""
      },
      {
        "name": "baseUrl",
        "value": "https://FASTSHOP.vtexpayments.com.br:443"
      },
      {
        "name": "currencyCode",
        "value": ""
      },
      {
        "name": "connector",
        "value": "Vtex.PaymentGateway.Connectors.PromissoryConnector"
      },
      {
        "name": "sequence",
        "value": "500001-0"
      },
      {
        "name": "notified",
        "value": "True"
      },
      {
        "name": "waitingForAutoSettle",
        "value": "06/10/2015 16:16:32 P UTC"
      },
      {
        "name": "settlement",
        "value": "[{\"Id\":\"8c53c854-1b64-430e-a9fb-cfcda8987f78\",\"Token\":\"06FF3567FEA14E8582BC6DF39026EB12\",\"SettlementDate\":\"2015-10-06T16:16:35.8276913Z\",\"ValueAsInt\":90500,\"Value\":905.0,\"SettlementType\":1}]"
      }
    ],
    "sheets": null
  }
]
```

Aprovando um pagamento:</br>

endpoint: ``` https://[loja].vtexpayments.com.br/api/pvt/payments/[idpagamento]/payment-notification ```</br>
verb: **GET**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **idpagamento** //identificador do pagamento</br>

_response:_

```json
200
```

Cancelando uma transação de pagamento:</br>

endpoint: ``` https://[loja].vtexpayments.com.br/api/pvt/transactions/[idtransacao]/cancellation-request ```</br>
verb: **POST**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **idtransacao** //identificador do transação do pagamento</br>

_response:_

```json
 {
    value: 90500 //valor do pagamento
 }
```

_response:_

```json
200
```

<a name="a9"></a>
###Ouvir o Feed de Status de Pedido

Ouvir o feed de status de pedido - Endpoint do VTEX Marketplace Switch. O aplicativo deve se increver no feed de status de pedidos, ouvir e tratar o status que lhe interessa e confirmar. Os status que vai ignorar devem somente ser confirmados.

http://lab.vtex.com/docs/oms/api/latest/feed/index.html

> 1) Quanto tempo o registro fica na fila do feed antes de ser excluído automaticamente?
>> R.: Uma semana

> 2) Por que mesmo a fila estando "cheia", hora vem 5 registros, hora vem 10 registros?
>> R.: Os dados são particionados. Por isso pode acontecer isso (é normal).

> 3) Consigo aumentar a quantidade de registros que são entregues a cada requisição?
>> R.: Pode se usar o parâmetro maxLot (ex: maxLot=3) e o maximo é 10.

> 4) Existe alguma change de um status deixar se ser enviado na fila?
>> R.: Não. A princípio só um  desastre (nos Data Centers) - nada é 100% seguro.
       Obs.: Após o início do consumo dos itens do feed, não é aconselhado parar de consumir-los por mais de 4 dias.

> 5) Se 2 sistemas difererentes desejam consumir o feed para objetivos diferentes, é necessário criar uma outra app-key.
>> R.: Sim. Se usar o mesmo usuário vai concorrer e consumir a mesma fila.

> 6) o que faço com os status que não me interessa?
>> R.: Eles devem ser descartados confirmado os mesmo na fila.

<a name="a10"></a>
###Buscar Detalhe de Pedido

Uma outra opção para acompanhar e buscar os dados de pedido, como nota fiscal e trackiing, é buscar pontualmente o grupo de pedidos e respectivamente os detalhes dos pedidos.

Busca grupo de pedidos:

endpoint: ``` https://[loja].vtexpayments.com.br/api/oms/pvt/orders?orderGroup=[idgrupo] ```</br>
verb: **GET**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **idgrupo** //identificador do grupo retornado no insert do pedido</br>

_response:_

```json
{
  "list": [ //lista de pedidos do grupo
    {
      "orderId": "566880726117-02",
      "creationDate": "2015-10-06T16:12:19.0000000+00:00",
      "clientName": "Jonas Bolognim",
      "items": [
        {
          "seller": "parceiro01",
          "quantity": 1,
          "description": "Produto Exemplo 1 Modelo B",
          "ean": "1234567",
          "refId": "Cod-Ref-Item-ModeloB"
        }
      ],
      "totalValue": 10000,
      "paymentNames": "FastShop Money F$",
      "status": "payment-approved",
      "statusDescription": "Pagamento Aprovado",
      "marketPlaceOrderId": null,
      "sequence": "500003",
      "salesChannel": "1",
      "affiliateId": "",
      "origin": "Marketplace",
      "workflowInErrorState": false,
      "workflowInRetry": false,
      "lastMessageUnread": "fastshop Seu pagamento foi aprovado! pedido realizado em: 06/10/2015 Olá, Jonas. Seu pagamento foi efetuado com sucesso. Você receberá outro",
      "ShippingEstimatedDate": "2015-10-09T16:16:50.0000000+00:00",
      "orderIsComplete": true,
      "listId": null,
      "listType": null
    },
    {
      "orderId": "566880726117-01",
      "creationDate": "2015-10-06T16:12:14.0000000+00:00",
      "clientName": "Jonas Bolognim",
      "items": [
        {
          "seller": "sandboxintegracao",
          "quantity": 1,
          "description": "TV LED 24\" Full HD 2 HDMI Conversor Digital LE24D1440 AOC 110",
          "ean": "0123456789127",
          "refId": "AJHDAOdh"
        }
      ],
      "totalValue": 80500,
      "paymentNames": "FastShop Money F$",
      "status": "invoiced",
      "statusDescription": "Faturado",
      "marketPlaceOrderId": null,
      "sequence": "500001",
      "salesChannel": "1",
      "affiliateId": "",
      "origin": "Marketplace",
      "workflowInErrorState": false,
      "workflowInRetry": false,
      "lastMessageUnread": "fastshop Seu pedido foi faturado! realizado em: 06/10/2015 Olá, Jonas. Os produtos abaixo já foram faturados só aguardando envio para a tran",
      "ShippingEstimatedDate": null,
      "orderIsComplete": true,
      "listId": null,
      "listType": null
    }
  ],
  "facets": [
    {
      "name": "SalesChannelName",
      "type": "multiple",
      "items": [
        {
          "name": "Loja Principal",
          "quantity": 2,
          "url": "Loja Principal"
        }
      ],
      "rangeUrlTemplate": "f_salesChannel"
    },
    {
      "name": "SellerNames",
      "type": "multiple",
      "items": [
        {
          "name": "Parceiro01",
          "quantity": 1,
          "url": "Parceiro01"
        },
        {
          "name": "Sandbox-integracao",
          "quantity": 1,
          "url": "Sandbox-integracao"
        }
      ],
      "rangeUrlTemplate": "f_sellerNames"
    },
    {
      "name": "PaymentNames",
      "type": "multiple",
      "items": [
        {
          "name": "FastShop Money F$",
          "quantity": 2,
          "url": "FastShop Money F$"
        }
      ],
      "rangeUrlTemplate": "f_paymentNames"
    },
    {
      "name": "StatusDescription",
      "type": "multiple",
      "items": [
        {
          "name": "Faturado",
          "quantity": 1,
          "url": "Faturado"
        },
        {
          "name": "Pagamento Aprovado",
          "quantity": 1,
          "url": "Pagamento Aprovado"
        }
      ],
      "rangeUrlTemplate": "f_statusDescription"
    },
    {
      "name": "ShippingEstimatedDate",
      "type": "single",
      "items": [
        {
          "name": "Atrasadas",
          "quantity": 0,
          "url": "-1.days"
        },
        {
          "name": "Hoje",
          "quantity": 0,
          "url": "0.days"
        },
        {
          "name": "Amanhã",
          "quantity": 1,
          "url": "1.days"
        },
        {
          "name": "Próximos 7 dias",
          "quantity": 1,
          "url": "7.days"
        }
      ],
      "rangeUrlTemplate": "f_shippingEstimate"
    },
    {
      "name": "invoicedDate",
      "type": "date",
      "items": [
        {
          "name": "Ontem",
          "quantity": 1,
          "url": "invoicedDate:[2015-10-07T12:00:00Z TO 2015-10-08T12:00:00Z]"
        },
        {
          "name": "Hoje",
          "quantity": 0,
          "url": "invoicedDate:[2015-10-08T12:00:00Z TO 2015-10-09T12:00:00Z]"
        },
        {
          "name": "Últimos 7 dias",
          "quantity": 1,
          "url": "invoicedDate:[2015-10-02T12:00:00Z TO 2015-10-09T12:00:00Z]"
        },
        {
          "name": "Últimos 15 dias",
          "quantity": 1,
          "url": "invoicedDate:[2015-09-24T12:00:00Z TO 2015-10-09T12:00:00Z]"
        },
        {
          "name": "Últimos 30 dias",
          "quantity": 1,
          "url": "invoicedDate:[2015-09-09T12:00:00Z TO 2015-10-09T12:00:00Z]"
        }
      ],
      "rangeUrlTemplate": "f_invoicedDate"
    },
    {
      "name": "creationDate",
      "type": "date",
      "items": [
        {
          "name": "Ontem",
          "quantity": 0,
          "url": "creationDate:[2015-10-07T12:00:00Z TO 2015-10-08T12:00:00Z]"
        },
        {
          "name": "Hoje",
          "quantity": 0,
          "url": "creationDate:[2015-10-08T12:00:00Z TO 2015-10-09T12:00:00Z]"
        },
        {
          "name": "Últimos 7 dias",
          "quantity": 2,
          "url": "creationDate:[2015-10-02T12:00:00Z TO 2015-10-09T12:00:00Z]"
        },
        {
          "name": "Últimos 15 dias",
          "quantity": 2,
          "url": "creationDate:[2015-09-24T12:00:00Z TO 2015-10-09T12:00:00Z]"
        },
        {
          "name": "Últimos 30 dias",
          "quantity": 2,
          "url": "creationDate:[2015-09-09T12:00:00Z TO 2015-10-09T12:00:00Z]"
        }
      ],
      "rangeUrlTemplate": "f_creationDate"
    },
    {
      "name": "authorizedDate",
      "type": "date",
      "items": [
        {
          "name": "Ontem",
          "quantity": 0,
          "url": "authorizedDate:[2015-10-07T12:00:00Z TO 2015-10-08T12:00:00Z]"
        },
        {
          "name": "Hoje",
          "quantity": 0,
          "url": "authorizedDate:[2015-10-08T12:00:00Z TO 2015-10-09T12:00:00Z]"
        },
        {
          "name": "Últimos 7 dias",
          "quantity": 2,
          "url": "authorizedDate:[2015-10-02T12:00:00Z TO 2015-10-09T12:00:00Z]"
        },
        {
          "name": "Últimos 15 dias",
          "quantity": 2,
          "url": "authorizedDate:[2015-09-24T12:00:00Z TO 2015-10-09T12:00:00Z]"
        },
        {
          "name": "Últimos 30 dias",
          "quantity": 2,
          "url": "authorizedDate:[2015-09-09T12:00:00Z TO 2015-10-09T12:00:00Z]"
        }
      ],
      "rangeUrlTemplate": "f_authorizedDate"
    }
  ],
  "paging": {
    "total": 2,
    "pages": 1,
    "currentPage": 1,
    "perPage": 15
  },
  "stats": {
    "stats": {
      "totalItems": {
        "Count": 2,
        "Max": 1,
        "Mean": 1,
        "Min": 1,
        "Missing": 0,
        "StdDev": 0,
        "Sum": 2,
        "SumOfSquares": 2
      },
      "totalValue": {
        "Count": 2,
        "Max": 80500,
        "Mean": 45250,
        "Min": 10000,
        "Missing": 0,
        "StdDev": 49851.0280736516,
        "Sum": 90500,
        "SumOfSquares": 6580250000
      }
    }
  }
}
```
>>Neste retorno o que realmente interessa é a lista de pedidos, ignorar o resto.

Busca detalhe de um pedido:

endpoint: ``` https://[loja].vtexpayments.com.br/api/oms/pvt/orders/[orderId] ```</br>
verb: **GET**</br>
Content-Type: **application/json**</br>
Accept: **application/json**</br>
Parametro: **orderId** //identificador do pedido</br>

_response:_

```json
{
  "orderId": "566880726117-01",
  "sequence": "500001",
  "marketplaceOrderId": "",
  "marketplaceServicesEndpoint": null,
  "sellerOrderId": "FSH-566880726117-01",
  "origin": "Marketplace",
  "affiliateId": "",
  "salesChannel": "1",
  "merchantName": null,
  "status": "invoiced",
  "statusDescription": "Faturado",
  "value": 80500,
  "creationDate": "2015-10-06T16:12:14.8938894+00:00",
  "lastChange": "2015-10-08T02:56:00.5943566+00:00",
  "orderGroup": "566880726117",
  "totals": [
    {
      "id": "Items",
      "name": "Total dos Itens",
      "value": 80000
    },
    {
      "id": "Discounts",
      "name": "Total dos Descontos",
      "value": 0
    },
    {
      "id": "Shipping",
      "name": "Total do Frete",
      "value": 500
    },
    {
      "id": "Tax",
      "name": "Total da Taxa",
      "value": 0
    }
  ],
  "items": [
    {
      "uniqueId": "39281E5A994E4136AC5D4B090830F6FA",
      "id": "15",
      "productId": "4",
      "ean": "0123456789127",
      "lockId": null,
      "itemAttachment": {
        "content": {},
        "name": null
      },
      "attachments": [],
      "quantity": 1,
      "seller": "sandboxintegracao",
      "name": "TV LED 24\" Full HD 2 HDMI Conversor Digital LE24D1440 AOC 110",
      "refId": "AJHDAOdh",
      "price": 80000,
      "listPrice": 82000,
      "manualPrice": null,
      "priceTags": [],
      "imageUrl": "http://fastshop.vteximg.com.br/arquivos/ids/155347-55-55/110_0.jpg",
      "detailUrl": "/tv-led-24--full-hd-2-hdmi-conversor-digital-le24d1440-aoc-4/p",
      "components": [],
      "bundleItems": [],
      "params": [],
      "offerings": [
        {
          "type": "Garantia Extendida SMARTIA 12 Meses",
          "id": "2",
          "name": "Garantia Extendida SMARTIA 12 Meses",
          "price": 12000
        },
        {
          "type": "Garantia Extendida SMARTIA 24 Meses",
          "id": "3",
          "name": "Garantia Extendida SMARTIA 24 Meses",
          "price": 21000
        }
      ],
      "sellerSku": "2000037",
      "priceValidUntil": null,
      "commission": 0,
      "tax": 0,
      "preSaleDate": null,
      "additionalInfo": {
        "brandName": "AOC",
        "brandId": "2000000",
        "categoriesIds": "/8/9/10/",
        "productClusterId": "",
        "commercialConditionId": "1",
        "dimension": {
          "cubicweight": 0.1667,
          "height": 10,
          "length": 10,
          "weight": 10,
          "width": 10
        },
        "offeringInfo": null,
        "offeringType": null,
        "offeringTypeId": null
      },
      "measurementUnit": "un",
      "unitMultiplier": 1,
      "sellingPrice": 80000,
      "isGift": false,
      "shippingPrice": null
    }
  ],
  "marketplaceItems": [],
  "clientProfileData": {
    "id": "clientProfileData",
    "email": "84e7100d66364ab3834cfe54c672ebb1@ct.vtex.com.br",
    "firstName": "Jonas",
    "lastName": "Bolognim",
    "documentType": "cpf",
    "document": "08896581885",
    "phone": "999981488",
    "corporateName": null,
    "tradeName": null,
    "corporateDocument": null,
    "stateInscription": null,
    "corporatePhone": null,
    "isCorporate": false,
    "userProfileId": "4cc1f644-69ba-4619-92bc-1d9af0c6deeb"
  },
  "giftRegistryData": null,
  "marketingData": null,
  "ratesAndBenefitsData": {
    "id": "ratesAndBenefitsData",
    "rateAndBenefitsIdentifiers": []
  },
  "shippingData": {
    "id": "shippingData",
    "address": {
      "addressType": "residential",
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
      "reference": "Bairro Praia Azul / Posto de Saúde 17"
    },
    "logisticsInfo": [
      {
        "itemIndex": 0,
        "selectedSla": "Normal",
        "lockTTL": null,
        "price": 500,
        "listPrice": 500,
        "sellingPrice": 0,
        "deliveryWindow": null,
        "deliveryCompany": "Normal",
        "shippingEstimate": "4bd",
        "shippingEstimateDate": "2015-10-12T16:16:56.7561604+00:00",
        "slas": [
          {
            "id": "Normal",
            "name": "Normal",
            "shippingEstimate": "4bd",
            "deliveryWindow": null,
            "price": 500
          },
          {
            "id": "Entrega Agendada",
            "name": "Entrega Agendada",
            "shippingEstimate": "4bd",
            "deliveryWindow": null,
            "price": 1500
          }
        ],
        "shipsTo": [],
        "deliveryIds": [
          {
            "courierId": "2",
            "courierName": "Normal",
            "dockId": "1",
            "quantity": 1,
            "warehouseId": "1_1"
          }
        ]
      }
    ]
  },
  "paymentData": {
    "transactions": [
      {
        "isActive": true,
        "transactionId": "CE0C550E892044D3BDDA435984D73860",
        "payments": [
          {
            "id": "929FDCB1C9FB4111875F2863A47FF8D2",
            "paymentSystem": "201",
            "paymentSystemName": "FastShop Money F$",
            "value": 90500,
            "installments": 1,
            "referenceValue": 90500,
            "cardHolder": null,
            "cardNumber": null,
            "firstDigits": null,
            "lastDigits": null,
            "cvv2": null,
            "expireMonth": null,
            "expireYear": null,
            "url": null,
            "giftCardId": null,
            "giftCardName": null,
            "giftCardCaption": null,
            "redemptionCode": null,
            "group": "promissory",
            "tid": null,
            "dueDate": null,
            "connectorResponses": {}
          }
        ]
      }
    ]
  },
  "packageAttachment": {
    "packages": [
      {
        "items": [
          {
            "itemIndex": 0,
            "quantity": 1,
            "price": 80000,
            "description": null
          }
        ],
        "courier": null,
        "invoiceNumber": "500617",
        "invoiceValue": 80500,
        "invoiceUrl": null,
        "issuanceDate": "2015-10-07T03:00:00.0000000+00:00",
        "trackingNumber": null,
        "invoiceKey": null,
        "trackingUrl": null,
        "embeddedInvoice": null,
        "type": "Output",
        "courierStatus": {
          "status": "unknown",
          "finished": false,
          "data": []
        }
      }
    ]
  },
  "sellers": [
    {
      "id": "sandboxintegracao",
      "name": "Sandbox-integracao",
      "logo": "http://portal.vtexcommercestable.com.br/arquivos/logo.jpg"
    }
  ],
  "callCenterOperatorData": null,
  "followUpEmail": "9aa35cf23be347688fc20446d31371b2@ct.vtex.com.br",
  "lastMessage": "fastshop Seu pedido foi faturado! realizado em: 06/10/2015 Olá, Jonas. Os produtos abaixo já foram faturados só aguardando envio para a tran",
  "hostname": "fastshop",
  "changesAttachment": null,
  "openTextField": null,
  "roundingError": 0
}
```


Esses são os passos que devem ser seguidos para a integração com o VTEX Marketplace Switch.


> POR LER ATE AQUI
>> Na ferramenta POSTMAN usando esse link ``` https://www.getpostman.com/collections/00738f3e2732d6aa736a ``` é possível importar uma coleção de requests. Nela tem um exemplo de cada request citado nesse documento.



---
Autor: _Jonas Bolognim_</br>
Propriedade:_VTEX &copy;_</br>
