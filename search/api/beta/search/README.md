# Search

A Api de busca tem o objetivo de trazer o mesmo resultado de produtos retornados na navegação de qualquer url do site, porém em um formato Json que ajude a integração com outros dispositivos, customizações e facilite a integração de sistemas 

A busca poderá ser realizada de diversas formas, vamos a elas.

- [Texto](#texto) - Essa é a busca natural do site.

- [Categoria](#categoria) - Busca por categoria simples.

- [Produto](#produto) - Busca por produto simples.

- [Filtros](#filtros) - Busca por filtros e especificações.

- [Paginação](#paginacao) - Paginando resultados.

- [Exemplo de retorno](#exemplo).

## Busca por texto


Essa é a busca mais simples do site.

Modelo da url: http://{storeurl}/api/catalog_system/pub/products/search/{searchterms}

{storeurl} será substituído pela url da loja, vamos supor www.nomedaloja.com

{searchterms} sera substituído pela palavra a ser buscada, vamos buscar por "camisa"

Exemplo da url: http://www.nomedaloja.com/api/catalog_system/pub/products/search/camisa

## Busca por categoria simples


Modelo da url: http://{storeurl}/api/catalog_system/pub/products/search/{searchterms}

{storeurl} será substituído pela url da loja, vamos supor www.nomedaloja.com.br

{searchterms} sera substituído pela estrutura da categoria que a navegação normal do site já monta. Vamos entrar no departamento www.nomedaloja.com.br/eletrodomesticos/televisao na busca ficaria 

Exemplo da url: http://www.nomedaloja.com.br/api/catalog_system/pub/products/search/eletrodomesticos/televisao

## Busca por filtros e especificações


Modelo da url: http://{storeurl}/api/catalog_system/pub/products/search/?{searchquery}

{storeurl} será substituído pela url da loja, vamos supor www.nomedaloja.com

{searchquery} sera substituído pela estrutura da busca que pode ser montada com alguns parâmetros 

- fq=C:/{a}/{b}                             -    Filtra os resultados por um path por id de categorias Ex.: - -       fq=C:/1000041/1000049/
- fq=specificationFilter_{a}:{b}    -    Filtro que esta sendo usado para a busca, {a} = id da especificação {b} = valor selecionado Ex.: fq=specificationFilter_12:Masculino
- fq=P:[{a} TO {b}]                      -    Filtro por preço onde o {a} é o preço "de" e o {b} é o preço "até" Ex.:   fq=P:[0 TO 20]   vai buscar produto de 0 a 20 reais.

Exemplo da url: http://www.nomedaloja.com/api/catalog_system/pub/products/search/?fq=C:/1000041/1000049/&fq=specificationFilter_12:Masculino&fq=P:[0 TO 20]

## Paginação


A busca por padrão retorna 10 resultados. E possui um limite de 50 resultados por request.
Para solicitar mais resultados envie um header http
chave: resources
value: {a}-{b}  -  {a} registro inicial, {b} registro final

Se desejar receber os 20 primeiros itens chame então

Ex: resources 0-19

No caso da página ter mais resultados o HTTP Code retornado será um 206 Partial Content
Sempre o resultado voltará no header de resposta o mesmo parâmetro resources com os itens solicitados/total de registros

Ex: resources 0-19/32

Para pegar os outros itens faça novamente o mesmo request alterando o header para

resources 20-32

Quando a página retornar todos os registros o HTTP Code será um 200 OK


## Exemplo de retorno

````json
[  
   {  
      "productName":"Camisa Calvin Klein Jeans Infantil ML City Line CKJ Marinho",
      "brand":"Calvin Klein Jeans",
      "linkText":"camisa-calvin-klein-jeans-infantil-ml-city-line-ckj-marinho-179957",
      "productReference":"7909163367117",
      "categories":[  
         "/Infantil/Vestuário Masculino/Camisa/",
         "/Infantil/Vestuário Masculino/",
         "/Infantil/"
      ],
      "link":"http://www.nomedaloja.com/camisa-calvin-klein-jeans-infantil-ml-city-line-ckj-marinho-179957/p",
      "Especificações":[  
         " 
<div id=\"tabelanova\">\r\n 
<table width=\"226\" border=\"0\" cellspacing=\"0\" cellpadding=\"5\" style=\"text-align:center; font-size:12px; color:#666666; font-family:Arial, Helvetica, sans-serif;\" >\r\n 
<tr style=\"color:#000000\">\r\n     
<td>&nbsp;</td>\r\n 
    <td>Torax</td>\r\n     
<td>Costas</td>\r\n 
<td>Manga</td>\r\n 
<td>Comp.</td>\r\n  </tr>\r\n 
<tr bgcolor=\"#dbdbdb\">\r\n 
    <td style=\"color:#000000\">6</td>\r\n   
  	<td>40cm</td>\r\n    <td>30cm</td>\r\n 
    <td>41cm</td>\r\n    <td>55cm</td>\r\n   
</tr>\r\n 
<tr>\r\n     
<td style=\"color:#000000\">8</td>\r\n 
    <td>42cm</td>\r\n     
<td>32cm</td>\r\n 
    <td>45cm</td>\r\n     
<td>56cm</td>\r\n  </tr>\r\n  
 <tr bgcolor=\"#dbdbdb\">\r\n 
    <td style=\"color:#000000\">10</td>\r\n  
   	<td>44cm</td>\r\n     
<td>34cm</td>\r\n   
<td>49cm</td>\r\n     
<td>57cm</td>\r\n   
</tr>\r\n   
<tr>\r\n 
    <td style=\"color:#000000\">12</td>\r\n 
    <td>46cm</td>\r\n 
    <td>36cm</td>\r\n 
    <td>54cm</td>\r\n 
    <td>61cm</td>\r\n   
</tr>\r\n   
<tr bgcolor=\"#dbdbdb\">\r\n 
   <td style=\"color:#000000\">14</td>\r\n 
   <td>48cm</td>\r\n     
   <td>38cm</td>\r\n     
   <td>60cm</td>\r\n     
  <td>67cm</td>\r\n 
</tr>\r\n </table>\r\n</div>"
      ],
      "Composição":[  
         "Composição: 100% Algodão"
      ],
      "Sexo":[  
         "Masculino"
      ],
      "allSpecifications":[  
         "Especificações",
         "Composição",
         "Sexo"
      ],
      "description":"",
      "items":[  
         {  
            "itemId":"2060772",
            "name":"Camisa Calvin Klein Jeans Infantil ML City Line CKJ Marinho",
            "nameComplete":"Camisa Calvin Klein Jeans Infantil ML City Line CKJ Marinho",
            "complementName":"",
            "ean":"7909163367117",
            "referenceId":[  
               {  
                  "Key":"RefId",
                  "Value":"179957_10"
               }
            ],
            p://nomedaloja.vteximg.com.br/arquivos/ids/666171/7909163367100_1.jpg",
            "imageText":"7909163367100_1"
         }
      ],
      "Tamanho":[  
         "6"
      ],
      "variations":[  
         "Tamanho"
      ],
      "sellers":[  
         {  
            "sellerId":"1",
            "sellerName":"nomedaloja",
            "addToCartLink":"http: //www.nomedaloja.com/checkout/cart/add?sku=2060775&qty=1&seller=1&sc=1",
            "commertialOffer":{  
               "Price":229.0,
               "ListPrice":229.0,
               "PriceValidUntil":null,
               "AvailableQuantity":1,
               "DeliverySlaSamples":[  
                  {  
                     "DeliverySlaPerTypes":[  
                        {  
                           "TypeName":"Normal",
                           "Price":20.0,
                           "EstimatedTimeSpanToDelivery":"3.00: 00: 00"
                        }
                     ],
                     "Region":{  
                        "images":[  
                           {  
                              "imageId":"666172",
                              "imageLabel":"over",
                              "imageTag":"<img src=\"~/arquivos/ids/666172-#width#-#height#/7909163367100_2.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163367100_2\" id=\"\"/>",
                              "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/666172/7909163367100_2.jpg",
                              "imageText":"7909163367100_2"
                           },
                           {  
                              "imageId":"666171",
                              "imageLabel":null,
                              "imageTag":"<imgsrc=\"~/arquivos/ids/666171-#width#-#height#/7909163367100_1.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163367100_1\" id=\"\"/>",
                              "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/666171/7909163367100_1.jpg",
                              "imageText":"7909163367100_1"
                           }
                        ],
                        "Tamanho":[  
                           "10"
                        ],
                        "variations":[  
                           "Tamanho"
                        ],
                        "sellers":[  
                           {  
                              "sellerId":"1",
                              "sellerName":"nomedaloja",
                              "addToCartLink":"http: //www.nomedaloja.com/checkout/cart/add?sku=2060772&qty=1&seller=1&sc=1",
                              "commertialOffer":{  
                                 "Price":229.0,
                                 "ListPrice":229.0,
                                 "PriceValidUntil":null,
                                 "AvailableQuantity":1,
                                 "DeliverySlaSamples":[  
                                    {  
                                       "DeliverySlaPerTypes":[  
                                          {  
                                             "TypeName":"Normal",
                                             "Price":20.0,
                                             "EstimatedTimeSpanToDelivery":"3.00: 00: 00"
                                          }
                                       ],
                                       "Region":{  
                                          "IsPersisted":false,
                                          "IsRemoved":false,
                                          "Id":1,
                                          "Name":"AreadaVtexSP",
                                          "CountryCode":"BRA",
                                          "ZipCode":"04548005",
                                          "CultureInfoName":"pt-BR"
                                       }
                                    }
                                 ]
                              }
                           }
                        ]
                     },
                     {  
                        "itemId":"2060773",
                        "name":"CamisaCalvinKleinJeansInfantilMLCityLineCKJMarinho",
                        "nameComplete":"CamisaCalvinKleinJeansInfantilMLCityLineCKJMarinho",
                        "complementName":"",
                        "ean":"7909163367124",
                        "referenceId":[  
                           {  
                              "Key":"RefId",
                              "Value":"179957_12"
                           }
                        ],
                        "images":[  
                           {  
                              "imageId":"666172",
                              "imageLabel":"over",
                              "imageTag":"<imgsrc=\"~/arquivos/ids/666172-#width#-#height#/7909163367100_2.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163367100_2\" id=\"\"/>",
                              "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/666172/7909163367100_2.jpg",
                              "imageText":"7909163367100_2"
                           },
                           {  
                              "imageId":"666171",
                              "imageLabel":null,
                              "imageTag":"<imgsrc=\"~/arquivos/ids/666171-#width#-#height#/7909163367100_1.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163367100_1\" id=\"\"/>",
                              "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/666171/7909163367100_1.jpg",
                              "imageText":"7909163367100_1"
                           }
                        ],
                        "Tamanho":[  
                           "12"
                        ],
                        "variations":[  
                           "Tamanho"
                        ],
                        "sellers":[  
                           {  
                              "sellerId":"1",
                              "sellerName":"nomedaloja",
                              "addToCartLink":"http: //www.nomedaloja.com/checkout/cart/add?sku=2060773&qty=1&seller=1&sc=1",
                              "commertialOffer":{  
                                 "Price":229.0,
                                 "ListPrice":229.0,
                                 "PriceValidUntil":"2114-05-14T19: 12: 53.86",
                                 "AvailableQuantity":0,
                                 "DeliverySlaSamples":[  
                                    {  
                                       "DeliverySlaPerTypes":[  

                                       ],
                                       "Region":{  
                                          "IsPersisted":false,
                                          "IsRemoved":false,
                                          "Id":1,
                                          "Name":"AreadaVtexSP",
                                          "CountryCode":"BRA",
                                          "ZipCode":"04548005",
                                          "CultureInfoName":"pt-BR"
                                       }
                                    }
                                 ]
                              }
                           }
                        ]
                     },
                     {  
                        "itemId":"2060774",
                        "name":"CamisaCalvinKleinJeansInfantilMLCityLineCKJMarinho",
                        "nameComplete":"CamisaCalvinKleinJeansInfantilMLCityLineCKJMarinho",
                        "complementName":"",
                        "ean":"7909163367131",
                        "referenceId":[  
                           {  
                              "Key":"RefId",
                              "Value":"179957_14"
                           }
                        ],
                        "images":[  
                           {  
                              "imageId":"666172",
                              "imageLabel":"over",
                              "imageTag":"<imgsrc=\"~/arquivos/ids/666172-#width#-#height#/7909163367100_2.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163367100_2\" id=\"\"/>",
                              "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/666172/7909163367100_2.jpg",
                              "imageText":"7909163367100_2"
                           },
                           {  
                              "imageId":"666171",
                              "imageLabel":null,
                              "imageTag":"<imgsrc=\"~/arquivos/ids/666171-#width#-#height#/7909163367100_1.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163367100_1\" id=\"\"/>",
                              "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/666171/7909163367100_1.jpg",
                              "imageText":"7909163367100_1"
                           }
                        ],
                        "Tamanho":[  
                           "14"
                        ],
                        "variations":[  
                           "Tamanho"
                        ],
                        "sellers":[  
                           {  
                              "sellerId":"1",
                              "sellerName":"nomedaloja",
                              "addToCartLink":"http: //www.nomedaloja.com/checkout/cart/add?sku=2060774&qty=1&seller=1&sc=1",
                              "commertialOffer":{  
                                 "Price":229.0,
                                 "ListPrice":229.0,
                                 "PriceValidUntil":null,
                                 "AvailableQuantity":1,
                                 "DeliverySlaSamples":[  
                                    {  
                                       "DeliverySlaPerTypes":[  
                                          {  
                                             "TypeName":"Normal",
                                             "Price":20.0,
                                             "EstimatedTimeSpanToDelivery":"3.00: 00: 00"
                                          }
                                       ],
                                       "Region":{  
                                          "IsPersisted":false,
                                          "IsRemoved":false,
                                          "Id":1,
                                          "Name":"AreadaVtexSP",
                                          "CountryCode":"BRA",
                                          "ZipCode":"04548005",
                                          "CultureInfoName":"pt-BR"
                                       }
                                    }
                                 ]
                              }
                           }
                        ]
                     },
                     {  
                        "itemId":"2060775",
                        "name":"CamisaCalvinKleinJeansInfantilMLCityLineCKJMarinho",
                        "nameComplete":"CamisaCalvinKleinJeansInfantilMLCityLineCKJMarinho",
                        "complementName":"",
                        "ean":"7909163367094",
                        "referenceId":[  
                           {  
                              "Key":"RefId",
                              "Value":"179957_6"
                           }
                        ],
                        "images":[  
                           {  
                              "imageId":"666172",
                              "imageLabel":"over",
                              "imageTag":"<imgsrc=\"~/arquivos/ids/666172-#width#-#height#/7909163367100_2.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163367100_2\" id=\"\"/>",
                              "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/666172/7909163367100_2.jpg",
                              "imageText":"7909163367100_2"
                           },
                           {  
                              "imageId":"666171",
                              "imageLabel":null,
                              "imageTag":"<imgsrc=\"~/arquivos/ids/666171-#width#-#height#/7909163367100_1.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163367100_1\" id=\"\"/>",
                              "imageUrl":"htt"                              IsPersisted":false,
                              "IsRemoved":false,
                              "Id":1,
                              "Name":"AreadaVtexSP",
                              "CountryCode":"BRA",
                              "ZipCode":"04548005",
                              "CultureInfoName":"pt-BR"
                           }
                        }
                     ]
                  }
               }
            ]
         },
         {  
            "itemId":"2060776",
            "name":"CamisaCalvinKleinJeansInfantilMLCityLineCKJMarinho",
            "nameComplete":"CamisaCalvinKleinJeansInfantilMLCityLineCKJMarinho",
            "complementName":"",
            "ean":"7909163367100",
            "referenceId":[  
               {  
                  "Key":"RefId",
                  "Value":"179957_8"
               }
            ],
            "images":[  
               {  
                  "imageId":"666172",
                  "imageLabel":"over",
                  "imageTag":"<imgsrc=\"~/arquivos/ids/666172-#width#-#height#/7909163367100_2.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163367100_2\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/666172/7909163367100_2.jpg",
                  "imageText":"7909163367100_2"
               },
               {  
                  "imageId":"666171",
                  "imageLabel":null,
                  "imageTag":"<imgsrc=\"~/arquivos/ids/666171-#width#-#height#/7909163367100_1.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163367100_1\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/666171/7909163367100_1.jpg",
                  "imageText":"7909163367100_1"
               }
            ],
            "Tamanho":[  
               "8"
            ],
            "variations":[  
               "Tamanho"
            ],
            "sellers":[  
               {  
                  "sellerId":"1",
                  "sellerName":"nomedaloja",
                  "addToCartLink":"http: //www.nomedaloja.com/checkout/cart/add?sku=2060776&qty=1&seller=1&sc=1",
                  "commertialOffer":{  
                     "Price":229.0,
                     "ListPrice":229.0,
                     "PriceValidUntil":"2114-05-14T19: 12: 53.86",
                     "AvailableQuantity":0,
                     "DeliverySlaSamples":[  
                        {  
                           "DeliverySlaPerTypes":[  

                           ],
                           "Region":{  
                              "IsPersisted":false,
                              "IsRemoved":false,
                              "Id":1,
                              "Name":"AreadaVtexSP",
                              "CountryCode":"BRA",
                              "ZipCode":"04548005",
                              "CultureInfoName":"pt-BR"
                           }
                        }
                     ]
                  }
               }
            ]
         }
      ]
   },
   {  
      "productName":"CamisaCalvinKleinJeansInfantilMLEastCoastBranca",
      "brand":"CalvinKleinJeans",
      "linkText":"camisa-calvin-klein-jeans-infantil-ml-east-coast-branca-179969",
      "productReference":"7909163368565",
      "categories":[  
         "/Infantil/VestuárioMasculino/Camisa/",
         "/Infantil/VestuárioMasculino/",
         "/Infantil/"
      ],
      "link":"http: //www.nomedaloja.com/camisa-calvin-klein-jeans-infantil-ml-east-coast-branca-179969/p",
      "Especificações":[
			"<div id=\"tabelanova\">\r\n
			<table width=\"226\" border=\"0\" cellspacing=\"0\" cellpadding=\"5\" style=\"text-align:center; font-size:12px; color:#666666; font-family:Arial, Helvetica, sans-serif;\" >\r\n
				<tr style=\"color:#000000\">\r\n    
					<td>&nbsp;</td>\r\n
				    <td>Torax</td>\r\n    
					<td>Costas</td>\r\n
					<td>Manga</td>\r\n
					<td>Comp.</td>\r\n  </tr>\r\n
				<tr bgcolor=\"#dbdbdb\">\r\n
				    <td style=\"color:#000000\">6</td>\r\n  
				  	<td>40cm</td>\r\n    <td>30cm</td>\r\n
				    <td>41cm</td>\r\n    <td>55cm</td>\r\n  
				</tr>\r\n
				<tr>\r\n    
					<td style=\"color:#000000\">8</td>\r\n
				    <td>42cm</td>\r\n    
					<td>32cm</td>\r\n
				    <td>45cm</td>\r\n    
					<td>56cm</td>\r\n  </tr>\r\n 
				 <tr bgcolor=\"#dbdbdb\">\r\n
				    <td style=\"color:#000000\">10</td>\r\n 
				   	<td>44cm</td>\r\n    
					<td>34cm</td>\r\n  
					<td>49cm</td>\r\n    
					<td>57cm</td>\r\n  
				</tr>\r\n  
				<tr>\r\n
				    <td style=\"color:#000000\">12</td>\r\n
				    <td>46cm</td>\r\n
				    <td>36cm</td>\r\n
				    <td>54cm</td>\r\n
				    <td>61cm</td>\r\n  
				</tr>\r\n  
				<tr bgcolor=\"#dbdbdb\">\r\n
					   <td style=\"color:#000000\">14</td>\r\n
					   <td>48cm</td>\r\n    
					   <td>38cm</td>\r\n    
					   <td>60cm</td>\r\n    
					  <td>67cm</td>\r\n
				</tr>\r\n </table>\r\n</div>"
      ],
      "Composição":[  
         "Composição: 100% Algodão"
      ],
      "Sexo":[  
         "Masculino"
      ],
      "allSpecifications":[  
         "Especificações",
         "Composição",
         "Sexo"
      ],
      "description":"",
      "items":[  
         {  
            "itemId":"2056171",
            "name":"Camisa Calvin Klein Jeans Infantil ML East Coast Branca",
            "nameComplete":"Camisa Calvin Klein Jeans Infantil ML East Coast Branca",
            "complementName":"",
            "ean":"7909163368565",
            "referenceId":[  
               {  
                  "Key":"RefId",
                  "Value":"179969_6"
               }
            ],
            "images":[  
               {  
                  "imageId":"660125",
                  "imageLabel":"over",
                  "imageTag":"<img src=\"~/arquivos/ids/660125-#width#-#height#/7909163368572_2.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163368572_2\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/660125/7909163368572_2.jpg",
                  "imageText":"7909163368572_2"
               },
               {  
                  "imageId":"660124",
                  "imageLabel":null,
                  "imageTag":"<imgsrc=\"~/arquivos/ids/660124-#width#-#height#/7909163368572_1.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163368572_1\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/660124/7909163368572_1.jpg",
                  "imageText":"7909163368572_1"
               }
            ],
            "Tamanho":[  
               "6"
            ],
            "variations":[  
               "Tamanho"
            ],
            "sellers":[  
               {  
                  "sellerId":"1",
                  "sellerName":"nomedaloja",
                  "addToCartLink":"http: //www.nomedaloja.com/checkout/cart/add?sku=2056171&qty=1&seller=1&sc=1",
                  "commertialOffer":{  
                     "Price":229.0,
                     "ListPrice":229.0,
                     "PriceValidUntil":null,
                     "AvailableQuantity":1,
                     "DeliverySlaSamples":[  
                        {  
                           "DeliverySlaPerTypes":[  
                              {  
                                 "TypeName":"Normal",
                                 "Price":20.0,
                                 "EstimatedTimeSpanToDelivery":"3.00: 00: 00"
                              }
                           ],
                           "Region":{  
                              "IsPersisted":false,
                              "IsRemoved":false,
                              "Id":1,
                              "Name":"AreadaVtexSP",
                              "CountryCode":"BRA",
                              "ZipCode":"04548005",
                              "CultureInfoName":"pt-BR"
                           }
                        }
                     ]
                  }
               }
            ]
         },
         {  
            "itemId":"2056172",
            "name":"CamisaCalvinKleinJeansInfantilMLEastCoastBranca",
            "nameComplete":"CamisaCalvinKleinJeansInfantilMLEastCoastBranca",
            "complementName":"",
            "ean":"7909163368572",
            "referenceId":[  
               {  
                  "Key":"RefId",
                  "Value":"179969_8"
               }
            ],
            "images":[  
               {  
                  "imageId":"660125",
                  "imageLabel":"over",
                  "imageTag":"<imgsrc=\"~/arquivos/ids/660125-#width#-#height#/7909163368572_2.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163368572_2\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/660125/7909163368572_2.jpg",
                  "imageText":"7909163368572_2"
               },
               {  
                  "imageId":"660124",
                  "imageLabel":null,
                  "imageTag":"<imgsrc=\"~/arquivos/ids/660124-#width#-#height#/7909163368572_1.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163368572_1\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/660124/7909163368572_1.jpg",
                  "imageText":"7909163368572_1"
               }
            ],
            "Tamanho":[  
               "8"
            ],
            "variations":[  
               "Tamanho"
            ],
            "sellers":[  
               {  
                  "sellerId":"1",
                  "sellerName":"nomedaloja",
                  "addToCartLink":"http: //www.nomedaloja.com/checkout/cart/add?sku=2056172&qty=1&seller=1&sc=1",
                  "commertialOffer":{  
                     "Price":229.0,
                     "ListPrice":229.0,
                     "PriceValidUntil":null,
                     "AvailableQuantity":1,
                     "DeliverySlaSamples":[  
                        {  
                           "DeliverySlaPerTypes":[  
                              {  
                                 "TypeName":"Normal",
                                 "Price":20.0,
                                 "EstimatedTimeSpanToDelivery":"3.00: 00: 00"
                              }
                           ],
                           "Region":{  
                              "IsPersisted":false,
                              "IsRemoved":false,
                              "Id":1,
                              "Name":"AreadaVtexSP",
                              "CountryCode":"BRA",
                              "ZipCode":"04548005",
                              "CultureInfoName":"pt-BR"
                           }
                        }
                     ]
                  }
               }
            ]
         },
         {  
            "itemId":"2056173",
            "name":"CamisaCalvinKleinJeansInfantilMLEastCoastBranca",
            "nameComplete":"CamisaCalvinKleinJeansInfantilMLEastCoastBranca",
            "complementName":"",
            "ean":"7909163368589",
            "referenceId":[  
               {  
                  "Key":"RefId",
                  "Value":"179969_10"
               }
            ],
            "images":[  
               {  
                  "imageId":"660125",
                  "imageLabel":"over",
                  "imageTag":"<imgsrc=\"~/arquivos/ids/660125-#width#-#height#/7909163368572_2.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163368572_2\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/660125/7909163368572_2.jpg",
                  "imageText":"7909163368572_2"
               },
               {  
                  "imageId":"660124",
                  "imageLabel":null,
                  "imageTag":"<imgsrc=\"~/arquivos/ids/660124-#width#-#height#/7909163368572_1.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163368572_1\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/660124/7909163368572_1.jpg",
                  "imageText":"7909163368572_1"
               }
            ],
            "Tamanho":[  
               "10"
            ],
            "variations":[  
               "Tamanho"
            ],
            "sellers":[  
               {  
                  "sellerId":"1",
                  "sellerName":"nomedaloja",
                  "addToCartLink":"http: //www.nomedaloja.com/checkout/cart/add?sku=2056173&qty=1&seller=1&sc=1",
                  "commertialOffer":{  
                     "Price":229.0,
                     "ListPrice":229.0,
                     "PriceValidUntil":"2114-05-14T19: 12: 53.86",
                     "AvailableQuantity":0,
                     "DeliverySlaSamples":[  
                        {  
                           "DeliverySlaPerTypes":[  

                           ],
                           "Region":{  
                              "IsPersisted":false,
                              "IsRemoved":false,
                              "Id":1,
                              "Name":"AreadaVtexSP",
                              "CountryCode":"BRA",
                              "ZipCode":"04548005",
                              "CultureInfoName":"pt-BR"
                           }
                        }
                     ]
                  }
               }
            ]
         },
         {  
            "itemId":"2056174",
            "name":"CamisaCalvinKleinJeansInfantilMLEastCoastBranca",
            "nameComplete":"CamisaCalvinKleinJeansInfantilMLEastCoastBranca",
            "complementName":"",
            "ean":"7909163368596",
            "referenceId":[  
               {  
                  "Key":"RefId",
                  "Value":"179969_12"
               }
            ],
            "images":[  
               {  
                  "imageId":"660125",
                  "imageLabel":"over",
                  "imageTag":"<imgsrc=\"~/arquivos/ids/660125-#width#-#height#/7909163368572_2.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163368572_2\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/660125/7909163368572_2.jpg",
                  "imageText":"7909163368572_2"
               },
               {  
                  "imageId":"660124",
                  "imageLabel":null,
                  "imageTag":"<imgsrc=\"~/arquivos/ids/660124-#width#-#height#/7909163368572_1.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163368572_1\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/660124/7909163368572_1.jpg",
                  "imageText":"7909163368572_1"
               }
            ],
            "Tamanho":[  
               "12"
            ],
            "variations":[  
               "Tamanho"
            ],
            "sellers":[  
               {  
                  "sellerId":"1",
                  "sellerName":"nomedaloja",
                  "addToCartLink":"http: //www.nomedaloja.com/checkout/cart/add?sku=2056174&qty=1&seller=1&sc=1",
                  "commertialOffer":{  
                     "Price":229.0,
                     "ListPrice":229.0,
                     "PriceValidUntil":null,
                     "AvailableQuantity":1,
                     "DeliverySlaSamples":[  
                        {  
                           "DeliverySlaPerTypes":[  
                              {  
                                 "TypeName":"Normal",
                                 "Price":20.0,
                                 "EstimatedTimeSpanToDelivery":"3.00: 00: 00"
                              }
                           ],
                           "Region":{  
                              "IsPersisted":false,
                              "IsRemoved":false,
                              "Id":1,
                              "Name":"AreadaVtexSP",
                              "CountryCode":"BRA",
                              "ZipCode":"04548005",
                              "CultureInfoName":"pt-BR"
                           }
                        }
                     ]
                  }
               }
            ]
         },
         {  
            "itemId":"2056175",
            "name":"CamisaCalvinKleinJeansInfantilMLEastCoastBranca",
            "nameComplete":"CamisaCalvinKleinJeansInfantilMLEastCoastBranca",
            "complementName":"",
            "ean":"7909163368602",
            "referenceId":[  
               {  
                  "Key":"RefId",
                  "Value":"179969_14"
               }
            ],
            "images":[  
               {  
                  "imageId":"660125",
                  "imageLabel":"over",
                  "imageTag":"<imgsrc=\"~/arquivos/ids/660125-#width#-#height#/7909163368572_2.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163368572_2\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/660125/7909163368572_2.jpg",
                  "imageText":"7909163368572_2"
               },
               {  
                  "imageId":"660124",
                  "imageLabel":null,
                  "imageTag":"<imgsrc=\"~/arquivos/ids/660124-#width#-#height#/7909163368572_1.jpg\" width=\"#width#\" height=\"#height#\" alt=\"7909163368572_1\" id=\"\"/>",
                  "imageUrl":"http: //nomedaloja.vteximg.com.br/arquivos/ids/660124/7909163368572_1.jpg",
                  "imageText":"7909163368572_1"
               }
            ],
            "Tamanho":[  
               "14"
            ],
            "variations":[  
               "Tamanho"
            ],
            "sellers":[  
               {  
                  "sellerId":"1",
                  "sellerName":"nomedaloja",
                  "addToCartLink":"http: //www.nomedaloja.com/checkout/cart/add?sku=2056175&qty=1&seller=1&sc=1",
                  "commertialOffer":{  
                     "Price":229.0,
                     "ListPrice":229.0,
                     "PriceValidUntil":null,
                     "AvailableQuantity":1,
                     "DeliverySlaSamples":[  
                        {  
                           "DeliverySlaPerTypes":[  
                              {  
                                 "TypeName":"Normal",
                                 "Price":20.0,
                                 "EstimatedTimeSpanToDelivery":"3.00: 00: 00"
                              }
                           ],
                           "Region":{  
                              "IsPersisted":false,
                              "IsRemoved":false,
                              "Id":1,
                              "Name":"AreadaVtexSP",
                              "CountryCode":"BRA",
                              "ZipCode":"04548005",
                              "CultureInfoName":"pt-BR"
                           }
                        }
                     ]
                  }
               }
            ]
         }
      ]
   }
]
```
