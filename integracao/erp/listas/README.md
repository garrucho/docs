##Integração de Listas (aniversário, casamento, chás, etc...)

Este documento tem por objetivo auxiliar na integração de uma lista entre o ERP e uma loja hospedada na palataforma da VTEX. As listas na maioria dos cenários são criadas pelo próprio cliente final no e-commerce da loja, mas existem cenários onde a lista é criada em outro ambiente, loja fisica, por exemplo, então deve se usar a sequencia de métodos abaixo para finalizar esse tarefa:


###Inserir Cliente

Para iniciar o processo de criação de listas, o primeiro passo é inserir o dono da lista, aqui nomeado de cliente.

Abaixo exemplos de chamada e resposta de inserção do cliente dono da lista através do método "ClientInsertUpdateV3":

_request:_  

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:ClientInsertUpdateV3>
         <tem:client>            
            <vtex:CpfCnpj>08896581885</vtex:CpfCnpj> <!--string, documento do cliente-->
            <vtex:Email>jonasrj@hotmail.com</vtex:Email> <!--string, e-mail do cliente-->
            <vtex:FirstName>Jonas</vtex:FirstName> <!--string, nome do cliente-->
            <vtex:IsActive>true</vtex:IsActive> <!--bool, está ativo-->
            <vtex:LastName>Bolognim</vtex:LastName>  <!--string, sobrenome do cliente-->
            <vtex:Phone>21999981488</vtex:Phone> <!--string, fone de contato do cliente-->
         </tem:client>
      </tem:ClientInsertUpdateV3>
   </soapenv:Body>
</soapenv:Envelope>
```

_response:_  

```xml	
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
   <s:Body>
      <ClientInsertUpdateV3Response xmlns="http://tempuri.org/">
         <ClientInsertUpdateV3Result xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
            <a:AddressClientId i:nil="true"/>
            <a:Age i:nil="true"/>
            <a:CelPhone i:nil="true"/>
            <a:ChangeDate i:nil="true"/>
            <a:CompanyName i:nil="true"/>
            <a:CompanyNickName i:nil="true"/>
            <a:CpfCnpj>08896581885</a:CpfCnpj>
            <a:CreateDate i:nil="true"/>
            <a:DateBirth i:nil="true"/>
            <a:Email>jonasrj@hotmail.com</a:Email>
            <a:FirstName>Jonas</a:FirstName>
            <a:Gender i:nil="true"/>
            <a:Id i:nil="true"/>
            <a:IsActive>true</a:IsActive>
            <a:IsCorporate i:nil="true"/>
            <a:IsFraud i:nil="true"/>
            <a:IsNewsReceiver i:nil="true"/>
            <a:IsSMSNewsReceiver i:nil="true"/>
            <a:IsStateInscriptionImmune i:nil="true"/>
            <a:LastLogonDate i:nil="true"/>
            <a:LastName>Bolognim</a:LastName>
            <a:NickName i:nil="true"/>
            <a:OfficePhone i:nil="true"/>
            <a:Password i:nil="true"/>
            <a:Phone>21999981488</a:Phone>
            <a:RewardGiftCardId i:nil="true"/>
            <a:StateInscription i:nil="true"/>
            <a:UserGUID>e41efd14-b230-42f4-950a-56cc7cc689e4</a:UserGUID>
         </ClientInsertUpdateV3Result>
      </ClientInsertUpdateV3Response>
   </s:Body>
</s:Envelope>
```

###Criar o Endereço do Cliente  

Após inserir o cliente, é necessário inserir o endereço do cliente dono da lista.

Abaixo exemplos de chamada e resposta de inserção de endereço do dono da lista através do método "AddressInsertUpdateV3":

_request:_  

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:AddressInsertUpdateV3>
         <tem:address>
            <vtex:AddressNameV3>casa</vtex:AddressNameV3> <!--string, nome do endereço:-->
            <vtex:AddressType>residencial</vtex:AddressType>  <!--string, tipo: residencial, comercial-->
            <vtex:City>Rio de Janeiro</vtex:City> <!--string, cidade:-->
            <vtex:Country>BRA</vtex:Country> <!--string, sigla do país:-->
            <vtex:Neighborhood>Copacabana</vtex:Neighborhood> <!--string, bairro:-->
            <vtex:Number>188</vtex:Number> <!--int, numero do endereço-->
            <vtex:RecipientName>Jonas Bolognim</vtex:RecipientName> <!--string, nome do destinatário-->
            <vtex:State>RJ</vtex:State> <!--string, unidade federativa-->
            <vtex:Street>Rua 8 de maio</vtex:Street> <!--string, nome da rua-->
            <vtex:UserGUID>e41efd14-b230-42f4-950a-56cc7cc689e4</vtex:UserGUID> <!--identificador do cliente retornado na inserção-->
            <vtex:ZipCode>22051080</vtex:ZipCode> <!--CEP-->
         </tem:address>
      </tem:AddressInsertUpdateV3>
   </soapenv:Body>
</soapenv:Envelope>
```

