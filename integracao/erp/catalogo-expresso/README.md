# Integração Rápida de Catálogo, Preço e Estoque

Este documento tem por objetivo auxiliar na integração de catálogo,preço e estoque do ERP para uma loja hospedada na versão smartcheckout da VTEX, de uma maneira rápida.

Nesse tipo de integração a administração da loja está no admin da VTEX, sendo o ERP apenas uma fonte de onde nascem os produtos e SKUs.

### Catalogo Fluxo Básico (Express)


Nesse cenário de fluxo básico, apenas os dados básicos de produtos e SKUs são manipulados pelo ERP, e todo o enriquecimento (marca, fornecedor, imagens, categoria, ativação, etc.) será feito pelo admin da loja na plataforma VTEX.

Para o ERP integrar se ao catálogo da loja na VTEX, deverá usar o webservice da própria loja, que por definição atenderá em [https://webservice-nomedaloja.vtexcommerce.com.br/service.svc?wsdl](https://webservice-nomedaloja.vtexcommerce.com.br/service.svc?wsdl "web service da loja"). As credenciais de acesso ao webservice deverão ser solicitadas junto ao administrador da loja.

Futuramente além do serviço SOAP (webservice) estaremos também oferecendo integração de catálogo por APIs REST (JSON) bem definidas e de alta performance.

### Organização dos Produtos Dentro da Loja


Geralmente, os produtos são organizados dentro da loja em estruturas mercadológicas formadas por:

1. **Departamento** - categoria cujo id de categoria pai é **nulo**,
2. **Categoria** - categoria cujo id de categoria pai é um **departamento**,
3. **SubCategoria**. categoria cujo id de categoria pai é um **categoria**

_Exemplo:_

* Departamento/Categoria/SubCategoria/Produto
* Ferramentas/Eletricas/Furadeiras/Super Drill

O cadastro da estrutura mercadologica deve ser feito diretamente no admin da própria loja (_http://sualoja.com.br/admin/Site/Categories.aspx_), e para atender a integração vinda do ERP, é criado um departamento padrão para produtos que vem do ERP, ou seja, todos os produtos caem no admin da loja nesse departamento padrão, e depois no momento do enriquecimento é colocado na categoria desejada.

**Marca**
A criação das marcas também deve ser feita pelo admin da VTEX. Para descida de produto do ERP, criar uma marca padrão, e depois no momento do enriquecimento, dentro do admin da VTEX, coloca na marca correta.

###Produtos e SKUs


> Qual é a diferença entre produto e SKU?

  O **Produto** é uma definição mais genérica de algo que é ofertado ao cliente, por exemplo, *Geladeira*, *Camiseta*, *Bola*.

  O **SKU** é uma sigla em inglês de "Stock Keeping Unit", em português Unidade de Manutenção de Estoque, ou seja, uma SKU define uma variação de um produto, por exemplo, *Geladeira Branca 110V*, *Camiseta Amarela Grande*

  No modelo de cadastro de Produtos e SKUs da VTEX, um SKU sempre será filho de um Produto (não existe SKU sem produto), mesmo que esse produto não tenha variações, e nesse caso será 1 SKU para 1 produto, por exemplo, produto *Bola Jabulani* com a *SKU Bola Jabulani*.

###Integração de Produtos e SKUs


Após definida as variações e a estrutura mercadológica da loja, o próximo passo é enviar os produtos e as SKUs do ERP para a loja VTEX.

_Fluxo:_

![alt text](ERP-catalogo-expresso.PNG "Fluxo Básico")

###Produto


Abaixo exemplo de chamada e resposta de uma inserção de produto usando o método "ProductInsertUpdate":

_request:_

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:arr="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
<soapenv:Header/>
	<soapenv:Body>
		<tem:ProductInsertUpdate>
			<tem:productVO>
			<!--int, identificdor da marca-->
			<vtex:BrandId>2000011</vtex:BrandId>
			<!--int, identificdor da categoria-->
			<vtex:CategoryId>1000020</vtex:CategoryId>
			<!--int, identificdor do departamento-->
			<vtex:DepartmentId>1000018</vtex:DepartmentId>
			<!--string, descrição completa do produto-->
			<vtex:Description>Vaso de barro vermelho, feito a mão com barro do mar vermelho</vtex:Description>
			<!--string, descrição curta do produto-->
			<vtex:DescriptionShort>Vaso de barro vermelho artesanal</vtex:DescriptionShort>
			<!--int, opcional, identificador no ERP caso int-->
            <vtex:Id>1234567</vtex:Id>
			<!--bool, se não atender requisitos de ativação(ter SKUs ativas), não será ativado-->
			<vtex:IsActive>true</vtex:IsActive>
			<!--bool, vai ser visível no site-->
			<vtex:IsVisible>true</vtex:IsVisible>
			<!--string, palavras chaves relevantes para a busca-->
			<vtex:KeyWords> Barro, vaso, vermelho</vtex:KeyWords>
			<!--lista de inteiros, pra qual canal de vendas = loja principal = 1-->
			<vtex:ListStoreId>
			   	<arr:int>1</arr:int>
				<arr:int>2</arr:int>
			</vtex:ListStoreId>
			<!--meta tag de description (SEO)-->
			<vtex:MetaTagDescription>feito a mão com barro do mar vermelho</vtex:MetaTagDescription>
			<!--string, nome do produto-->
			<vtex:Name>Vaso Artesanal de Barro Vermelho</vtex:Name>
			<!--string, identificador do produto no ERP-->
			<vtex:RefId>1234567890</vtex:RefId>
			<!--string, titulo do produto-->
			<vtex:Title>Vaso Artesanal de Barro Vermelho</vtex:Title>
			</tem:productVO>
		</tem:ProductInsertUpdate>
	</soapenv:Body>
</soapenv:Envelope>
```

_response:_

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
   <s:Body>
      <ProductInsertUpdateResponse xmlns="http://tempuri.org/">
         <ProductInsertUpdateResult xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
            <a:AdWordsRemarketingCode i:nil="true"/>
            <a:BrandId>2000011</a:BrandId>
            <a:CategoryId>1000020</a:CategoryId>
            <a:DepartmentId>1000018</a:DepartmentId>
            <a:Description>Vaso de barro vermelho, feito a mão com barro do mar vermelho</a:Description>
            <a:DescriptionShort>Vaso de barro vermelho artesanal</a:DescriptionShort>
			<!--identificador do produto inserido-->
            <a:Id>31018369</a:Id>
            <a:IsActive>false</a:IsActive>
            <a:IsVisible>true</a:IsVisible>
            <a:KeyWords>Barro, vaso, vermelho</a:KeyWords>
            <a:ListStoreId xmlns:b="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
               <b:int>1</b:int>
               <b:int>2</b:int>
            </a:ListStoreId>
            <a:LomadeeCampaignCode i:nil="true"/>
            <a:MetaTagDescription>feito a mão com barro do mar vermelho</a:MetaTagDescription>
            <a:Name>Vaso Artesanal de Barro Vermelho</a:Name>
            <a:RefId>1234567890</a:RefId>
            <a:ReleaseDate i:nil="true"/>
            <a:ShowWithoutStock>true</a:ShowWithoutStock>
            <a:SupplierId i:nil="true"/>
            <a:TaxCode i:nil="true"/>
            <a:Title>Vaso Artesanal de Barro Vermelho</a:Title>
         </ProductInsertUpdateResult>
      </ProductInsertUpdateResponse>
   </s:Body>
</s:Envelope>
```


###SKU


Uma vez inseridos todos os produtos, que teoricamente são os pais das SKUs, chegou o momento de enviar as SKUs filhas dos produtos.
Abaixo exemplo de chamada e resposta de uma inserção de SKU usando o metodo "StockKeepingUnitInsertUpdate":

_request:_

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tem="http://tempuri.org/" xmlns:vtex="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts">
<soapenv:Header/>
	<soapenv:Body>
		<tem:StockKeepingUnitInsertUpdate>
			<tem:stockKeepingUnitVO>
			<!--decimal, cubagem = peso cubico -->
			<vtex:CubicWeight>100</vtex:CubicWeight>
			<!--decimal, altura com embalagem e metros-->
			<vtex:Height>1.800</vtex:Height>
			<!--int, opcional, identificador da sku no ERP caso int-->
            <vtex:Id>00123456</vtex:Id>
			<!--bool, se não atender requisitos de ativação(imagem, preço, estoque, espec., outros reqs obrig.), não será ativado-->
			<vtex:IsActive>true</vtex:IsActive>
			<!--bool, disponibilidade -->
			<vtex:IsAvaiable>true</vtex:IsAvaiable>
			<!--bool, é um KIT? -->
			<vtex:IsKit>false</vtex:IsKit>
			<!--decimal, comprimento com embalagem em metros -->
			<vtex:Length>1.52</vtex:Length>
			<!--decimal, ** ler obs -->
			<vtex:ListPrice>150.0</vtex:ListPrice>
			<!--number, prefixo do estoque (id estoque = 1_1, mandar 1) -->
			<vtex:ModalId>1</vtex:ModalId>
			<!--string, opicional, tipo de carga, necessita configuração de transportadora especial quando preenchido -->
			<vtex:ModalType>Vidro</vtex:ModalType>
			<!--string, nome da SKU -->
			<vtex:Name>Vaso Artesanal de Barro Vermelho Escuro </vtex:Name>
			<!--decimal,  preço, "POR", opcional **ler obs-->
			<vtex:Price>110.0</vtex:Price>
			<!--int, identificador do produto pai da SKU -->
			<vtex:ProductId>31018369</vtex:ProductId>
			<!--decimal, peso real em kilos -->
			<vtex:RealHeight>1.740</vtex:RealHeight>
			<!--decimal, altural real em metros -->
			<vtex:RealLength>1.60</vtex:RealLength>
			<!--decimal, peso real em kilos -->
			<vtex:RealWeightKg>2.750</vtex:RealWeightKg>
			<!--decimal, comprimento real em metros -->
			<vtex:RealWidth>1.7</vtex:RealWidth>
			<!--string, identificador da SKU no ERP -->
			<vtex:RefId>00123456</vtex:RefId>
			<!--number, opcional, numero de pontos dessa SKU -->
			<vtex:RewardValue>0</vtex:RewardValue>
			<!--lista de string, EANs do produto -->
			<vtex:StockKeepingUnitEans>
			   <vtex:StockKeepingUnitEanDTO>
			   		<vtex:Ean>0123456789123</vtex:Ean>
			   </vtex:StockKeepingUnitEanDTO>
			</vtex:StockKeepingUnitEans>
			<!--int, unidade de multipliçao para venda -->
			<vtex:UnitMultiplier>1</vtex:UnitMultiplier>
			<!--decimal, peso em kilos-->
			<vtex:WeightKg>1.5</vtex:WeightKg>
			<!--decimal, largura com embalagem em metros -->
			<vtex:Width>2.780</vtex:Width>
			</tem:stockKeepingUnitVO>
		</tem:StockKeepingUnitInsertUpdate>
	</soapenv:Body>
</soapenv:Envelope>
```

_response:_

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
   <s:Body>
      <StockKeepingUnitInsertUpdateResponse xmlns="http://tempuri.org/">
         <StockKeepingUnitInsertUpdateResult xmlns:a="http://schemas.datacontract.org/2004/07/Vtex.Commerce.WebApps.AdminWcfService.Contracts" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
            <a:CommercialConditionId i:nil="true"/>
            <a:CostPrice>1</a:CostPrice>
            <a:CubicWeight>100</a:CubicWeight>
            <a:DateUpdated>2014-10-29T19:03:17.718427</a:DateUpdated>
            <a:EstimatedDateArrival i:nil="true"/>
            <a:Height>1.800</a:Height>
			<!--identificador do sku inserido-->
            <a:Id>31018371</a:Id>
            <a:InternalNote i:nil="true"/>
            <a:IsActive>false</a:IsActive>
            <a:IsAvaiable>false</a:IsAvaiable>
            <a:IsKit>false</a:IsKit>
            <a:Length>1.52</a:Length>
            <a:ListPrice>150.0</a:ListPrice>
            <a:ManufacturerCode i:nil="true"/>
            <a:MeasurementUnit>un</a:MeasurementUnit>
            <a:ModalId>1</a:ModalId>
            <a:ModalType>Vidro</a:ModalType>
            <a:Name>Vaso Artesanal de Barro Vermelho Escuro</a:Name>
            <a:Price>110.0</a:Price>
            <a:ProductId>31018369</a:ProductId>
            <a:ProductName>Vaso Artesanal de Barro Vermelho</a:ProductName>
            <a:RealHeight>1.740</a:RealHeight>
            <a:RealLength>1.60</a:RealLength>
            <a:RealWeightKg>2.750</a:RealWeightKg>
            <a:RealWidth>17</a:RealWidth>
            <a:RefId>00123456</a:RefId>
            <a:RewardValue>0</a:RewardValue>
            <a:StockKeepingUnitEans>
               <a:StockKeepingUnitEanDTO>
                  <a:Ean>0123456789123</a:Ean>
               </a:StockKeepingUnitEanDTO>
            </a:StockKeepingUnitEans>
            <a:UnitMultiplier>1</a:UnitMultiplier>
            <a:WeightKg>2.780</a:WeightKg>
            <a:Width>1.550</a:Width>
         </StockKeepingUnitInsertUpdateResult>
      </StockKeepingUnitInsertUpdateResponse>
   </s:Body>
</s:Envelope>
```

**Obersevação:**
O preço da SKU pode NÂO ser enviado no momento da inserção da SKU. Quando um preço não é enviado no momento da criação de uma SKU, na tabela de SKU por obrigatoriedade é criado um preço fictício de 99999.00, e no sistema de "Pricing" da VTEX não é inserido o preço.

###Preço e Estoque


Uma vez cadastradas os produtos e as SKUs na loja da VTEX, é necessário alimentar o estoque e acertar o preço na tabela de preço (se no momento de inserir a SKU não enviou o preço).


###Preço


Se no momento da inserção da SKU não foi enviado um preço válido para a SKU é necessário inserir o preço da mesma. Isso pode ser feito direto no admin da loja na VTEX (_urldaloja/admin/Site/SkuTabelaValor.aspx_), ou usando a API REST do sistema de **Pricing**.


Através da API do Pricing, inserir ou atualizar preço na SKUs:

<a title="inserir ou atualizar preço na SKUs" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/PRICING/PRICING_Set" target="_blank">[Developer] - Exemplo de chamada para inserir ou atualizar preço nas SKUs</a>


A documentação completa sobre a API de **Pricing** se encontra em: [http://lab.vtex.com/docs/pricing/api/latest/pricing/index.html](http://lab.vtex.com/docs/pricing/api/latest/pricing/index.html)

###Estoque

Isso pode ser feito direto no admin da loja na VTEX (_urldaloja/admin/logistics/#/dashboard_), maneira rápida:

1. Criar a transportadora - <a title="criar a transportadora" href="http://www.youtube.com/watch?v=IkIM53OXvVo" target="_blank">[Video]</a>,
2. Criar a doca <a title="criar a doca" href="https://www.youtube.com/watch?v=fNJ3JBoEoW0" target="_blank">[Video]</a>,
3. Criar o estoque - <a title="criar o estoque" href="http://www.youtube.com/watch?v=FijZQoaWiDM" target="_blank">[Video]</a>.


Criar o estoque, criar a transportadora e criar a doca no admin da VTEX, e depois usar a API REST do **Logistics** para manipular o estoque.


Através da API do Logistics, inserir ou atualizar os estoques na SKUs:

<a title="inserir ou atualizar os estoques na SKUs" href="http://bridge.vtexlab.com.br/vtex.bridge.web_deploy/swagger/ui/index.html#!/LOGISTIC/LOGISTIC_SetBalance" target="_blank">[Developer] - Exemplo de chamada para inserir ou atualizar estoque nas SKUs</a>

A documentação completa sobre a API de **Logistics** se encontra em: [http://lab.vtex.com/docs/logistics/api/latest/warehouse/index.html](http://lab.vtex.com/docs/logistics/api/latest/warehouse/index.html)

###Pedidos
Para a integraão de pedidos consulte o tópico [[Guide] Integração de Pedido, Nota Fiscal e Tracking](http://lab.vtex.com/docs/integracao/guide/erp/pedido-e-tracking/index.html).

---

autor:_Jonas Bolognim_
propriedade: _VTEX_