_response_:  

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
   <s:Body>
      <AddressInsertUpdateV3Response xmlns="http://tempuri.org/">
         <AddressInsertUpdateV3Result xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
            <a:AddressGUID>00000000-0000-0000-0000-000000000000</a:AddressGUID>
            <a:AddressNameV3>casa</a:AddressNameV3>
            <a:AddressType>residencial</a:AddressType>
            <a:City>Rio de Janeiro</a:City>
            <a:Country>BRA</a:Country>
            <a:Id i:nil="true"/>
            <a:More i:nil="true"/>
            <a:Neighborhood>Copacabana</a:Neighborhood>
            <a:Number>188</a:Number>
            <a:RecipientName>Jonas Bolognim</a:RecipientName>
            <a:ReferencePoint i:nil="true"/>
            <a:State>RJ</a:State>
            <a:Street>Rua 8 de maio</a:Street>
            <a:UserGUID>00000000-0000-0000-0000-000000000000</a:UserGUID>
            <a:ZipCode>22051080</a:ZipCode>
         </AddressInsertUpdateV3Result>
      </AddressInsertUpdateV3Response>
   </s:Body>
</s:Envelope>
```

###Criar a Lista

Após inserir o cliente e o endereço do dono da lista, este é o momento de criar a lista.

Abaixo exemplos de chamada e resposta de criação de lista através do método "GiftListInsertUpdate":

_request:_  

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:GiftListInsertUpdate>
         <tem:giftList>
            
            <vtex:EventCity>Rio de Janeiro</vtex:EventCity> <!--nome da cidade do evento-->
            <vtex:EventDate>2015-10-12</vtex:EventDate> <!--data do evento-->
            <vtex:EventLocation>Praia de Botafogo 518</vtex:EventLocation> <!--localização do evento-->
            <vtex:EventState>RJ</vtex:EventState> <!--Unidade Federatica do evento-->
           
            <vtex:GiftListMembers>
               <vtex:GiftListMemberDTO>
                  <vtex:Title>Sr.</vtex:Title> <!--string, pronome de tratamento do membro-->
                  <vtex:Name>Hiran</vtex:Name> <!--string, nome do membro da lista-->
                  <vtex:Surname>Azedo</vtex:Surname> <!--string, nome de falilia do mebro-->
                  <vtex:Mail>hiran@vtex.com.br</vtex:Mail> <!--string, e-mail do mebro da lista:-->
                  <vtex:IsAdmin>false</vtex:IsAdmin> <!--bool, é admintrador da lista?:-->
                  <vtex:IsActive>true</vtex:IsActive> <!--bool, membro ativo? :-->
                  <vtex:ProfileSystemUserId></vtex:ProfileSystemUserId> <!--identificador do cliente membro, enviar vazio-->
               </vtex:GiftListMemberDTO>

	   		<vtex:GiftListMemberDTO>
                  <vtex:Title>Sra.</vtex:Title> <!--string, pronome de tratamento do membro-->
                  <vtex:Name>Eduarda</vtex:Name> <!--string, nome do membro da lista-->
                  <vtex:Surname>Doce</vtex:Surname> <!--string, nome de falilia do mebro-->
                  <vtex:Mail>eduarda@vtex.com.br</vtex:Mail> <!--string, e-mail do mebro da lista-->
                  <vtex:IsAdmin>false</vtex:IsAdmin> <!--bool, é admintrador da lista?-->
                  <vtex:IsActive>true</vtex:IsActive> <!--bool, membro ativo?-->
                  <vtex:ProfileSystemUserId></vtex:ProfileSystemUserId> <!--identificador do cliente membro, enviar vazio-->
               </vtex:GiftListMemberDTO>
            </vtex:GiftListMembers>

            <vtex:GiftListTypeId>2</vtex:GiftListTypeId> <!--identifador do tipo lista criado na loja VTEX-->
            <vtex:IsActive>true</vtex:IsActive> <!--bool, lista esta ativa-->
            <vtex:IsPublic>true</vtex:IsPublic> <!--bool, escopo da lista-->
            <vtex:Message>lindo, muito lindo o casal</vtex:Message> <!--menagem da lista-->
            <vtex:Name>hiran e eduarda</vtex:Name>  <!--string, nome da lista-->
            <vtex:ProfileSystemUserAddressName>casa</vtex:ProfileSystemUserAddressName> <!--AddressNameV3 inserido no endereço-->
            <vtex:ProfileSystemUserId>e41efd14-b230-42f4-950a-56cc7cc689e4</vtex:ProfileSystemUserId> <!--identificador do cliente criador da lista-->
            <vtex:UrlFolder>hiraneeduarda</vtex:UrlFolder> <!--string, caminho da lista no site, ex: http://meusite.com.br/list/hiraneeduarda-->
            <vtex:Version>2</vtex:Version> <!--smartcheckout, usar sempre 2-->
         </tem:giftList>
      </tem:GiftListInsertUpdate>
   </soapenv:Body>
</soapenv:Envelope>
```

_response_:  

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
   <s:Body>
      <GiftListInsertUpdateResponse xmlns="http://tempuri.org/">
         <GiftListInsertUpdateResult xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
            <a:ClientAddressId i:nil="true"/>
            <a:ClientId i:nil="true"/>
            <a:DateCreated>2015-03-10T12:54:44.7339224</a:DateCreated>
            <a:DateModified i:nil="true"/>
            <a:EventCity>Rio de Janeiro</a:EventCity>
            <a:EventDate>2015-10-12T00:00:00</a:EventDate>
            <a:EventLocation>Praia de Botafogo 518</a:EventLocation>
            <a:EventState>RJ</a:EventState>
            <a:FileId i:nil="true"/>
            <a:GiftCardId i:nil="true"/>
            <a:GiftListId>6</a:GiftListId>
            <a:GiftListMembers>
               <a:GiftListMemberDTO>
                  <a:GiftListMemberId>5</a:GiftListMemberId>
                  <a:GiftListId>6</a:GiftListId>
                  <a:ClientId i:nil="true"/>
                  <a:Title>Sr.</a:Title>
                  <a:Name>Hiran</a:Name>
                  <a:Surname>Azedo</a:Surname>
                  <a:Mail>hiran@vtex.com.br</a:Mail>
                  <a:Text1 i:nil="true"/>
                  <a:Text2 i:nil="true"/>
                  <a:IsAdmin>false</a:IsAdmin>
                  <a:IsActive>true</a:IsActive>
                  <a:ProfileSystemUserId/>
               </a:GiftListMemberDTO>
               <a:GiftListMemberDTO>
                  <a:GiftListMemberId>6</a:GiftListMemberId>
                  <a:GiftListId>6</a:GiftListId>
                  <a:ClientId i:nil="true"/>
                  <a:Title>Sra.</a:Title>
                  <a:Name>Eduarda</a:Name>
                  <a:Surname>Doce</a:Surname>
                  <a:Mail>eduarda@vtex.com.br</a:Mail>
                  <a:Text1 i:nil="true"/>
                  <a:Text2 i:nil="true"/>
                  <a:IsAdmin>false</a:IsAdmin>
                  <a:IsActive>true</a:IsActive>
                  <a:ProfileSystemUserId/>
               </a:GiftListMemberDTO>
            </a:GiftListMembers>
            <a:GiftListTypeId>2</a:GiftListTypeId>
            <a:Gifted i:nil="true"/>
            <a:IsActive>true</a:IsActive>
            <a:IsPublic>true</a:IsPublic>
            <a:Message>lindo, muito lindo o casal</a:Message>
            <a:Name>hiran e eduarda</a:Name>
            <a:ProfileSystemUserAddressName>casa</a:ProfileSystemUserAddressName>
            <a:ProfileSystemUserId>e41efd14-b230-42f4-950a-56cc7cc689e4</a:ProfileSystemUserId>
            <a:UrlFolder>hiraneeduarda</a:UrlFolder>
            <a:Version>2</a:Version>
         </GiftListInsertUpdateResult>
      </GiftListInsertUpdateResponse>
   </s:Body>
</s:Envelope>
```

###Inserir SKUs na Lista


Após criar a lista, agora é o momento de inserir a SKUs que formarão a lista.

Abaixo exemplos de chamada e resposta de inserção de SKU numa lista através do método "GiftListSkuInsert":

_request1:_  

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:GiftListSkuInsert>
         <tem:giftListSku>
            <vtex:GiftListId>6</vtex:GiftListId> <!--identificador da lista criada, GiftListId-->
	    	<vtex:InsertedByProfileSystemUserId>e41efd14-b230-42f4-950a-56cc7cc689e4</vtex:InsertedByProfileSystemUserId> <!--identificador do dono da lista-->
	    	<vtex:SkuId>2000002</vtex:SkuId> <!--identificador da SKU-->
            <vtex:WishedByProfileSystemUserId>e41efd14-b230-42f4-950a-56cc7cc689e4</vtex:WishedByProfileSystemUserId> <!--identificador do dono da lista-->
         </tem:giftListSku>
      </tem:GiftListSkuInsert>
   </soapenv:Body>
</soapenv:Envelope>
```

_response1_:  

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
   <s:Body>
      <GiftListSkuInsertResponse xmlns="http://tempuri.org/">
         <GiftListSkuInsertResult xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
            <a:DateCreated>2015-03-10T17:09:56.8390683</a:DateCreated>
            <a:DatePurchased i:nil="true"/>
            <a:FreightAndServicesValue i:nil="true"/>
            <a:GiftListId>6</a:GiftListId>
            <a:GiftListSkuId>13</a:GiftListSkuId>
            <a:InsertedByClientId>0</a:InsertedByClientId>
            <a:InsertedByProfileSystemUserId>e41efd14-b230-42f4-950a-56cc7cc689e4</a:InsertedByProfileSystemUserId>
            <a:ItemValue i:nil="true"/>
            <a:OmsOrderId i:nil="true"/>
            <a:OrderId i:nil="true"/>
            <a:OrderMessage i:nil="true"/>
            <a:OrderMessageFrom i:nil="true"/>
            <a:OrderMessageTo i:nil="true"/>
            <a:OrderResponseMessage i:nil="true"/>
            <a:SkuId>2000002</a:SkuId>
            <a:WishedByClientId i:nil="true"/>
            <a:WishedByProfileSystemUserId>e41efd14-b230-42f4-950a-56cc7cc689e4</a:WishedByProfileSystemUserId>
            <a:_IsOrderFinished i:nil="true"/>
         </GiftListSkuInsertResult>
      </GiftListSkuInsertResponse>
   </s:Body>
</s:Envelope>
```

_request2:_ 

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts">
   <soapenv:Header/>
   <soapenv:Body>
      <tem:GiftListSkuInsert>
         <tem:giftListSku>
            <vtex:GiftListId>6</vtex:GiftListId> <!--identificador da lista criada, GiftListId-->
	    	<vtex:InsertedByProfileSystemUserId>e41efd14-b230-42f4-950a-56cc7cc689e4</vtex:InsertedByProfileSystemUserId> <!--identificador do dono da lista-->
	    	<vtex:SkuId>2000002</vtex:SkuId> <!--identificador da SKU-->
            <vtex:WishedByProfileSystemUserId>e41efd14-b230-42f4-950a-56cc7cc689e4</vtex:WishedByProfileSystemUserId> <!--identificador do dono da lista-->
         </tem:giftListSku>
      </tem:GiftListSkuInsert>
   </soapenv:Body>
</soapenv:Envelope>
```

_response_:  

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
   <s:Body>
      <GiftListSkuInsertResponse xmlns="http://tempuri.org/">
         <GiftListSkuInsertResult xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
            <a:DateCreated>2015-03-10T14:03:30.9631177</a:DateCreated>
            <a:DatePurchased i:nil="true"/>
            <a:FreightAndServicesValue i:nil="true"/>
            <a:GiftListId>6</a:GiftListId>
            <a:GiftListSkuId>14</a:GiftListSkuId>
            <a:InsertedByClientId>0</a:InsertedByClientId>
            <a:InsertedByProfileSystemUserId i:nil="true"/>
            <a:ItemValue i:nil="true"/>
            <a:OmsOrderId i:nil="true"/>
            <a:OrderId i:nil="true"/>
            <a:OrderMessage i:nil="true"/>
            <a:OrderMessageFrom i:nil="true"/>
            <a:OrderMessageTo i:nil="true"/>
            <a:OrderResponseMessage i:nil="true"/>
            <a:SkuId>2000002</a:SkuId>
            <a:WishedByClientId i:nil="true"/>
            <a:WishedByProfileSystemUserId i:nil="true"/>
            <a:_IsOrderFinished i:nil="true"/>
         </GiftListSkuInsertResult>
      </GiftListSkuInsertResponse>
   </s:Body>
</s:Envelope>
```

---

autor: _Jonas Bolognim_  
propriedade: _VTEX_